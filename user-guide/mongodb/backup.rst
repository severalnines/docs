Backup
-------

Provides interface for database backup management, scheduling and report. Each backup will be assigned with a backup ID and ClusterControl creates a directory under *ClusterControl > Settings > Backup > Backup Directory* accordingly to store the backups based on this ID.

Create Backup
`````````````

Creates a backup of the database immediately. You can choose to create a full backup using :term:`mongodump` or MongoDB consistent backup. The backup created by this feature will be a full backup.

* **Backup Method**
	- mongodump - See `mongodump`_ section.
	- mongodb consistent - See `mongodb consistent`_ section.

* **Storage Directory**
	- Enter a backup directory or use default path provided by ClusterControl under *ClusterControl > Settings > Backup*. ClusterControl organizes backup directory according to shard or replica set name, under the *Backup Directory* on the ClusterControl host.
	

Schedule Backup
```````````````

Creates backup schedules of the database. You can choose to create a full backup using mongodump or mongodb-consistent-backup. ClusterControl organizes backup directory according to shard or replica set, under the *Backup Directory* on the ClusterControl host.

* **Schedule**
	- Sets the backup schedule.

.. Note:: The time is the local time on ClusterControl node.

* **Backup Method**
	- mongodump - See `mongodump`_ section.
	- mongodb consistent - See `mongodb consistent`_ section.

* **Storage Directory**
	- Enter a backup directory or use default path provided by ClusterControl under *ClusterControl > Settings > Backup*. ClusterControl organizes backup directory according to shard or replica set name, under the *Backup Directory* on the ClusterControl host.
  
* **Failover backup if node is down**
	- Yes - Backup will be run on any available node (or selected node based on the *Backup Failover Host*) if the target database node is down. If failover is enabled and the selected node is not online, the backup job elects an online node to create the backup. This ensures that a backup will be created even if the selected node is not available. If the scheduled backup is an incremental backup and a full backup does not exist on the new elected node, then a full backup will be created.
	- No - Backup will not run if the target database node is down.
	
* **Backup Failover Host**
	- List of database host to failover in case the target node is down during the scheduled backup.
  
Scheduled backups
`````````````````

List of scheduled backups. You can enable and disable the schedule by toggling it accordingly. The created schedule can only be deleted and cannot be modified.

Backup Method
`````````````

This section explains backup method used by ClusterControl for MongoDB.

.. Note:: Backup process performed by ClusterControl is running on a background thread (RUNNING3) which doesn't block any other non-backup jobs in queue. If the backup job takes hours to complete, other non-backup jobs can still run simultaneously via the main thread (RUNNING). You can see the job progress at *ClusterControl > Logs > Jobs*.

mongodump
.........

ClusterControl uses the standard command to perform :term:`mongodump` with ``--journal`` option, which allows mongodump operations to use the durability journal to ensure that the export is in a consistent state against shards.

mongodb consistent
...................

Built on top of Python 2.7, also known as :term:`mongodb-consistent-backup`, it creates cluster-consistent point-in-time backups of MongoDB via wrapping :term:`mongodump`. Backups are remotely-pulled and outputted onto the host running the tool. Even if you do not run MongoDB 3.2+, it is strongly recommended to use MongoDB 3.2+ binaries due to inline compression and parallelism.

Backup List
````````````

Provides a list of finished backup jobs. The status can be:

========= ===========
Status    Description
========= ===========
completed Backup was successfully created and stored in the chosen node.
running   Backup process is running.
failed    Backup was failed.
========= ===========
