Backup
-------

Provides interface for database backup management, scheduling and report. Each backup will be assigned with a backup ID and ClusterControl creates a directory under *ClusterControl > Settings > Backup > Backup Directory* accordingly to store the backups based on this ID.

Backup Immediately
``````````````````

Creates an immediate full backup of the database using mongodump. Backups is created and stored locally on ClusterControl node. Therefore, ensure the MongoDB client (same version with shards) is installed on the ClusterControl node before proceed with a backup.

* **Backup Method**
	- Supported backup method:
		- mongodump - Separated compressed schema and data dump. See `mongodump`_ section.

* **Backup Directory**
	- Specify the backup path. Backup will be stored on ClusterControl node.

.. Note:: You can check the backup status under *ClustreControl > Backup > Reports*.

Backup Schedule
```````````````

Creates backup schedules of the database.

* **Start Time**
	- Backup start time.

.. note:: The time is UTC on the ClusterControl server.

* **Backup Directory**
	- Specify the backup directory.

* **Backup Method**
	- Available backup tools:
		- mongodump - Separated compressed schema and data dump. See `mongodump`_ section.

Current Backup Schedule
.......................

List of backup schedules. 

Backup Method
`````````````

This section explains backup method used by ClusterControl for MongoDB.

mongodump
.........

ClusterControl performs :term:`mongodump` locally on ClusterControl node. Therefore, ensure the MongoDB client (same version with shards) is installed on the ClusterControl node before proceed with a backup. On Redhat/CentOS, you can simply run following command to install it (provided MongoDB repository is configured):

.. code-block:: bash

	yum install -y mongodb-org-shell mongodb-org-tools


ClusterControl uses the standard command to perform :term:`mongodump` with ``--journal`` option, which allows mongodump operations to use the durability journal to ensure that the export is in a consistent state against shards. ClusterControl organizes backup directory according to database name, under the *Backup Directory*.

Reports
```````

Backup Report provides a list of finished backup jobs. The status can be:

========= ===========
Status    Description
========= ===========
completed Backup was successfully created and stored in the chosen node.
running   Backup process is running.
failed    Backup was failed. For mongodump, ClusterControl provides the backup log.
========= ===========
