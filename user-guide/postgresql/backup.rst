Backup
-------

Provides interface for database backup management, restore, scheduling and report. Each backup will be assigned with a backup ID and ClusterControl creates a directory under *ClusterControl > Settings > Backup > Backup Directory* accordingly to store the backups based on this ID.

Backup Immediately
``````````````````

Creates an immediate backup of all databases. The backup will be created using :term:`pg_dumpall`. Backups can be stored on the database host that is performing the backup, or the files can be streamed to the ClusterControl host. The backup by this feature will be a full backup. 

* **Hostname**
	- The target database host.

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

Creates backup schedules. The backup will be created using :term:`pg_dumpall` against all databases.

* **Start Time**
	- Backup start time.

.. note:: The time is UTC on the ClusterControl server.

* **Backup Directory**
	- Enter a backup directory or use default path provided by ClusterControl under *ClusterControl > Settings > Backup*.

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

ClusterControl has ability to restore pg_dump backups created by ClusterControl. The following steps will be performed:

1. Copy backup files to the selected server (if the backup stored in controller).
2. Restore the backup using pg_restore. 
3. Follow the instruction in the *ClusterControl > Logs > Job > Job Message* on the restore status.

* **Backup Id**
	- Selected backup ID. This is auto picked if you click the *Restore Backup* button.

* **Restore backup on**
	- The backup will be restored to the selected server. This field will be disabled if the backup is stored in ClusterControl server.
	- If the backup is stored on the database node, ClusterControl assumes that you are going to restore the backup on the corresponding node.