Backup
-------

Provides interface for database backup and restore management, scheduling and reporting. Each backup will be assigned with a backup ID and ClusterControl creates a directory under *ClusterControl > Backup > Settings > Default Backup Directory* accordingly to store the backups based on this ID. On top of the page, you can see 4 function tabs followed by the created backup list underneath it.

Create Backup
`````````````

Creates a backup of the database immediately. You can choose to create a full backup using mysqldump or Percona Xtrabackup (fullbackup). Backups can be stored on the database host that is performing the backup, or the files can be streamed to the ClusterControl host. The backup created by this feature will be a full backup.

* **Hostname**
	- The target database host.

* **Backup Method**
	- mysqldump - Separated compressed schema and data dump. See `mysqldump`_ section.
	- xtrabackup (full) - A full compressed backup. See `Percona Xtrabackup`_ section.
	- NDB backup (for MySQL Cluster) - See `NDB backup (MySQL Cluster)`_ section.

* **Desync node during backup**
	- Exclusive for Galera. De-syncing a node with the highest local index before performing backup.
	
* **Backup Locks**
	- This option is only available if xtrabackup is selected. 
	- Yes - Uses "LOCK TABLES FOR BACKUP" where it supported when making a backup.
	- No - Sets ``--no-backup-locks`` which use ``FLUSH NO_WRITE_TO_BINLOG TABLES`` and ``FLUSH TABLES WITH READ LOCK`` when making backup.

* **Use Compression**
	- This option is only available if xtrabackup is selected.
	- Yes - Tells xtrabackup to compress all output data, including the transaction log file and meta data files.
	- No - Do not use compression for the backup.

* **Xtrabackup Parallel Copy Threads**
	- This option is only available if xtrabackup is selected.
	- This option specifies the number of threads to use to copy multiple data files concurrently when creating a backup. The default value is 1 (i.e., no concurrent transfer).

* **Use PIGZ for parallel gzip**
	- This option is only available if xtrabackup is selected.
	- Yes - Use PIGZ instead of standard gzip. This is helpful if you want to backup very large data set.
	- No - Use the standard gzip.	
	
* **Backup Location**
	- Store on Node - Store the backup inside corresponding database node.
	- Store on Controller - Store the backup inside ClusterControl node. This requires :term:`netcat` on source and destination host. By default, ClusterControl uses port 9999 to stream the backup created on the database node to ClusterControl node.

* **Backup Directory**
	- You can opt to use another backup directory as you wish. If you leave this field blank, the system will use the default backup directory specified in the *ClusterControl > Settings > General Settings > Backup*.
	
* **Netcat Port**
	- Specify the port number that will be used by ClusterControl to stream backup created on the database node. This port must be opened on both source and destination hosts. Only available if you choose *Store on Controller* in *Backup Location*.
	
* **Databases**
	- List of databases retrieved from the monitored MySQL servers. Default is 'All Databases'.

Schedule Backup
```````````````

Creates backup schedules of the database. You can choose to create a full or incremental backup using mysqldump (full backup only) or xtrabackup. 

* **Schedule**
	- Sets the backup schedule.

.. Note:: The time is the local time on ClusterControl node.

* **Backup Directory**
	- Enter a backup directory or use default path provided by ClusterControl under *ClusterControl > Settings > Backup*.

* **Backup Method**
	- mysqldump - Separated compressed schema and data dump. See `mysqldump`_ section.
	- xtrabackup (full) - A full compressed backup. See `Percona Xtrabackup`_ section.
	- xtrabackup (incr) - An incremental compressed backup. See `Percona Xtrabackup`_ section.
	- NDB backup (for MySQL Cluster) - See `NDB backup (MySQL Cluster)`_ section.

* **Backup Locks**
	- This option is only available if xtrabackup is selected. 
	- Yes - Uses "LOCK TABLES FOR BACKUP" where it supported when making a backup.
	- No - Sets ``--no-backup-locks`` which use "FLUSH NO_WRITE_TO_BINLOG TABLES" and "FLUSH TABLES WITH READ LOCK" when making backup.

* **Use Compression**
	- This option is only available if xtrabackup is selected.
	- Yes - Tells xtrabackup to compress all output data, including the transaction log file and meta data files.
	- No - Do not use compression for the backup.

* **Xtrabackup Parallel Copy Threads**
	- This option is only available if xtrabackup is selected.
	- This option specifies the number of threads to use to copy multiple data files concurrently when creating a backup. The default value is 1 (i.e., no concurrent transfer).

* **Use PIGZ for parallel gzip**
	- This option is only available if xtrabackup is selected.
	- Yes - Use PIGZ instead of standard gzip. This is helpful if you want to backup very large data set.
	- No - Use the standard gzip.
	
* **Backup Host**
	- Host to run the backup command. Choose "Auto Select" to allow ClusterControl to automatically select which node to take the backup on.

* **Backup Location**
	- Supported backup locations:
		- Store on Node - Store the backup inside corresponding database node.
		- Store on Controller - Store the backup inside ClusterControl node. This requires :term:`netcat` on source and destination host. By default, ClusterControl uses port 9999 to stream the backup created on the database node to ClusterControl node.

* **Netcat Port**
	- Specify the port number that will be used by ClusterControl to stream backup created on the database node. This port must be opened on both source and destination hosts. Only available if you choose *Store on Controller* in *Backup Location*.

* **Databases**
	- List of databases retrieved from the monitored MySQL servers. Default is 'All Databases'.
  
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

This section explains backup method used by ClusterControl.

.. Note:: Backup process performed by ClusterControl is running on a background thread (RUNNING3) which doesn't block any other non-backup jobs in queue. If the backup job takes hours to complete, other non-backup jobs can still run simultaneously via the main thread (RUNNING). You can see the job progress at *ClusterControl > Logs > Jobs*.

mysqldump
.........

ClusterControl performs :term:`mysqldump` against all or selected databases by using the ``--single-transaction`` option. It automatically performs mysqldump with ``--master-data=2`` if it detects binary logging is enabled on the particular node to generate binary log file and position statement in the dump file. ClusterControl generates a set of 4 mysqldump files with the following suffixes:

* _data.sql.gz - Schemas’ data.
* _schema.sql.gz - Schemas’ structure.
* _mysqldb.sql.gz - MySQL system database.
* _triggerseventroutines.sql.gz - MySQL triggers, event and routines.


Percona Xtrabackup
..................

Percona Xtrabackup is an open-source MySQL hot backup utility from Percona. It is a combination of :term:`xtrabackup` (built in C) and :term:`innobackupex` (built on Perl) and can back up data from InnoDB, :term:`XtraDB` and :term:`MyISAM` tables. Xtrabackup does not lock your database during the backup process. For large databases (100+ GB), it provides much better restoration time as compared to mysqldump. The restoration process involves preparing MySQL data from the backup files before replacing or switching it with the current data directory on the target node.

Since its ability to create full and incremental MySQL backups, ClusterControl manages incremental backups, and groups the combination of full and incremental backups in a backup set. A backup set has an ID based on the latest full backup ID. All incremental backups after a full backup will be part of the same backup set. The backup set can then be restored as one single unit using `Restore Backup`_ feature.

.. Attention:: Without a full backup to start from, the incremental backups are useless.

NDB backup (MySQL Cluster)
..........................

NDB backup triggers ``START BACKUP`` command on management node and perform mysqldump on each of the SQL nodes subsequently. These backup files will be created and streamed to ClusterControl node based on *ClusterControl > Settings > Backup > Backup Directory* location.

Backup List
````````````

Provides a list of finished backup jobs. The status can be:

========= ===========
Status    Description
========= ===========
Completed Backup was successfully created and stored in the chosen node.
Running   Backup process is running.
Failed    Backup was failed.
========= ===========

All incremental backups are automatically grouped together under the last full backup and expandable with a drop down.

* **Restore**
	- See `Restore Backup`_.

* **Log**
	- Shows the output when ClusterControl executed the backup job.

* **Delete**
	- Removes the backup set. If you remve the backup set, all incremental backups associated with it will be removed as well.

Restore Backup
..............

Restores backup (mysqldump and xtrabackup) created by ClusterControl. You can restore up to a certain incremental backup by clicking on the *Restore* button for the respective backup ID. The following steps will be performed:

For mysqldump (online restore):

1. Copy backup files to the target server.
2. Checking disk space on the target server.
3. The mysqldump files will be copied to the node.
4. The schema, data and triggers/functions dump files are applied.
5. Optionally restore the `mysql` database. If the `cmon` user privileges has changed it may cause ClusterControl to stop functioning. This is fixable of course.
6. The rest of the member will then catch up with the target server.

For Percona Xtrabackup (offline restore):

1. Stop all nodes in the cluster.
2. Copy backup files to the target server.
3. Checking disk space on the target server.
4. Prepare and restore the backup.
5. Follow the instruction in the *ClusterControl > Logs > Job > Job Message* on how to bootstrap the cluster. Alternatively, you can toggle on *Bootstrap cluster from the restored node*.

.. Attention:: ClusterControl does not support restoring a partial backup created by xtrabackup. The restoration requires you to manually export and import tablespace into a running MySQL server. Please refer to Percona Xtrabackup documentation before performing this exercise.

* **Restore backup on**
	- The backup will be restored to the selected server.
	
* **Tmp Dir**
	- Temporary storage for ClusterControl to prepare the big. It must be as big as the expected MySQL data directory.

* **Bootstrap cluster from the restored node?**
	- Toggle to ON if you want ClusterControl to automatically re-bootstrap the cluster on the restored node.

* **Make a copy of the datadir before restoring the backup**
	- Toggle to ON to keep the old MySQL datadir before replacing the datadir with the prepared backup.
	
.. Attention:: The datadir must have enough space to accomodate the restored backup.

* **Restore "MySQL" Database**
	- Exclusive to mysqldump. Toggle to ON to restore the ``mysql`` database if the backup was created by ClusterControl. If the ``cmon`` user privileges has changed, it may cause ClusterControl to stop functioning. This is fixable, of course. Default is "No".
	
Restore External Backups
........................

Restores external backups which does not listed in the `Backup List`_. It could be a backup created by another ClusterControl instance or the backup was created by user. 

.. Attention:: An external backup must contain privileges allowing the database user 'cmon' to connect to the MySQL server or all Galera nodes, or else ClusterControl may not be able to connect and monitor/manage the database nodes.

The following steps will be performed:

1. Stop all nodes in the cluster.
2. Copy backup files to the selected server.
3. Restore the backup.
4. Follow the instruction in the *ClusterControl > Logs > Job > Job Message* on how to bootstrap the cluster.

.. Note:: Only ``xbstream``, ``xbstream.gz`` and ``.tar.gz`` extensions are supported. Do prepare your external backup with one of these extensions beforehand.

* **Restore backup on**
	- The backup will be restored to the selected node.

* **Backup Method**
	- How the backup was created, either mysqldump or xtrabackup.

* **Backup Path**
	- The backup file path on ClusterControl node.

* **Tmp Dir**
	- Temporary storage for ClusterControl to prepare the big. It must be as big as the expected MySQL data directory.
	
* **Bootstrap cluster from the restored node?**
	- Toggle to ON if you want ClusterControl to automatically re-bootstrap the cluster on the restored node.

* **Make a copy of the datadir before restoring the backup**
	- Toggle to ON to keep the old MySQL datadir before replacing the datadir with the prepared backup.
	
.. Attention:: The datadir must have enough space to accomodate the restored backup.

* **Restore to Database**
	- Exclusive to mysqldump. Restores the backup to a specific database name. Leave blank if you want it to restore as it is.

* **Restore "MySQL" Database**
	- Exclusive to mysqldump. Toggle to ON to restore the ``mysql`` database if the backup was created by ClusterControl. If the ``cmon`` user privileges has changed, it may cause ClusterControl to stop functioning. This is fixable, of course. Default is "No".