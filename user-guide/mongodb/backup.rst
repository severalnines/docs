Backup
-------

Provides interface for database backup and restore management, scheduling and reporting. Each backup is assigned with a backup ID and ClusterControl creates a directory under *Storage Directory* to store the backups based on this ID.

Create Backup
+++++++++++++

Creates or schedules a MongoDB backup. 

Create Backup
`````````````

Creates a backup of the database immediately. You can choose to create a full backup using :term:`mongodump` or MongoDB consistent backup. The backup created by this feature will be a full backup.

* **Backup Method**
	- mongodump - See `mongodump`_ section.
	- mongodb consistent - See `mongodb consistent`_ section.

* **Storage Directory**
	- Enter a backup directory or use default path provided by ClusterControl under *ClusterControl > Settings > Backup*. ClusterControl organizes backup directory according to shard or replica set name, under the *Backup Directory* on the ClusterControl host.
	
* **Backup Subdirectory**
	- Set the name of the backup subdirectory. The full backup path will be *Storage Directory*/*Backup Subdirectory*/. This string may hold standard ``%X`` field separators, the ``%06I`` for example will be replaced by the numerical ID of the backup in 6 field wide format that uses '0' as leading fill characters. Default value: ``BACKUP-%I``.

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

* **Upload Backup to the cloud**
	- Upload the backup to the cloud storage. The upload process happens right after the backup is successfully created. This feature requires you to set up the cloud credentials first. See :ref:`Sidebar - Integrations - Cloud Providers`.
	
* **Enable Encryption**
	- Encrypts the generated backup. Backup is encrypted at rest using AES-256 CBC algorithm, where the encryption key will be created automatically and stored inside CMON configuration file for this cluster. See `Backup Encryption and Decryption`_.

* **Retention**
	- How long ClusterControl should keep this backup once successfully created. You can set a custom period in days or keep it forever. Otherwise, ClusterControl will use the default retention period. Take note that modifying retention period on existing schedule has no effect on already created backup.

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

* **Backup Subdirectory**
	- Set the name of the backup subdirectory. The full backup path will be *Storage Directory*/*Backup Subdirectory*/. This string may hold standard ``%X`` field separators, the ``%06I`` for example will be replaced by the numerical ID of the backup in 6 field wide format that uses '0' as leading fill characters. Default value: ``BACKUP-%I``.

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

* **Upload Backup to the cloud**
	- Upload the backup to the cloud storage. The upload process happens right after the backup is successfully created. This feature requires you to set up the cloud credentials first. See :ref:`Sidebar - Integrations - Cloud Providers`.

* **Enable Encryption**
	- Encrypts the generated backup. Backup is encrypted at rest using AES-256 CBC algorithm, where the encryption key will be created automatically and stored inside CMON configuration file for this cluster. See `Backup Encryption and Decryption`_.

* **Retention**
	- How long ClusterControl should keep this backup once successfully created. You can set a custom period in days or keep it forever. Otherwise, ClusterControl will use the default retention period. Take note that modifying retention period on existing schedule has no effect on already created backup.

* **Failover backup if node is down**
	- Yes - Backup will be run on any available node (or selected node based on the *Backup Failover Host*) if the target database node is down. If failover is enabled and the selected node is not online, the backup job elects an online node to create the backup. This ensures that a backup will be created even if the selected node is not available. If the scheduled backup is an incremental backup and a full backup does not exist on the new elected node, then a full backup will be created.
	- No - Backup will not run if the target database node is down.
	
* **Backup Failover Host**
	- List of database host to failover in case the target node is down during the scheduled backup.

Scheduled backups
+++++++++++++++++

List of scheduled backups. You can enable and disable the schedule by toggling it accordingly. The created schedule can only be deleted and cannot be modified.

Backup Method
+++++++++++++

This section explains backup method used by ClusterControl for MongoDB.

.. Note:: Backup process performed by ClusterControl is running on a background thread (RUNNING3) which doesn't block any other non-backup jobs in queue. If the backup job takes hours to complete, other non-backup jobs can still run simultaneously via the main thread (RUNNING). You can see the job progress at *ClusterControl > Logs > Jobs*.

mongodump
``````````

ClusterControl uses the standard command to perform :term:`mongodump` with ``--journal`` option, which allows mongodump operations to use the durability journal to ensure that the export is in a consistent state against shards.

mongodb consistent
``````````````````

Built on top of Python 2.7, also known as :term:`mongodb-consistent-backup`, it creates cluster-consistent point-in-time backups of MongoDB via wrapping :term:`mongodump`. Backups are remotely-pulled and outputted onto the host running the tool. Even if you do not run MongoDB 3.2+, it is strongly recommended to use MongoDB 3.2+ binaries due to inline compression and parallelism.

.. Note:: :term:`mongodump` is required on the database node for this feature to work.

Backup List
+++++++++++

Provides a list of finished backup jobs. The status can be:

========= ===========
Status    Description
========= ===========
completed Backup was successfully created and stored in the chosen node.
running   Backup process is running.
failed    Backup was failed.
========= ===========


Backup Encryption and Decryption
++++++++++++++++++++++++++++++++

If encryption option is enabled for a particular backup, ClusterControl will uses :term:`OpenSSL` to encrypt the backup using AES-256 CBC algorithm. Encryption happens on the backup node. If you choose to store the backup on the controller node, the backup files are streamed over in encrypted format through :term:`socat` or :term:`netcat`.

If compression is enabled, the backup is first compressed and then encrypted resulting in smaller backup sizes. The encryption key will be generated automatically (if not exists) and stored inside CMON configuration for the particular cluster under ``backup_encryption_key`` option. This key is stored with base64 encoded and should be decoded first before using it as an argument to pass when decrypting the backup. The following command shows how to decode the key:

.. code-block:: bash

	$ cat /etc/cmon.d/cmon_X.cnf | grep ^backup_encryption_key | cut -d"'" -f2 | base64 -d > keyfile.key

Where X is the cluster ID. The above command will read the ``backup_encryption_key`` value and decode the value to a binary output. Thus, it is important to redirect the output to a file, as in the example, we redirected the output to ``keyfile.key``. The key file which stores the actual encryption key can be used in the openssl command to decrypt the backup, for example:

.. code-block:: bash

	$ cat {BACKUPFILE}.aes256 | openssl enc -d -aes-256-cbc -pass file:/path/to/keyfile.key > backup_file.gz
	
Or, you can pass the stdin to the respective restore command chain, for example:

.. code-block:: bash

	$ cat cat {BACKUPFILE}.aes256 | openssl enc -d -aes-256-cbc -pass file:/path/to/keyfile.key | /usr/bin/mongorestore --host localhost --port 27017 --username backupuser --password mysecret --authenticationDatabase admin --drop --oplogReplay --gzip --archive


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