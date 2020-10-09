.. include:: <isotech.txt>

s9s backup
----------

View and create database backups. Three backup methods are supported:
	* mysqldump
	* xtrabackup (full)
	* xtrabackup (incremental)
	* mariabackup (full)
	* mariabackup (incremental)
	* mongodump
	* pg_dump

The s9s client also needs to know:
	* The cluster ID (or cluster name) of the cluster to backup.
	* The node to backup.
	* The databases that should be included (default all databases).
	
By default, the backups will be stored on the controller node. If you wish to store the backup on the data node, you can set the flag ``--on-node``.
	
.. Note:: If you are using Percona Xtrabackup, an incremental backup requires that there is already a full backup made of the same databases (all or individually specified). Otherwise, the incremental backup will be upgraded to a full backup.

**Usage**

.. code-block:: bash

	s9s backup {command} {options}

**Command**

======================================== ===========
Name, shorthand                          Description
======================================== ===========
|minus|\ |minus|\ create                 Creates a new backup. This command line option will initiate a new job that will create a new backup.
|minus|\ |minus|\ create-schedule        Creates a backup schedule, a backup that is repeated. Please note that there are two ways to create a repeated backup. A job that creates a backup can be scheduled and repeated and with this option a backup schedule can be created to repeat the creation of a backup.
|minus|\ |minus|\ delete                 Deletes an existing backup.
|minus|\ |minus|\ delete-old             Initiates a job that checks for expired backups and removes them from the system.
|minus|\ |minus|\ list                   Lists the backups. When listing the backups with the ``--long`` option, a more detailed columns are listed. See `Backup List`_.
|minus|\ |minus|\ list-databases         Lists the backups in database view format. This format is designed to show the archived databases in the backups.
|minus|\ |minus|\ list-files             Lists the backups in file view format. This format is designed to show the archive files of the backups.
|minus|\ |minus|\ list-schedules         Lists the backup schedules.
|minus|\ |minus|\ restore                Restores an existing backup.
|minus|\ |minus|\ restore-cluster-info   Restores the information the controller has about a cluster from a previously created archive file.
|minus|\ |minus|\ restore-controller     Restores the entire controller from a previously created tarball (created by using the ``--save-controller`` option).
|minus|\ |minus|\ save-cluster-info      Saves the information about one cluster.
|minus|\ |minus|\ save-controller        Saves the entire controller into a file.
|minus|\ |minus|\ verify                 Creates a job to verify a backup. When this main option is used the ``--backup-id``  option  has  to  be  used to identify a backup and the ``--test-server`` is also necessary to provide a server where the backup will be tested.
======================================== ===========

**Options**

=============================================== ===========
Name, shorthand                                 Description
=============================================== ===========
|minus|\ |minus|\ backup-directory=DIR          The directory where the backup is placed.
|minus|\ |minus|\ backup-format[=FORMATSTRING]  The string that controls the format of the printed information about the backups. See `Backup Format`_.
|minus|\ |minus|\ backup-id=ID                  The ID of the backup.
|minus|\ |minus|\ backup-method=METHOD          Controls what backup software is going to be used to create the backup. The controller currently supports the following methods: ``ndb``, ``mysqldump``, ``xtrabackupfull``, ``xtrabackupincr``, ``mariabackupfull``, ``mariabackupincr``, ``mongodump``, ``pgdump``, ``pg_basebackup``, ``mysqlpump``.
|minus|\ |minus|\ backup-password=PASSWORD      The password for the SQL account that will create the backup. This command line option is not mandatory.
|minus|\ |minus|\ backup-retention=DAYS         Controls a custom retention period for the backup, otherwise the default global setting will be used. Specifying a positive number value here can control how long (in days) the taken backups will be preserved, -1 has a special meaning, it means the backup will be kept forever, while value 0 is the default, means prefer the global setting (configurable on UI).
|minus|\ |minus|\ backup-user=USERNAME          The username for the SQL account that will create the backup.
|minus|\ |minus|\ cloud-retention=DAYS          Retention used when the backup is on a cloud.
|minus|\ |minus|\ cluster-id=ID                 The ID of the cluster.
|minus|\ |minus|\ databases=LIST                A comma separated list of database names. This argument controls which databases are going to be archived into the backup file. By default all the databases are going to be archived.
|minus|\ |minus|\ encrypt-backup                When this option is specified ClusterControl will attempt to encrypt the backup files using AES-256 encryption (the key will be auto-generated if not exists yet and stored in cluster configuration file).
|minus|\ |minus|\ full-path                     Print the full path of the files.
|minus|\ |minus|\ memory=MEGABYTES              Controls how many memory the archiver process should use while restoring an archive. Currently only the xtrabackup supports this option.
|minus|\ |minus|\ no-compression                Do not compress the archive file.
|minus|\ |minus|\ nodes=NODELIST                The list of nodes involved in the backup. See `Node List`_.
|minus|\ |minus|\ on-node                       Do not copy the created archive file to the controller, store it on the node where it was created.
|minus|\ |minus|\ on-controller                 Stream and store the created backup files on the controller.
|minus|\ |minus|\ parallelism=N                 Controls how many threads are used while creating backup. Please note that not all the backup methods support multi-thread operations.
|minus|\ |minus|\ pitr-compatible               Creates PITR-compatible backup.
|minus|\ |minus|\ recurrence=STRING             Schedule time and frequency in cron format.
|minus|\ |minus|\ safety-copies=N               Controls how many safety backups should be kept while deleting old backups. This command line option can be used together with the ``--delete-old`` option.
|minus|\ |minus|\ subdirectory=MARKUPSTRING     Sets the name of the subdirectory that holds the newly created backup files. The command line option argument is considered to be a subpath that may contain the field specifiers using the usual ``%X`` format. See `Backup Subdirectory Variables`_.
|minus|\ |minus|\ test-server=HOSTNAME          Use the given server to verify the backup. If this option is provided while creating a new backup after the backup is created a new job is going to be created to verify the backup. During the verification the SQL software will be installed on the test server and the backup will be restored on this server. The verification job will be successful if the backup is successfully restored.
|minus|\ |minus|\ title=STRING                  A short human readable string that helps the user to identify the backup later.
|minus|\ |minus|\ to-individual-files           Archive every database into individual files. Currently only the mysqldump backup method supports this option.
|minus|\ |minus|\ use-pigz                      Use the pigz program to compress archive.
=============================================== ===========

Backup Subdirectory Variables
+++++++++++++++++++++++++++++

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

Backup List
+++++++++++

========= ===================
Column    Description
========= ===================
ID        The numerical ID of the backup.
PI        The numerical ID of the parent backup if there is a parent backup for the given entry.
CID       The numerical ID of the cluster to which the backup belongs.
V         The verification status. Here ``V`` means the backup is verified, ``-`` means the backup is not verified.
I         The flag showing if the backup is incremental or not. Here ``F`` means the backup is a full backup, ``I`` means the backup is incremental, ``-`` means the backup contains no incremental or full backup files (because for example the backup failed) and ``B`` means the backup contains both full and incremental backups files (which is impossible).
STATE     The state of the backup. Here "COMPLETED" means the backup is completed, "FAILED" means the backup has failed and "RUNNING" means the backup is being created.
OWNER     The name of the Cmon user that owns the backup.
HOSTNAME  The name of the host where the backup was created.
CREATED   The date and time when the backup was created.
SIZE      The total size of the created backup files.
TITLE     The name or title of the backup. This is a human readable string that helps identify the backup.
========= ===================

Backup Format
+++++++++++++

When the option ``--backup-format`` is used the specified information will be printed instead of the default columns. The format string uses the ``%`` character to mark variable fields and flag characters as they are specified in the standard ``printf()`` C library functions. The ``%`` specifiers are ended by field name letters to refer to various properties of the backups.

The  ``%+12I`` format string for example has the ``+12`` flag characters in it with the standard meaning: the field will be 12 character wide and the "+" or "-" sign will always be printed with the number. The properties of the backup are encoded by letters. The in the ``%16H`` for example the letter ``H`` encodes the ``hostname``. Standard ``\`` notation is also available, ``\n`` for example encodes a new-line character.

The s9s-tools support the following fields:

========= ===================
Character Description
========= ===================
B         The date and time when the backup creation was beginning. The format used to print the dates and times can be set using the ``--date-format``.
C         The backup file creation date and time. The format used to print the dates and times can be  set using the ``--date-format``.
d         The names of the databases in a comma separated string list.
D         The description of the backup. If the ``c`` modifier is used (e.g. ``%cD``) the configured description is shown.
e         The word "ENCRYPTED" or "UNENCRYOTED" depending on the encryption status of the backup.
E         The date and time when the backup creation was ended. The format used to print the dates and times can be set using the ``--date-format``.
F         The archive file name.
H         The backup host (the host that created the backup). If the ``c`` modifier is used (e.g. ``%cH``) the configured backup host is shown.
I         The numerical ID of the backup.
i         The numerical ID of the cluster to which the backup belongs.
J         The numerical ID of the job that created the backup.
M         The backup method used. If the ``c`` modifier is used the configured backup method will be shown.
O         The name of the owner of the backup.
P         The full path of the archive file.
R         The root directory of the backup.
S         The name of the storage host, the host where the backup was stored.
s         The size of the backup file measured in bytes.
t         The title of the backup. The can be added when the backup is created, it helps to identify the backup later.
v         The verification status of the backup. Possible values are "Unverified", "Verified" and "Failed".
%         The percent sign itself. Use two percent signs, ``%%`` the same way the standard ``printf()`` function interprets it as one percent sign.
========= ===================

**Examples**

Suppose we have a data node on 10.10.10.20 (port 3306) on cluster id 2, and we want to backup all databases using mysqldump and store the backup on ClusterControl server:

.. code-block:: bash

	$ s9s backup --create \
		--backup-method=mysqldump \
		--cluster-id=2 \
		--nodes=10.10.10.20:3306 \
		--on-controller \
		--backup-directory=/storage/backups

Create a mongodump backup on 10.0.0.148 for cluster named 'MongoDB ReplicaSet 3.2' and store the backup on the database node:

.. code-block:: bash

	$ s9s backup --create \
		--backup-method=mongodump \
		--cluster-name='MongoDB ReplicaSet 3.2' \
		--nodes=10.0.0.148 \
		--backup-directory=/storage/backups

Schedule a full backup using MariaDB backup every midnight at 1:10 AM:

.. code-block:: bash

	$ s9s backup --create \
		--backup-method=mariabackupfull \
		--nodes=10.10.10.19:3306 \
		--cluster-name=MDB101 \
		--backup-dir=/home/vagrant/backups \
		--on-controller \
		--recurrence='10 1 * * *'

Schedule an incremental backup using MariaDB backup everyday at 1:30 AM:

.. code-block:: bash

	$ s9s backup --create \
		--backup-method=mariabackupincr \
		--nodes=10.10.10.19:3306 \
		--cluster-name=MDB101 \
		--backup-dir=/home/vagrant/backups \
		--on-controller \
		--recurrence='30 1 * * *'

Create a pg_dumpall backup on PostgreSQL master server and store the backup on ClusterControl server:

.. code-block:: bash

	$ s9s backup --create \
		--backup-method=pgdump \
		--nodes=192.168.0.81:5432 \
		--cluster-id=43 \
		--backup-dir=/home/vagrant/backups  \
		--on-controller \
		--log

List all backups for cluster ID 2:

.. code-block:: bash

	$ s9s backup --list \
		--cluster-id=2 \
		--long \
		--human-readable

.. Note:: Omit the ``--cluster-id=2`` to see the backup records for all clusters.

Restore backup ID 3 on cluster ID 2:

.. code-block:: bash
	
	$ s9s backup --restore \
		--cluster-id=2 \
		--backup-id=3 \
		--wait

.. Note:: If the backup is encrypted, it will be decrypted automatically when restoring.

Create a job to verify the given backup identified by the backup ID. The job will attempt to install MySQL on the test server using the same  settings as for the given cluster, then restore the backup on this test server. The job returns OK only if the backup is successfully restored on the test server:

.. code-block:: bash

	$ s9s backup --verify \
		--log \
		--backup-id=1 \
		--test-server=192.168.0.55 \
		--cluster-id=1
		
Delete old backups for cluster ID 1 that are longer than 7 days, but do not delete at least 3 of the latest backups:

.. code-block:: bash

	$ s9s backup --delete-old \
		--cluster-id=1 \
		--backup-retention=7 \
		--safety-copies=3 \
		--log
