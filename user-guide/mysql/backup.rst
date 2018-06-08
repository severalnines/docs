Backup
-------

Provides interface for database backup and restore management, scheduling and reporting. Each backup is assigned with a backup ID and ClusterControl creates a directory under *Storage Directory* to store the backups based on this ID.

Create Backup
+++++++++++++

Creates or schedules a MySQL backup. 

Create Backup
``````````````

To create an instant backup, you have options to create a full backup using mysqldump, Percona Xtrabackup or MariaDB Backup. Incremental backup using Percona Xtrabackup and MariaDB Backup (MariaDB only) is only available under `Schedule Backup`_. Backups can be stored on the database host that performs the backup, or the files can be streamed over to the ClusterControl host for centralized storage.

* **Backup Method**
	- mysqldump - Separated compressed schema and data dump. See `mysqldump`_.
	- xtrabackup (full) - A full compressed backup. See `Percona Xtrabackup`_.
	- mariabackup (full) - A full compressed backup. See `MariaDB Backup`_.
	- NDB backup (for MySQL Cluster) - See `NDB backup (MySQL Cluster)`_.

* **Dump Type**
	- Exclusive for mysqldump only. Choose between schema only (``--no-data``), data only (``--no-create-info``) or schema and data only.

* **Backup Host**
	- The target database node.

* **Databases**
	- List of databases retrieved from the monitored MySQL servers. Default is 'All Databases'. Otherwise, specify the databases that you would like to backup for this set.

* **Tables**
	- Exclusive for mysqldump only. List of tables retrieved from the monitored MySQL servers. Default is 'All Tables'. Otherwise, specify the table that you would like to backup for this set. Use *Include tables* and *Exclude tables* for additional condition to the table list.

* **Create one dumpfile for each database**
	- Exclusive for mysqldump only. Each chosen database will have its own dumpfile.
	
* **Storage Location**
	- Store on Node - Stores the backup inside corresponding database node.
	- Store on Controller - Stores the backup inside ClusterControl node. This requires :term:`netcat` on source and destination host. By default, ClusterControl uses port 9999 to stream the backup created on the database node to ClusterControl node.

* **Storage Directory**
	- You can opt to use another backup directory as you wish. If you leave this field blank, ClusterControl will use the default backup directory specified in the *Settings > Default backup directory*.

* **Backup Subdirectory**
	- Set the name of the backup subdirectory. This string may hold standard ``%X`` field separators, the ``%06I`` for example will be replaced by the numerical ID of the backup in 6 field wide format that uses '0' as leading fill characters. Default value: ``BACKUP-%I``.

	========= ===================
	Variable  Description
	========= ===================
	B         The date and time when the backup creation was beginning.
	H         The name of the backup host, the host that created the backup.
	i         The numerical ID of the cluster.
	I         The numerical ID of the backup.
	J         The numerical ID of the job that created the backup.
	M         The backup method (e.g. "mysqldump").
	O         The name of the user who initiated the backup job.
	S         The name of the storage host, the host that stores the backup files.
	%         The percent sign itself. Use two percent signs, ``%%`` the same way the standard ``printf()`` function interprets it as one percent sign.
	========= ===================

* **Netcat Port**
	- Specify the port number that will be used by ClusterControl to stream backup created on the database node. This port must be opened on both source and destination hosts. Only available if you choose *Store on Controller* in *Storage Location*.
	
* **Use Compression**
	- Yes - Tells the chosen backup method to compress all output data, including the transaction log file and meta data files.
	- No - Do not use compression for the backup.

* **Compression Level**
	- Specify the compression level for the backup. This is according to :term:`gzip` compression level. 1 is the fastest compression (least compression) and 9 indicates the slowest compression (best compression).

* **Use Extended Insert**
	- Exclusive for mysqldump only. The default is ON where ClusterControl will create the dump with with ``--extended-insert``. This results in a smaller dump file and speeds up inserts when the file is reloaded. If toggled OFF, ClusterControl will use ``--skip-extended-insert`` and ``--set-gtid-purged=OFF`` flags in the mysqldump command.

* **PITR Compatible**
	- Exclusive for mysqldump and if binary log is enabled. A mysqldump PITR-compatible backup contains one single dump file, with GTID info, binlog file and position. Thus, only the database node that produces binary log will have the "PITR Compatible" option available. See `Point-in-Time Recovery`_.

* **Upload Backup to cloud**
	- Automatically upload the finished backup to AWS S3 or Google Cloud Storage. This backup can then be downloaded and restored from the cloud. You have to configure `Cloud Credentials <../index.html#cloud-providers>`_ beforehand. 

* **Backup Individual Schema**
	- Exclusive for mysqldump. Each selected databases is backed up individually, in its own directory in the storage directory.

* **Enable Encryption**
	- Encrypts the generated backup. Backup is encrypted at rest using AES-256 CBC algorithm, where the encryption key will be created automatically. If you choose to store the backup on the controller node, the backup files are transferred in encrypted format through :term:`socat` or :term:`netcat`.

* **Retention**
	- How long ClusterControl should keep this backup once successfully created. You can set a custom period in days or keep it forever. Otherwise, ClusterControl will use the default retention period.

* **Desync node during backup**
	- Exclusive for Galera and xtrabackup/mariabackup. De-syncing a node before performing backup, which disables Flow Control for the node. The node continues to receive write-sets and fall further behind the cluster. The cluster does not wait for desynced nodes to catch up, even if it reaches the ``fc_limit`` value.
	
* **Backup Locks**
	- Exclusive for xtrabackup/mariabackup.
	- Yes - Uses ``LOCK TABLES FOR BACKUP`` where it supported when making a backup.
	- No - Sets ``--no-backup-locks`` which use ``FLUSH NO_WRITE_TO_BINLOG TABLES`` and ``FLUSH TABLES WITH READ LOCK`` when making backup.

* **Xtrabackup Parallel Copy Threads**
	- Exclusive for xtrabackup/mariabackup. This option specifies the number of threads to use to copy multiple data files concurrently when creating a backup. The default value is 1 (i.e., no concurrent transfer).

* **Xtrabackup Throttle Rate (IOPS)**
	- Exclusive for xtrabackup/mariabackup. Use ``--throttle`` flag to enable disk :term:`IOPS` throttling. 0 means disabled. This might be helpful on systems that do not have much spare I/O capacity.
	
* **Network Streaming Throttle Rate (MB/s)**
	- Exclusive for xtrabackup/mariabackup and only if the storage location is the controller. Throttle the backup streaming process using a tool called :term:`pv`. 0 means disabled.

* **Use PIGZ for parallel gzip**
	- Exclusive for xtrabackup/mariabackup. 
	- Yes - Use PIGZ instead of standard gzip. This is helpful if you want to backup very large data set.
	- No - Use the standard gzip.	


Schedule Backup
................

Creates backup schedules of the database. You can choose to create a full or incremental backup using xtrabackup, mariabackup or mysqldump. 

* **Schedule**
	- Simple - Default scheduling option. This translates to the same output as the *Advanced* editor.
	- Advanced - Opens a cron-like editor. Formatting is similar to the standard :term:`cron`.

.. Note:: The backup time is in UTC time zone of the ClusterControl node.

* **Backup Method**
	- mysqldump - Separated compressed schema and data dump. See `mysqldump`_.
	- xtrabackup (full) - A full compressed backup. See `Percona Xtrabackup`_.
	- xtrabackup (incr) - An incremental compressed backup. See `Percona Xtrabackup`_.
	- mariabackup (full) - A full compressed backup. See `MariaDB Backup`_.
	- mariabackup (incr) - An incremental compressed backup. See `MariaDB Backup`_.
	- NDB backup (for MySQL Cluster) - See `NDB backup (MySQL Cluster)`_.

.. Note:: If you pick incremental backup as the backup method, ClusterControl will look for a full backup (parent backup) and will automatically revert to full backup if it can't find any.

* **Dump Type**
	- Exclusive for mysqldump only. Choose between schema only (``--no-data``), data only (``--no-create-info``) or schema and data only.

* **Backup Host**
	- The target database node.

* **Databases**
	- List of databases retrieved from the monitored MySQL servers. Default is 'All Databases'. Otherwise, specify the databases that you would like to backup for this set.

* **Tables**
	- Exclusive for mysqldump only. List of tables retrieved from the monitored MySQL servers. Default is 'All Tables'. Otherwise, specify the table that you would like to backup for this set. Use 'Include tables' or 'Exclude tables' condition to filter out the table list.

* **Create one dumpfile for each database**
	- Exclusive for mysqldump only. Each chosen database will have its own dumpfile.

* **Storage Location**
	- Store on Node - Stores the backup inside the database node.
	- Store on Controller - Stores the backup inside ClusterControl node. This requires :term:`socat` or :term:`netcat` on source and destination host. By default, ClusterControl uses port 9999 to stream the backup created on the database node to ClusterControl node.

* **Storage Directory**
	- You can opt to use another backup directory as you wish. If you leave this field blank, ClusterControl will use the default backup directory specified in the *Settings > Default backup directory*.

* **Backup Subdirectory**
	- Set the name of the backup subdirectory. This string may hold standard ``%X`` field separators, the ``%06I`` for example will be replaced by the numerical ID of the backup in 6 field wide format that uses '0' as leading fill characters. Default value: ``BACKUP-%I``.

	========= ===================
	Variable  Description
	========= ===================
	B         The date and time when the backup creation was beginning.
	H         The name of the backup host, the host that created the backup.
	i         The numerical ID of the cluster.
	I         The numerical ID of the backup.
	J         The numerical ID of the job that created the backup.
	M         The backup method (e.g. "mysqldump").
	O         The name of the user who initiated the backup job.
	S         The name of the storage host, the host that stores the backup files.
	%         The percent sign itself. Use two percent signs, ``%%`` the same way the standard ``printf()`` function interprets it as one percent sign.
	========= ===================

* **Netcat Port**
	- Specify the port number that will be used by ClusterControl to stream backup created on the database node. This port must be opened on both source and destination hosts. Only available if you choose *Store on Controller* in *Storage Location*.

* **Use Compression**
	- Yes - Use compression for the backup. Compression happens on the backup node.
	- No - Do not use compression for the backup.

* **Compression Level**
	- Specify the compression level for the backup. This is according to :term:`gzip` compression level. 1 is the fastest compression (least compression) and 9 is the slowest compression (best compression).

* **Failover backup if node is down**
	- Yes - Backup will be run on any available node (or selected node based on the *Backup Failover Host*) if the target database node is down. If failover is enabled and the selected node is not online, the backup job elects an online node to create the backup. This ensures that a backup will be created even if the selected node is not available. If the scheduled backup is an incremental backup and a full backup does not exist on the new elected node, then a full backup will be created.
	- No - Backup will not run if the target database node is down.
	
* **Backup Failover Host**
	- List of database host to failover in case the target node is down during the scheduled backup.

* **Verify Backup**
	- Verify the backup once successfully created. See `Verify Backup`_.

* **Use Extended Insert**
	- Exclusive for mysqldump only. The default is ON where ClusterControl will create the dump with with ``--extended-insert``. This results in a smaller dump file and speeds up inserts when the file is reloaded. If toggled OFF, ClusterControl will use ``--skip-extended-insert`` and ``--set-gtid-purged=OFF`` flags in the mysqldump command.

* **PITR Compatible**
	- Exclusive for mysqldump and if binary log is enabled. A mysqldump PITR-compatible backup contains one single dump file, with GTID info, binlog file and position. Thus, only the database node that produces binary log will have the "PITR Compatible" option available. See `Point-in-Time Recovery`_.

* **Upload Backup to the cloud**
	- Upload the backup to the cloud storage. The upload process happens right after the backup is successfully created.
	- This feature requires you to set up the cloud credentials first. See `Cloud Providers <../index.html#cloud-providers>`_.

* **Enable Encryption**
	- Encrypts the generated backup. Backup is encrypted at rest using AES-256 CBC algorithm, where the encryption key will be created automatically and stored inside CMON configuration file for this cluster. 
	- Encryption happens on the backup node. If you choose to store the backup on the controller node, the backup files are streamed over in encrypted format through :term:`socat` or :term:`netcat`. 

* **Retention**
	- How long ClusterControl should keep this backup once successfully created. You can set a custom period in days or keep it forever. Otherwise, ClusterControl will use the default retention period.

* **Backup Locks**
	- Exclusive for xtrabackup/mariabackup.
	- Yes - Uses ``LOCK TABLES FOR BACKUP`` whichever supported when making a backup.
	- No - Sets ``--no-backup-locks`` which use ``FLUSH NO_WRITE_TO_BINLOG TABLES`` and ``FLUSH TABLES WITH READ LOCK`` when making a backup.

* **Xtrabackup Parallel Copy Threads**
	- Exclusive for xtrabackup/mariabackup. This option specifies the number of threads to use to copy multiple data files concurrently when creating a backup. The default value is 1 (i.e., no concurrent transfer).

* **Xtrabackup Throttle Rate (IOPS)**
	- Exclusive for xtrabackup/mariabackup. Use ``--throttle`` flag to enable disk :term:`IOPS` throttling. 0 means disabled. This might be helpful on systems that do not have much spare I/O capacity.
	
* **Network Streaming Throttle Rate (MB/s)**
	- Exclusive for xtrabackup/mariabackup and only if the storage location is the controller. Throttle the backup streaming process using a tool called :term:`pv`. 0 means disabled.

* **Use PIGZ for parallel gzip**
	- Exclusive for xtrabackup/mariabackup. 
	- Yes - Use PIGZ instead of standard gzip. This is helpful if you want to backup very large data set.
	- No - Use the standard gzip.
 
  
Scheduled Backups
+++++++++++++++++

List of scheduled backups. You can enable and disable the schedule by toggling it accordingly. The created schedule can be edited and deleted.

Backup Method
++++++++++++++

This section explains backup method used by ClusterControl.

.. Note:: Backup process performed by ClusterControl is running as a background thread (RUNNING3) which doesn't block any other non-backup jobs in queue. If the backup job takes hours to complete, other non-backup jobs can still run simultaneously via the main thread (RUNNING). You can see the job progress at *ClusterControl > Logs > Jobs*.

mysqldump
``````````

ClusterControl performs :term:`mysqldump` against all or selected databases by using the ``--single-transaction`` option. It automatically performs mysqldump with ``--master-data=2`` if it detects binary logging is enabled on the particular node to generate binary log file and position statement in the dump file. ClusterControl generates a set of 4 mysqldump files with the following suffixes:

* _data.sql.gz - Schemas’ data.
* _schema.sql.gz - Schemas’ structure.
* _mysqldb.sql.gz - MySQL system database.
* _triggerseventroutines.sql.gz - MySQL triggers, event and routines.


Percona Xtrabackup
``````````````````

Percona Xtrabackup is an open-source MySQL hot backup utility from Percona. It is a combination of :term:`xtrabackup` (built in C) and :term:`innobackupex` (built on Perl) and can back up data from InnoDB, :term:`XtraDB` and :term:`MyISAM` tables. Xtrabackup does not lock your database during the backup process. For large databases (100+ GB), it provides much better restoration time as compared to mysqldump. The restoration process involves preparing MySQL data from the backup files before replacing or switching it with the current data directory on the target node.

Since its ability to create full and incremental MySQL backups, ClusterControl manages incremental backups, and groups the combination of full and incremental backups in a backup set. A backup set has an ID based on the latest full backup ID. All incremental backups after a full backup will be part of the same backup set. The backup set can then be restored as one single unit using `Restore Backup`_ feature.

.. Attention:: Without a full backup to start from, the incremental backups are useless.

MariaDB Backup
``````````````

MariaDB Backup is a fork of `Percona XtraBackup`_ with added support for compression and data-at-rest encryption available in MariaDB, included in MariaDB 10.1.23 and later. It is an open source tool provided by MariaDB for performing physical online backups of InnoDB, Aria and MyISAM tables. MariaDB Backup is available on Linux and Windows.	

On all supported versions for MariaDB 10.1 and 10.2, ClusterControl will default to MariaDB Backup as the preferred backup method and SST method. 

.. Seealso:: `MariaDB Backup Overview <https://mariadb.com/kb/en/library/mariadb-backup-overview/>`_


NDB backup (MySQL Cluster)
``````````````````````````

NDB backup triggers ``START BACKUP`` command on management node and perform mysqldump on each of the SQL nodes subsequently. These backup files will be created and streamed to ClusterControl node based on *ClusterControl > Settings > Backup > Backup Directory* location.

Backup List
+++++++++++

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
	- Removes the backup set. If you remove the backup set, all incremental backups associated with it will be removed as well.

* **Upload**
	- Manually upload the created backup to cloud storage. This will open "Upload Backup" wizard.

Verify Backup
+++++++++++++

Performs backup verification job.

* **Restore backup on**
	- Specify the FQDN, hostname or IP address of the standalone host. The host must not be part of the cluster.

* **Install Database Software**
	- A new MySQL server will be installed and setup if 'Install Software' has been enabled otherwise an existing running MySQL server on the target host will be used. If there is an existing MySQL server installed or running, it will be stopped and removed before ClusterControl performs the installation.

* **Disable Firewall?**
	- Check the box to disable firewall (recommended).

* **Disable SELinux/AppArmor?**
	- Check the box to disable SELinux (RHEL/CentOS) or AppArmor (Ubuntu).

* **Shutdown the server after the backup have been completed**
	- Select "Yes" if you want ClusterControl to shutdown the server after restoration completes. Select "No" if you want to let it run after restoration completes and the node will be listed under `Nodes <nodes.html>`_. You are then responsible for removing the MySQL server.

* **Verify the backup after N hours after completion**
	- Performs the backup verification after the specified hours once the backup is completed.

Restore Backup
++++++++++++++

Restores mysqldump, Percona Xtrabackup or MariaDB Backup created by ClusterControl and listed in the `Backup List`_. ClusterControl supports two restoration options:

- `Restore on node`_.
- `Restore and verify on standalone host`_.

Point-in-Time Recovery
``````````````````````

For Point-in-Time Recovery (PITR) compatible backup, there will be extra options to restore your database backup from the time of a full backup to a more recent time using a set of incremental backups represented by the database's binary log. If toggled, you will be presented with two recovery options:

- Time Based
	- Recover the data up until the data and time given by the *Restore Time*. 
	- Specify time in ClusterControl's server timezone. The restoration time must be in 'YYYY-MM-DD HH:MM:SS' format. E.g: "2018-08-22 21:00:00".
- Position Based
	- Recover the data up until the stop position is found in the specified binary log file. 
	- If you enter 'binlog.001827' under *Binary Log Name*, it will scan existing binary log files until binlog.001827 and will not go any further. 
	- Specify the log position to the point you want to recover under *Log Stop Position*.

Due to the dependency on the binary logs to perform recovery, the PITR-compatible backup can only be stored on the same host it was created from. Thus, this feature is applicable for `Restore on node`_.

Restore on node
````````````````

You can restore up to a number incremental backups by clicking on the *Restore* button for the respective backup ID. The following steps will be performed:

For mysqldump (online restore):

1. Copy backup files to the target server.
2. Checking disk space on the target server.
3. The mysqldump files will be copied to the node.
4. The schema, data and triggers/functions dump files are applied.
5. Optionally restore the 'mysql' database. If the 'cmon' user privileges has changed it may cause ClusterControl to stop functioning.
6. The rest of the members will then catch up with the target server.

For Percona Xtrabackup/MariaDB Backup (offline restore):

1. Stop all nodes in the cluster.
2. Copy backup files to the target server.
3. Checking disk space on the target server.
4. Prepare and restore the backup.
5. Follow the instruction in the *ClusterControl > Activity > Jobs > Restore Backup* on how to bootstrap the cluster. Alternatively, you can toggle on *Bootstrap cluster from the restored node*.

.. Attention:: ClusterControl does not support restoring a partial backup created by xtrabackup/mariabackup. The restoration requires you to manually export and import tablespace into a running MySQL server. Please refer to `Percona Xtrabackup documentation <https://www.percona.com/doc/percona-xtrabackup/LATEST/innobackupex/partial_backups_innobackupex.html#preparing-partial-backups>`_ before performing this exercise.

* **Point In Time Recovery (PITR)**
	- This option is only available if you want to restore a PITR-compatible backup. If toggled, you will be presented with two recovery options.
	- Time Based - Recover the data up until the data and time given by the *Restore Time*. Specify time in ClusterControl's server timezone. The restoration time must be in 'YYYY-MM-DD HH:MM:SS' format. E.g: "2018-08-22 21:00:00".
	- Position Based - Recover the data up until the stop position is found in the specified binary log file. If you enter 'binlog.001827' under *Binary Log Name*, it will scan existing binary log files until binlog.001827 and will not go any further. Specify the log position to the point you want to recover under *Log Stop Position*.

* **Restore backup on**
	- The backup will be restored on the selected server.

* **Tmp Dir**
	- Temporary storage for ClusterControl to prepare the big. It must be as big as the expected MySQL data directory.

* **xtrabackup --use-memory**
	- This option affects how much memory is allocated for preparing the backup using the ``--prepare`` flag. The default value is 100MB, and if you have enough memory available memory, 1024 MB to 2048 MB is a good recommended value.

* **Bootstrap cluster from the restored node**
	- Toggle to ON if you want ClusterControl to automatically re-bootstrap the cluster on the restored node.

* **Make a copy of the datadir before restoring the backup**
	- Toggle to ON to keep the old MySQL datadir before replacing the datadir with the prepared backup.
	
.. Attention:: The datadir must have enough space to accommodate the restored backup.

* **Restore "MySQL" Database**
	- Exclusive for mysqldump. Toggle to ON to restore the ``mysql`` system database if the backup was created by ClusterControl. If the ``cmon`` user privileges has changed, it may cause ClusterControl to stop functioning. This is fixable. Default is "No".

Restore and verify on standalone host
``````````````````````````````````````

Performs restoration on a standalone host and verify the backup. This requires a dedicated host which is not part of the cluster. ClusterControl will first deploy a MySQL instance on the target host, start the service, copy the backup from the backup repository and start performing the restoration. Once done, you can have an option either shutdown the server once restored or let it run so you can conduct investigation on the server.

You can monitor the job progress under *Activity > Jobs > Verify Backup* where ClusterControl will report the restoration status (based on the exit code) at the end of the job.

* **Restore backup on**
	- Specify the FQDN, hostname or IP address of the standalone host. The host must not be part of the cluster.

* **Install Software**
	- A new MySQL server will be installed and setup if 'Install Software' has been enabled otherwise an existing running MySQL server on the target host will be used. If there is an existing MySQL server installed or running, it will be stopped and removed before ClusterControl performs the installation.
	
* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Shutdown the server after the backup have been restored**
	- Select "Yes" if you want ClusterControl to shutdown the server after restoration completes. Select "No" if you want to let it run after restoration completes and the node will be listed under `Nodes <nodes.html>`_. You are then responsible for removing the MySQL server.

Restore External Backup
+++++++++++++++++++++++

Restores an external backup which does not listed in the `Backup List`_. It could be a backup created by another ClusterControl instance or the backup was created by the user.

.. Attention:: An external backup must contain privileges allowing the database user 'cmon' to connect to the MySQL server or all Galera nodes, or else ClusterControl may not be able to connect and monitor/manage the database nodes.

The following steps will be performed:

1. Stop all nodes in the cluster.
2. Copy backup files to the selected server.
3. Restore the backup.
4. Start the cluster.
5. Follow the instruction in the *ClusterControl > Logs > Job > Job Message* on how to bootstrap the cluster.

.. Note:: Only ``xbstream``, ``xbstream.gz``, ``.sql.gz`` extensions are supported. Do prepare your external backup with one of these extensions beforehand.

* **Restore backup on**
	- Specify the FQDN, hostname or IP address of the standalone host. The host must not be part of the cluster.

* **Backup Method**
	- How the backup was created, either mysqldump or xtrabackup.

* **Backup Path**
	- The backup file path (absolute path) on the ClusterControl node. The backup file will be copied to the target node during restoration.

* **Tmp Dir**
	- Temporary storage for ClusterControl to prepare the restoration data. It must be as big as the expected MySQL data directory. ClusterControl will check if it has enough disk space to work on before proceed with the restoration.
	
* **Bootstrap cluster from the restored node?**
	- Toggle to ON if you want ClusterControl to automatically re-bootstrap the cluster on the restored node.

* **Make a copy of the datadir before restoring the backup**
	- Toggle to ON to keep the old MySQL datadir before replacing the datadir with the prepared backup.
	
.. Attention:: The datadir must have enough space to accommodate the restored backup.

* **Does the dump file set the database to restore the data into?**
	- Exclusive for mysqldump. Toggle to OFF if the dump file doesn't contain ``USE {database}`` statement and specify the database name here.

* **RESET MASTER before restore**
	- Exclusive for mysqldump. Toggle to ON to perform ``RESET MASTER`` before performing the restoration. This may be needed if the dump file contains GTID information.
	
.. Warning:: If the dump file contains the mysql database, then it is required that the dump file contains the 'cmon' account and the same privileges. Else the controller cannot connect after the restore due to changed privileges.

Settings
++++++++

Manages the backup settings.

* **Default backup directory**
	- Default path for the backup directory. ClusterControl will create the backup directory on the destination host if doesn't exist.

* **Backup retention period**
	- The number of days ClusterControl keeps the existing backups. Backups older than the value defined here will be deleted. You can also customize the retention period per backup (default, custom or keep forever) under *Backup Retention* when creating or scheduling the backup.
	- The purging is based on the following conditions:
	  - When a new backup is successfully created, and if no verify backup is requested, the older backups will be checked and removed. 
	  - When the verify backup is successfully created, the older backups will be checked and removed.
	  - The backup housekeeping job remain executed every 24 hour. Thus, if no backups are created and no backups are verified, the backup retention still will be done in every 24 hours.

.. Note:: The backup housekeeping frequency is determined by how frequent the backups are taken, regardless if it's a scheduled or immediate backup.

* **Backup cloud retention period**
	- The number of days ClusterControl keeps the uploaded backups in the cloud. Backups older than the value defined here will be deleted.