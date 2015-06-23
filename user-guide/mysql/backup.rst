Backup
-------

Provides interface for database backup and restore management, scheduling, report and off-site backup. Each backup will be assigned with a backup ID and ClusterControl creates a directory under *ClusterControl > Settings > Backup > Backup Directory* accordingly to store the backups based on this ID.

Backup Immediately
``````````````````

Creates an immediate backup of the database. You can choose to create a full backup using mysqldump or Percona Xtrabackup. Backups can be stored on the database host that is performing the backup, or the files can be streamed to the ClusterControl host. The backup by this feature will be a full backup. 

* **Hostname**
	- The target database host.

* **Backup Method**
	- Supported backup method:
		- mysqldump - Separated compressed schema and data dump. See `mysqldump`_ section.
		- xtrabackup (full) - A full compressed backup. See `Percona Xtrabackup`_ section.
		- NDB backup (for MySQL Cluster) - See `NDB backup`_ section.

* **Backup Location**
	- Supported backup locations:
		- Store on Node - Store the backup inside corresponding database node.
		- Store on Controller - Store the backup inside ClusterControl node. This requires :term:`netcat` on source and destination host. By default, ClusterControl uses port 9999 to stream the backup created on the database node to ClusterControl node.

* **Backup Directory**
	- You can opt to use another backup directory as you wish. If you leave this field blank, the system will use the default backup directory specified in the *ClusterControl > Settings > General Settings > Backup*.
	
* **Netcat Port**
	- Specify the port number that will be used by ClusterControl to stream backup created on the database node. Only available if you choose *Store on Controller* in *Backup Location*.

.. Note:: You can check the backup status under *ClustreControl > Backup > Reports*.

Backup Schedule
```````````````

Creates backup schedules of the database. You can choose to create a full backup using mysqldump or xtrabackup. 

* **Start Time**
	- Backup start time.

.. note:: The time is UTC on the ClusterControl server.

* **Backup Directory**
	- Enter a backup directory or use default path provided by ClusterControl under *ClusterControl > Settings > Backup*.

* **Backup Method**
	- Available backup tools:
		- mysqldump - Separated compressed schema and data dump. See `mysqldump`_ section.
		- Full backup with xtrabackup - A full compressed backup. See `Percona Xtrabackup`_ section.
		- Incremental backup with xtrabackup - An incremental compressed backup. See `Percona Xtrabackup`_ section.
		- NDB backup (for MySQL Cluster) - See `NDB backup (MySQL Cluster)`_ section.

* **Backup Host**
	- Host to run the backup command.

* **Backup Location**
	- Supported backup locations:
		- Store on Node - Store the backup inside corresponding database node.
		- Store on Controller - Store the backup inside ClusterControl node. This requires :term:`netcat` on source and destination host. By default, ClusterControl uses port 9999 to stream the backup created on the database node to ClusterControl node.

* **Netcat Port**
	- Specify the port number that will be used by ClusterControl to stream backup created on the database node. Only available if you choose *Store on Controller* in *Backup Location*.

Current Backup Schedule
.......................

List of backup schedules. 

Backup Method
`````````````

This section explains backup method used by ClusterControl.

mysqldump
.........

ClusterControl performs :term:`mysqldump` against all databases by using the ``--single-transaction`` option. It automatically performs mysqldump with ``--master-data=2`` if it detects binary logging is enabled on the particular node to generate a statement about binary log file and position in the dump file. ClusterControl generates a set of three mysqldump files with the following suffixes:

* _data - All schemas’ data.
* _schema - All schemas’ structure.
* _mysqldb - MySQL system database.

The last output of the backup file would be a gunzip compressed file, ``.tar.gz`` consists of three ``.sql.gz`` files.

Percona Xtrabackup
..................

Xtrabackup is an open-source MySQL hot backup utility from Percona. It is a combination of :term:`xtrabackup` (built in C) and :term:`innobackupex` (built on Perl) and can back up data from InnoDB, :term:`XtraDB` and :term:`MyISAM` tables. Xtrabackup does not lock your database during the backup process. For large databases (100+ GB), it provides much better restoration time as compared to mysqldump. The restoration process involves preparing MySQL data from the backup files before replacing or switching it with the current data directory on the target node.

Since its ability to create full and incremental MySQL backups, ClusterControl manages incremental backups, and groups the combination of full and incremental backups in a backup set. A backup set has an ID based on the latest full backup ID. All incremental backups after a full backup will be part of the same backup set. The backup set can then be restored as one single unit using `Restore Backup`_ feature.

.. Attention:: Without a full backup to start from, the incremental backups are useless.

NDB backup (MySQL Cluster)
..........................

NDB backup triggers 'START BACKUP' command on management node and perform mysqldump on each of the SQL nodes subsequently. These backup files will be created and streamed to ClusterControl node based on *ClusterControl > Settings > Backup > Backup Directory* location.

Reports
```````

Backup Report provides a list of finished backup jobs. The status can be:

========= ===========
Status    Description
========= ===========
completed Backup was successfully created and stored in the chosen node.
running   Backup process is running.
failed    Backup was failed. For Xtrabackup, ClusterControl provides the backup log.
========= ===========

Restore Backup
..............

ClusterControl has ability to restore backups (mysqldump and xtrabackup) created by ClusterControl. The following steps will be performed:

1. Stop all nodes in the cluster.
2. Copy backup files to the selected server.
3. Restore the backup.
4. Follow the instruction in the *ClusterControl > Logs > Job > Job Message* on how to bootstrap the cluster.

* **Backup Id**
	- Selected backup ID. This is auto picked if you click the *Restore Backup* button.

* Restore backup on
	- The backup will be restored to the selected server.

Restore External Backups
........................

Restore external backups created by user independently. The following steps will be performed:

1. Stop all nodes in the cluster.
2. Copy backup files to the selected server.
3. Restore the backup.
4. Follow the instruction in the *ClusterControl > Logs > Job > Job Message* on how to bootstrap the cluster.

.. Note:: Only ``xbstream``, ``xbstream.gz`` and ``.tar.gz`` extensions are supported. Note to prepare your external backup with one of these extensions beforehand.

* **Restore backup on**
	- The backup will be restored to the selected node.

* **Backup Method**
	- How the backup was created, either mysqldump or xtrabackup.

* **Specify path to backup**
	- The backup file path on ClusterControl node.

Online Storage
``````````````

Manage off-site database backups to AWS S3 or Glacier. This feature is not available for MySQL Cluster.

Backups
.......

Choose one or more backup files and click *Upload to AWS/S3* button to start uploading.

* **Select SSH Key**
	- Select existing on-premises key (if exists).

* **Add Key**
	- Open On-premises Credentials window to manage the SSH key. ClusterControl uses this key to access the node and retrieve the backup file. You can upload the same SSH key as specified at Settings > General Settings > SSH Identity.

* **AWS Key Pair**
	- Select existing AWS key pair (if exists).

* **Add AWS Key**
	- Open AWS Credentials window to manage your AWS key pair. ClusterControl uses this key to upload the backup to the chosen destination.

* **Upload to**
	- Choose the upload destination:
		- AWS S3 - Amazon Simple Storage Service.
		- AWS Glacier - A reliable, secure, and inexpensive service to backup and archive data. If you choose this option, you need to specify the AWS region for Glacier.

* **Upload backup as**
	- If you choose more than one backup files to upload, ClusterControl is able to upload them all separately or in a single tarball.

S3/Glacier Backups
..................

Retrieve backups from S3 and Glacier. From here, you can delete the selected backup remotely.

Glacier Jobs
............

Lists Glacier Jobs for a vault including jobs that are in-progress initiated by ClusterControl.