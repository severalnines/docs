.. _PostgreSQL - Backup:

Backup
-------

Provides interface for database backup and restore management, scheduling and reporting. Each backup will be assigned with a backup ID and ClusterControl creates a directory under *Storage Directory* to store the backups based on this ID. On top of the page, you can see 3 function tabs followed by the created backup list underneath it.

.. _PostgreSQL - Backup - Create Backup:

Create Backup
+++++++++++++

Creates or schedules a PostgreSQL backup. 

Create Backup
``````````````

You can choose to create a full backup using :term:`pg_dumpall` or :term:`pg_basebackup`. Backups can be stored on the database host that is performing the backup, or the files can be streamed to the ClusterControl host. The backup created by this feature will be a full backup.

* **Backup Method**
	- pg_dumpall - Full logical backup using ``pg_dumpall``. See `pg_dumpall`_ section.
	- pg_basebackup - Full physical backup using ``pg_basebackup``. See `pg_basebackup`_ section.
	- pgbackrestfull - Full physical backup using ``pgBackRest``. See `pgBackRest`_ section.
	- pgbackrestdiff - Differential physical backup using ``pgBackRest``. See `pgBackRest`_ section.
	- pgbackrestincr - Incremental physical backup using ``pgBackRest``. See `pgBackRest`_ section.

* **Backup Host**
	- The target database host.
	
* **Storage Location**
	- Store on Node - Stores the backup inside corresponding database node.
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
	M         The backup method (e.g. "pg_dumpall").
	O         The name of the user who initiated the backup job.
	S         The name of the storage host, the host that stores the backup files.
	%         The percent sign itself. Use two percent signs, ``%%`` the same way the standard ``printf()`` function interprets it as one percent sign.
	========= ===================

* **Netcat Port**
	- Specify the port number, or port number range that will be used by ClusterControl to stream backup created on the database node. This port must be opened on both source and destination hosts. Only available if you choose *Store on Controller* in *Storage Location*.
	- Default is "9999,9990-9998", which means port 9999 will be preferred if available.
	
* **Use Compression**
	- Yes - Tells the chosen backup method to compress all output data, including the transaction log file and meta data files.
	- No - Do not use compression for the backup.

* **Compression Level**
	- Specify the compression level for the backup. This is according to :term:`gzip` compression level. 1 is the fastest compression (least compression) and 9 indicates the slowest compression (best compression).

* **Upload Backup to the cloud**
	- Uploads the backup to the cloud storage. The upload process happens right after the backup is successfully created. This feature requires you to set up the cloud credentials first. See :ref:`Sidebar - Integrations - Cloud Providers`.

* **Enable Encryption**
	- Encrypts the generated backup. Backup is encrypted at rest using AES-256 CBC algorithm, where the encryption key will be created automatically and stored inside CMON configuration file for this cluster. See `Backup Encryption and Decryption`_.

* **Retention**
	- How long ClusterControl should keep this backup once successfully created. You can set a custom period in days or keep it forever. Otherwise, ClusterControl will use the default retention period.

.. _PostgreSQL - Backup - Schedule Backup:

Schedule Backup
````````````````

Creates backup schedules of the database.

* **Schedule**
	- Simple - Default scheduling option. This translates to the same output as the Advanced editor.
	- Advanced - Opens a cron-like editor. Formatting is similar to the standard :term:`cron`.

.. Note:: The backup time is in UTC time zone of the ClusterControl node.

* **Backup Method**
	- pg_dumpall - Full logical backup using ``pg_dumpall``. See `pg_dumpall`_ section.
	- pg_basebackup - Full physical backup using ``pg_basebackup``. See `pg_basebackup`_ section.
	- pgbackrestfull - Full physical backup using ``pgBackRest``. See `pgBackRest`_ section.
	- pgbackrestdiff - Differential physical backup using ``pgBackRest``. See `pgBackRest`_ section.
	- pgbackrestincr - Incremental physical backup using ``pgBackRest``. See `pgBackRest`_ section.

* **Backup Host**
	- The target database host.

* **Storage Location**
	- Store on Node - Stores the backup inside corresponding database node.
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
	M         The backup method (e.g. "pg_dumpall").
	O         The name of the user who initiated the backup job.
	S         The name of the storage host, the host that stores the backup files.
	%         The percent sign itself. Use two percent signs, ``%%`` the same way the standard ``printf()`` function interprets it as one percent sign.
	========= ===================

* **Upload Backup to the cloud**
	- Upload the backup to the cloud storage. The upload process happens right after the backup is successfully created. This feature requires you to set up the cloud credentials first. See :ref:`Sidebar - Integrations - Cloud Providers`.

* **Netcat Port**
	- Specify the port number, or port number range that will be used by ClusterControl to stream backup created on the database node. This port must be opened on both source and destination hosts. Only available if you choose *Store on Controller* in *Storage Location*.
	- Default is "9999,9990-9998", which means port 9999 will be preferred if available.

* **Use Compression**
	- Yes - Tells the chosen backup method to compress all output data, including the transaction log file and meta data files.
	- No - Do not use compression for the backup.

* **Compression Level**
	- Specify the compression level for the backup. This is according to :term:`gzip` compression level. 1 is the fastest compression (least compression) and 9 indicates the slowest compression (best compression).

* **Failover backup if node is down**
	- Yes - Backup will be run on any available node (or selected node based on the *Backup Failover Host*) if the target database node is down. If failover is enabled and the selected node is not online, the backup job elects an online node to create the backup. This ensures that a backup will be created even if the selected node is not available. If the scheduled backup is an incremental backup and a full backup does not exist on the new elected node, then a full backup will be created.
	- No - Backup will not run if the target database node is down.
	
* **Failover Host**
	- List of database host to failover in case the target node is down during the scheduled backup.

* **Verify Backup**
	- Verify the backup once successfully created. See `Verify Backup`_.

* **Enable Encryption**
	- Encrypts the generated backup. Backup is encrypted at rest using AES-256 CBC algorithm, where the encryption key will be created automatically and stored inside CMON configuration file for this cluster. See `Backup Encryption and Decryption`_.

* **Retention**
	- How long ClusterControl should keep this backup once successfully created. You can set a custom period in days or keep it forever. Otherwise, ClusterControl will use the default retention period.
  
Scheduled Backups
+++++++++++++++++

List of scheduled backups. You can enable and disable the schedule by toggling it accordingly. The created schedule can be edited and deleted.

.. _PostgreSQL - Backup - Backup Method:

Backup Method
+++++++++++++

This section explains backup method used by ClusterControl.

.. Note:: Backup process performed by ClusterControl is running as a background thread (RUNNING3) which doesn't block any other non-backup jobs in queue. If the backup job takes hours to complete, other non-backup jobs can still run simultaneously via the main thread (RUNNING). You can see the job progress at *ClusterControl > Logs > Jobs*.

pg_dumpall
``````````

ClusterControl performs :term:`pg_dumpall` against all databases together with ``--clean`` option, which include SQL commands to clean (drop) databases before recreating them. DROP commands for roles and tablespaces are added as well. The output will be in ``.sql.gz`` extention and file name contains the timestamp of the backup.

pg_basebackup
``````````````

:term:`pg_basebackup` is used to take base backups of a running PostgreSQL database cluster. These are taken without affecting other clients to the database, and can be used both for point-in-time recovery and as the starting point for a log shipping or streaming replication standby servers. It makes a binary copy of the database cluster files, while making sure the system is put in and out of backup mode automatically. Backups are always taken of the entire database cluster; it is not possible to back up individual databases or database objects.

ClusterControl connects to the replication stream using the replication user (default is ``cmon_replication``) with ``--wal-method=fetch`` option when creating the backup. The output will be ``base.tar.gz`` inside the backup directory.

pgBackRest
``````````

.. Note:: This backup method is introduced in ClusterControl 1.7.2.

:term:`pgBackRest` is an open source software developed to perform efficient backup on PostgreSQL databases that measure in tens of terabytes and greater. It supports per file checksums, compression, partial/failed backup resume, high-performance parallel transfer, asynchronous archiving, tablespaces, expiration, full/differential/incremental, local/remote operation via SSH, hard-linking, restore, and more. PgBackRest is written in Perl and does not depend on rsync or tar but instead performs its own deltas which gives it maximum flexibility.

Only master node can be used to make a pgBackRest backup. During the first attempt of making pgBackRest backup, ClusterControl will re-configure the node to install and configure pgBackRest. Take note that this operation requires a database restart and might introduce downtime to your database. A configuration file will be created at ``/etc/pgbackrest.conf`` and will be configured according to the version used and location of the PostgreSQL data. Additionally, ClusterControl will configure the following lines inside ``postgresql.conf`` (which explains why it requires restart during the first run):

.. code-block:: bash

	archive_mode = on	# enables archiving; off, on, or always # (change requires restart)
	archive_command = 'pgbackrest --stanza=clustercontrol-stanza archive-push %p'	# command to use to archive a logfile segment

Full Backup
'''''''''''

pgBackRest copies the entire contents of the database cluster to the backup. The first backup of the database cluster is always a Full Backup. pgBackRest is always able to restore a full backup directly. The full backup does not depend on any files outside of the full backup for consistency.

Differential Backup
'''''''''''''''''''

pgBackRest copies only those database cluster files that have changed since the last full backup. pgBackRest restores a differential backup by copying all of the files in the chosen differential backup and the appropriate unchanged files from the previous full backup. The advantage of a differential backup is that it requires less disk space than a full backup, however, the differential backup and the full backup must both be valid to restore the differential backup.

For example, if a full backup is taken on Sunday and the following daily differential backups are scheduled, the data that being backed up will be:

* Monday - data from Sunday to Monday
* Tuesday - data from Sunday to Tuesday
* Wednesday - data from Sunday to Wednesday
* Thursday - data from Sunday to Thursday

Incremental Backup
''''''''''''''''''

pgBackRest copies only those database cluster files that have changed since the last backup (which can be another incremental backup, a differential backup, or a full backup). As an incremental backup only includes those files changed since the prior backup, they are generally much smaller than full or differential backups. As with the differential backup, the incremental backup depends on other backups to be valid to restore the incremental backup. Since the incremental backup includes only those files since the last backup, all prior incremental backups back to the prior differential, the prior differential backup, and the prior full backup must all be valid to perform a restore of the incremental backup. If no differential backup exists then all prior incremental backups back to the prior full backup, which must exist, and the full backup itself must be valid to restore the incremental backup.

For example, if a full backup is taken on Sunday and the following daily incremental backups are scheduled, the data that being backed up will be:

* Monday - data from Sunday to Monday
* Tuesday - data from Monday to Tuesday
* Wednesday - data from Tuesday to Wednesday
* Thursday - data from Wednesday to Thursday

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

* **Restore**
	- See `Restore Backup`_.

* **Log**
	- Shows the output once ClusterControl executes the backup job.

* **Delete**
	- Removes the backup set.

* **Upload**
	- Manually upload the created backup to cloud storage. This will open "Upload Backup" wizard.

.. _PostgreSQL - Backup - Verify Backup:

Verify Backup
+++++++++++++

Performs backup verification job.

* **Restore backup on**
	- Specify the FQDN, hostname or IP address of the standalone host. The host must not be part of the cluster.

* **Install Database Software**
	- A new PostgreSQL server will be installed and setup if this is enabled. If there is an existing PostgreSQL server installed or running, it will be stopped and removed before ClusterControl performs the installation. If unchecked, ClusterControl will not touch the existing installation and the existing PostgreSQL server (must be running) on the target host will be used.

* **Disable Firewall?**
	- Check the box to disable firewall (recommended).

* **Disable SELinux/AppArmor?**
	- Check the box to disable SELinux (RHEL/CentOS) or AppArmor (Ubuntu).

* **Shutdown the server after the backup have been completed**
	- Select "Yes" if you want ClusterControl to shutdown the server after restoration completes. Select "No" if you want to let it run after restoration completes and the node will be listed under :ref:`PostgreSQL - Nodes`. You are then responsible for removing the PostgreSQL server.

* **Verify the backup after N hours after completion**
	- Performs the backup verification after the specified hours once the backup is completed.

.. _PostgreSQL - Backup - Restore Backup:

Restore Backup
++++++++++++++

Restores ``pg_dumpall`` or ``pg_basebackup`` backup file created by ClusterControl and listed in the `Backup List`_. ClusterControl supports three restoration options:

- `Restore on node`_.
- `Restore and verify on standalone host`_.
- `Create cluster from backup`_.

Restore on node
````````````````

You can restore up to a certain incremental backup by clicking on the *Restore* button for the respective backup ID. The following steps will be performed:

For pgdump (online restore):

1. Copy backup files to the target server.
2. Checking disk space on the target server.
3. Restore the backup.
4. Follow the instruction in the *ClusterControl > Activity > Jobs* on how to rebuild the slaves.

For pg_basebackup (offline restore):

1. Stop the target node.
2. Backup the current PostgreSQL data directory.
3. Copy backup files to the target server.
4. Checking disk space on the target server.
5. Prepare and restore the backup.
6. Start the target node.
7. Follow the instruction in the *ClusterControl > Activity > Jobs* on how to rebuild the slaves.

* **Restore backup on**
	- The backup will be restored to the selected server.
	
* **Tmp Dir**
	- Temporary storage for ClusterControl to prepare the big. It must be as big as the expected PostgreSQL data directory.
	
* **Point In Time Recovery (PITR)**
	- This option is only available if you want to restore a PITR-compatible backup (with WAL archiving enabled). If toggled, you will have to specify the time (folloing the server's timezone) to recover the data up to that point. The restoration time must be in 'YYYY-MM-DD HH:MM:SS' format. E.g: "2018-08-22 21:00:00".
	
.. Attention:: The data directory must have enough space to accommodate the restored backup.

Restore and verify on standalone host
``````````````````````````````````````

Performs restoration on a standalone host and verify the backup. This requires a dedicated host which is not part of the cluster. ClusterControl will first deploy a PostgreSQL instance on the target host, start the service, stream the backup from the backup repository and start performing the restoration. Once done, you can have an option either to shutdown the server once restored or let it run so you can conduct further investigation on the server.

You can monitor the job progress under *Activity > Jobs > Verify Backup* where ClusterControl will report the restoration status (based on the exit code) at the end of the job.

* **Restore backup on**
	- Specify the FQDN, hostname or IP address of the standalone host. The host must not be part of the cluster.

* **Install Software**
	- A new PostgreSQL server will be installed and setup if this is enabled. If there is an existing PostgreSQL server installed or running, it will be stopped and removed before ClusterControl performs the installation. If unchecked, ClusterControl will not touch the existing installation and the existing PostgreSQL server (must be running) on the target host will be used.
	
* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Shutdown the server after the backup have been restored**
	- Select "Yes" if you want ClusterControl to shutdown the server after restoration completes. Select "No" if you want to let it run after restoration completes and the node will be listed under :ref:`PostgreSQL - Nodes`. You are then responsible for removing the PostgreSQL server.

Create cluster from backup
````````````````````````````

.. Note:: This feature is introduced in version 1.7.1, specifically for Galera Cluster and PostgreSQL clusters only.

Creates a new cluster from the existing backup. A new PostgreSQL cluster will be created from the selected backup. The selected backup must be accessible from the nodes in the new cluster. The admin user password for this cluster must the same as the PostgreSQL admin password as included in the backup.

Choosing this option will open a new dialog where the selected backup will be used as a base dataset for the new cluster. The same deployment wizard for PostgreSQL will be shown to configure a new cluster. See :ref:`Deploy - PostgreSQL` for reference.

Basically, ClusterControl performs the deployment job based on the following order:

1) Install necessary softwares and dependencies on all PostgreSQL nodes.
2) Start the first node.
3) Stream and restore backup on the first node (with auto-restart flag).
4) Configure and add the rest of the nodes.

A new PostgreSQL cluster will be listed under ClusterControl cluster dashboard once the job completes.

Backup Encryption and Decryption
++++++++++++++++++++++++++++++++

If encryption option is enabled for a particular backup, ClusterControl will uses :term:`OpenSSL` to encrypt the backup using AES-256 CBC algorithm. Encryption happens on the backup node. If you choose to store the backup on the controller node, the backup files are streamed over in encrypted format through :term:`socat` or :term:`netcat`.

If compression is enabled, the backup is first compressed and then encrypted resulting in smaller backup sizes. The encryption key will be generated automatically (if not exists) and stored inside CMON configuration for the particular cluster under ``backup_encryption_key`` option. This key is stored with base64 encoded and should be decoded first before using it as an argument to pass when decrypting the backup. The following command shows how to decode the key:

.. code-block:: bash

	$ cat /etc/cmon.d/cmon_X.cnf | grep ^backup_encryption_key | cut -d"'" -f2 | base64 -d > keyfile.key

Where X is the cluster ID. The above command will read the ``backup_encryption_key`` value and decode the value to a binary output. Thus, it is important to redirect the output to a file, as in the example, we redirected the output to ``keyfile.key``. The key file which stores the actual encryption key can be used in the openssl command to decrypt the backup, for example:

.. code-block:: bash

	$ cat {BACKUPFILE}.aes256 | openssl enc -d -aes-256-cbc -pass file:/path/to/keyfile.key > backup_file.sql.gz
	
Or, you can pass the stdin to the respective restore command chain, for example:

.. code-block:: bash

	$ cat {BACKUPFILE}.aes256 | openssl enc -d -aes-256-cbc -pass file:/path/to/keyfile.key | gunzip | psql -p5432 -f-

Settings
++++++++

Manages the backup settings.

* **Default Backup Directory**
	- Default path for the backup directory. ClusterControl will create the backup directory on the destination host if doesn't exist.

* **Default Backup Subdirectory**
	- Set the name of the backup subdirectory. This string may hold standard ``%X`` field separators, the ``%06I`` for example will be replaced by the numerical ID of the backup in 6 field wide format that uses '0' as leading fill characters. Default value: ``BACKUP-%I``.

	========= ===================
	Variable  Description
	========= ===================
	B         The date and time when the backup creation was beginning.
	H         The name of the backup host, the host that created the backup.
	i         The numerical ID of the cluster.
	I         The numerical ID of the backup.
	J         The numerical ID of the job that created the backup.
	M         The backup method (e.g. "pg_dumpall").
	O         The name of the user who initiated the backup job.
	S         The name of the storage host, the host that stores the backup files.
	%         The percent sign itself. Use two percent signs, ``%%`` the same way the standard ``printf()`` function interprets it as one percent sign.
	========= ===================

* **Backup retention period (days)**
	- The number of days ClusterControl keeps the existing backups. Backups older than the value defined here will be deleted. You can also customize the retention period per backup (default, custom or keep forever) under *Backup Retention* when creating or scheduling the backup.

* **Backup cloud retention period (days)**
	- The number of days ClusterControl keeps the uploaded backups in the cloud. Backups older than the value defined here will be deleted.

* **Enable Point in time recovery (WAL Archiving)**
	- Enables WAL archiving. If it is enabled and you click "Save", the following steps will be performed on the master node:
	
	1) Enable the WAL archiving on the master node.
	2) Master node will be restarted.
	
* **Compress WAL Archive**
	- Option to compress the WAL archives.
	
* **PITR Retention Hours**
	- This setting specifies how long WAL files are kept. Default is 0 which means old WAL files will be kept forever.