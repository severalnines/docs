.. _changelog:

Change Logs
===========

This change logs list details about updates in each version of ClusterControl.

Changes in v1.5.0
-----------------

Initial Release: Nov 13th, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.5.0-4088
	- clustercontrol-controller-1.5.0-2224
	- clustercontrol-cmonapi-1.5.0-290
	- clustercontrol-notifications-1.5.0-67
	- clustercontrol-ssh-1.5.0-37
	- clustercontrol-cloud-1.5.0-31
	- clustercontrol-clud-1.5.0-31

In this release we have started to add integrations with cloud services and initially plan to add support for the major public cloud providers - Amazon Web Services, Google Cloud and Azure.

We are reintroducing backup to the cloud where you can now manually upload or schedule backups to be stored on AWS S3 and Google Cloud Storage. You can then download and restore backups from the cloud in case of local backup storage disasters or if you need to reduce local disk space usage for your backups.

For MySQL based clusters we have added support for MariaDB 10.2 and you can now choose to initially stage a slave from an existing backup instead of staging from a master. Individual databases (mysqldump only) can be backed up with separate dumps/files, and you can trigger verification/restoring of a backup to happen after N hours after a scheduled backup has completed.

PostgreSQL has an additional backup method ``pg_basebackup`` that can be used for online binary backups. Backups taken with ``pg_basebackup`` can be used later for point-in-time recovery and as the starting point for a log shipping or streaming replication standby servers. We have also added support for synchronous replication failover and deploying HAProxy with Keepalived (for load balancing HA) to be used with PostgreSQL clusters.

Load balancers (HAProxy) can be deployed by explicitly selecting the public/management IP for connecting and provisioning the software. Especially useful for cloud environments if you are provisioning/managing over a public network.

We also have some additional improvements for ProxySQL. You can add or modify schedulers and also mass import existing database users into your ProxySQL instances to quickly setup access.

**Feature Details**

* Cloud Services (AWS S3 and Google Cloud Storage):
	- Manual upload or schedule backups to be uploaded after completion to the cloud.
	- Download and restore backups from a cloud storage.

* Backup (MySQL):
	- Backup individual databases separately (mysqldump only).
	- Upload, download and restore backups stored in the cloud.
	- Trigger a verification and restore of a backup after N hours of completion.
	- Rebuild a replication slave by staging it from an existing backup.
	- Add a new replication slave by staging it from an existing backup.

* PostgreSQL:
	- New backup method ``pg_basebackup`` which makes a binary copy of the database files.
	- Synchronous replication failover (support for ``synchronous_standby_names``).
	- Support for HAProxy with Keepalived.
	- Support for PostgreSql 10.

* ProxySQL:
	- Mass import existing database users into ProxySQL.
	- Add and modify scheduler scripts.

* Misc:
	- MariaDB v10.2 support (Galera and MySQL Replication).
	- MySQL Cluster(NDB) v7.5 support.
	- Added support to show and filter DB status variables for MongoDB nodes.
	- HTML formatted alarm and digest emails.
	- Multiple NICs supports when deploying Load balancers (HAProxy).
	- Continuous improvements to UX/UI and performance.
	- New cmon-cloud process and clud client to handle cloud services.
	- New Report: Database Growth 

Changes in v1.4.2
-----------------

Patch Release: Oct 30th, 2017
``````````````````````````````

* Build:
	- clustercontrol-controller-1.4.2-2189

* Controller:
	- MySQL based cluster: if the 'mysql' database was explicitly backed up, then it was restored in the wrong way causing permission denied and the restore to fail.
	- Galera: codership repository fixes
	- Debian Jessie (Debian 9) support.

Patch Release: Oct 25th, 2017
``````````````````````````````

* Build: 
	- clustercontrol-1.4.2-3958
	- clustercontrol-controller-1.4.2-2179

* Controller:
	- Only collect the relevant log files from each host
	- Accounts daemon fix, to prevent doing any operations on accounts-daemon if running the environment as root or if it is not started.
	- Group-replication bugfixes
	- Galera: Add replicaiton slave: Properly detect if a replication slave is actually connected to the master.
	- error-reporter: include node type(s) in the host directory names
	- CmonDB 'alarm' table UTF-8 changes
	- HAProxy config check

* UI:
	- Resend alarm emails
	- Removed banner from the Add Existing Slave, making it hard to understand what would happen.
	- Set default value as "1" by default for Compression Level for mysqldump.
	- Galera: Overview Page, "Flow Control Paused" now shows floating points value
	- Host statistics graphs, issue with multicore CPU graphing
	- More verbosity when capturing LDAP logs
	- Configuration Management: Applied the byte conversion mechanism for the mysql change parameter dialog.
	- Fixed the save settings for property 'History' and removed property 'SSH Options'
	- ProxySQL: Query Rules, added IN () format to match pattern generation
	- Query Monitor: Adding query outliers explanation in Overview page
	- Query Monitor: Renamed Query Histogram to Query Outliers to match what it actually is.

Patch Release: Oct 3rd, 2017
`````````````````````````````

* Build:
	- clustercontrol-controller-1.4.2-2161

* Controller:
	- Backups: Always execute commands on controller, only use the seen address (from node's POV) for constructing the netcat sender command line.
	- s9s_error_reporter: Updates for better compatibility with all s9s cli version.
	- s9s_error_reporter: Prevent error reporting from being blocked by other jobs.
	- Deployment failure on MariaDB 10.2 and 10.1 for Galera Custer - mariadb-compat does not exist on Debian.
	- mysqldump: Handling the backup compression level (bugfix)
	- Galera (all vendors): ``mysql_upgrade`` must only run if ``monitored_mysql_root_password`` is set. The upgrade will failed if not possible to connect
	- Galera: Fix advisor to handle ``wsrep_cluster_address`` arguments

Patch Release: Oct 3rd, 2017
`````````````````````````````

* Build: 
	- clustercontrol-notifications-1.4.2-62
	- clustercontrol-ssh-1.4.2-32

* UI:
	- System V Init - Prevent/disable the 'cmon-events' process to start (by cron or manually) when ``<webroot>/clustercontrol/bootstrap.php`` has set ``define('CMON_EVENTS_ENABLED', false);``.
	- System V Init - Prevent/disable the 'cmon-ssh' process to start (by cron or manually) when ``<webroot>/clustercontrol/bootstrap.php`` has set ``define('SSH_ENABLED', false);``.

Patch Release: Sept 11th, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.4.2-3699
	- clustercontrol-controller-1.4.2-2091

* UI:
	- Non-default cluster specific SSH port support for host validation when adding a new or an existing node.
	- Show all valid nodes for 'Rebuild Replication Slave' and 'Change Replication Master'. All nodes with binary logging enabled is a valid option.
	- Minor filtering fixes to 'Manage -> Schemas and Users'.
	- Removed controller host from PostgreSQL's query monitor.
	- Minor performance optimization. Removed redundant repeated timezone call.

* Controller:
	- Use cluster specific SSH settings for host validation when adding a new or an existing node.
	- New error report tarball naming convention - error-report-TIMESTAMP-clusterCID.tar.gz.
	- Include backup records and backup schedules in the error reports.
	- Minor fix to backup scheduling when using advanced cron format.

Patch Release: August 25th, 2017
````````````````````````````````

* Build:
	- clustercontrol-controller-1.4.2-2063

* Controller:
	- HAProxy: A problem with hidden properties made it impossible to view HAProxy details in the UI unless the stats admin user and password was not admin/admin. 
	- Alarms: Possibility to disable the SwapV2 alarms (set ``swap_inout_period=0`` in cmon_X.cnf)

Patch Release: August 24th, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.4.2-3629

* UI:
	- Configuration Management: Correctly exclude non DB nodes from drop downs.

Patch Release: August 22nd, 2017
````````````````````````````````

* Build: 
	- clustercontrol-1.4.2-3607
	- clustercontrol-controller-2058

* UI:
	- Group Replication: SUDO password not set in job.
	- MySQL (all variants): Password validation updated to support more characters.
	- MySQL (all variants): Import existing MySQL cluster fails if specified user is other than ‘root’.

* Controller:
	- PostgreSQL: A problem restoring abackup on the specified node (by job: server_address, UI sends master/writable) is fixed.
	- Error reporting: Important error reporter fix to be more tolerant of empty/invalid filenames.
	- Replication: Cluster state was not set if node/cluster recovery was disabled.

Patch Release: August 14th, 2017
````````````````````````````````
* Build: 
	- clustercontrol-1.4.2-3574
	- clustercontrol-controller-1.4.2-2045

* UI: 
	- Group Replication: Create Cluster job did not submit the sudo password if set.
	- Galera: Restore backup host dropdown was empty unless the Galera node had log_bin enabled.
	- Postgres: small UI fix to remove empty columns.

* Controller:
	- MySQL(all variants)/PostgreSQL: use socat for streaming when it is available. 
	- MySQL (all variants): Super read-only causing create database to fail during restore.
	- MySQL (all variants): Backup, failed to read included config files from my.cnf (!includedir), if the included config dir was empty.
	- Error reporter: drop -W option from netstat (not supported by rhel/centos 6.x).
	- Error reporter: Add missing dependencies for error-reporter (tar/gzip) for minimal distros (eg.: containers.
	- MongoDb: Backup creation fix (for case when ssh user is not allowed to ssh to the controller itself).
	- ProxySQL: Installing an improved galera checker script for new ProxySQL installations.
	- ProxySQL: A fix to auto-restart a failed ProxySQL node.
	- Docker: Small fix to support HAProxy with Docker.
	- Docker: Do not set ulimit inside a container (as this makes some operation failing inside docker).
	- Query Monitor: Doesn't collect queries with mysql local override and PS=off.
	- Replication: do not recover a user shutdown node

Patch Release: August 1st, 2017
```````````````````````````````

* Build: 
	- clustercontrol-1.4.2-3538

* UI:
	- Fix password reset script for php v7.
	- Fix LDAP regression with Active Directory and "samba account".

Patch Release: July 31st, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.4.2-3531

* UI:
	- Fix host filtering for Query Monitor.
	- Fix LDAP login regression.
	- Fix to show all databases for Group Replication backups.

Patch Release: July 27th, 2017
``````````````````````````````````
* Build:
	- clustercontrol-ssh-1.4.2-26

* UI: 
	- Fix not fatal duplicated symlink error creation at post-installation.

Patch Release: July 24th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-1.4.2-3505
	- clustercontrol-notifications-1.4.2-57
	- clustercontrol-ssh-1.4.2-25
	- clustercontrol-controller-1.4.2-2013

* Controller:
	- ProxySQL log rotate: ProxySQL logs can grow big very fast.
	- PostgreSQL:  Improved master failure handling to prevent an old master from being accidentally restarted.
	- Galera/Replication: Adding a node did not update the loadbalancer HAProxy correctly. Xinetd was not started.
	- Minor fixes to printouts in cmon log file.

* UI:
	- Add support to disable automatic node discovery at import time for Galera cluster. Manually add IPs/hostnames.
	- Add support to filter by host for PostgreSQL's Query Monitoring.
	- Fix a race condition for ProxySQL graphs that would eventually consume all memory and crash the browser.
	- Fix escapes in match patterns for ProxySQL.
	- Remove execution flag for systemd service files for cmon-events and cmon-ssh.

Patch Release: July 11th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-3465

* UI:
	- Fix master selection dropdown for add node. No longer shows non-master nodes.
	- Fix transient node switching glitch in the nodes page.
	- Fix regression of minimum 2 SQL nodes at deployment (MySQL/NDB). No longer required.
	- Fix node selection dropdown when restoring a mysqldump. Only masters allowed.
	- Add standalone option when importing a MySQL Replication cluster.
	- Remove ProxySQL load balancer option with MySQL/NDB Cluster. Currently not supported.
	- Fix activity viewer next/prev causing page to scroll.
	- Fix missing sudo password if it was set when verifying/checking a host with deployment/add nodes.

Patch Release: July 4th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-controller-1981

* Controller:
	- Fixed a cmon grant error (for root and cmon passwords like "!password$$")
	- Skip .sst from db_growth calculation.
	- Restore mysqldump bugfix (for strange passwords)
	- Properly escape cmon password
	- Don't do smartctl on /dev/mapper devices at all
	- MySQL:
		- Deployment (MySQL5.7 templates): added ``ignore-db-dir=lost+found``
		- Backup: Improved passsword handling of backupuser
		- Backup: Add compression level for backups
	- ProxySQL:
		- Can't remove node when the node is unreachable
	- PostgreSQL:
		- Fix a minor systemd override file access rights issue.
		- Put slave to failed state when replication is known to be broken
		- Fix a minor systemd ``override.conf`` file access rights issue
		- An important bugfix for failover (the solution for the nodes stuck in 'startup' replication state)
	- MySQL Replication:
		- deeper external checks when there is a master failure. Try to connect from the slaves to the master using the mysql client to determine if the slave can see the master or get's an 2003/2013 error.
	- Galera:
		- Rolling-restart could fail due to an old value of the node's cluster size. Collect the wsrep variables before checking the cluster size and this is now done in a time controlled loop.

Initial Release: June 21st, 2017
``````````````````````````````````

* Build:
	- clustercontrol-1.4.2-3421
	- clustercontrol-controller-1969
	- clustercontrol-cmonapi-279
	- clustercontrol-notifications-14.2-53
	- clustercontrol-ssh-1.4.2-21

* ProxySQL:
	* Copy, Export and Import ProxySQL configurations to/from other instances to make them in sync.
	* Add Existing standalone ProxySQL instance.
	* Add Existing Keepalived in active/passive setups with ProxySQL.
	* Support for 3 ProxySQL instances with a Keepalived active/passive setup.
	* Simplified Query Cache creation.
	* Query hits column

* Backup:
	* Verify/Restore a mysqldump on standalone host that is not part of your clusters.
	* Verify/Restore an xtrabackup on standalone host that is not part of your clusters.
	* Customize your backup schedule by using the cron format.

* Notifications (clustercontrol-notifications):
	* Send Alarms and Events to PagerDuty, VictorOps, OpsGenie, Slack, Telegram or user registered Webhooks.

* Web SSH Console (clustercontrol-ssh):
	* Open a terminal window to any cluster nodes.
	* Only supported with Apache 2.4+.

* PostgreSQL:
	* New Master - Slave(s) cluster deployment wizard (streaming replication).
	* Automated failover and slave to master promotion.
	* Rebuild slave.

* Misc:
	* Fixed TLS connection issues for e-mail sending (SMTP).
	* Improved configuration handling of include/includeDir directives. 
	* Database user management RPC API for the s9s command line client.
	* Continuous improvements to UX/UI.
	* New cmon-events process to handle notifications to 3rd party services.
	* New cmon-ssh process to handle Web SSH console access.
	* Improved error reporting for troubleshooting/support.
	* Use a custom mysql port when adding a MySQL Asynchronous slave (MySQL Galera).


Changes in v1.4.1
------------------

Patch Release: June 20th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-1.4.1-3393

* UI:
	- Fix for a build issue on Ubuntu/Debian. 

Patch Release: June 19th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-1.4.1-3384

* UI:
	- Fix for setting the Settings->Backup's retention period. In future versions *Settings -> Backups* will be deprecated/removed and can be accessed from the Backup page instead.
	- Fix inconsistent backup executed and next execution time and timezones displayed. UTC timezone is used across the backup page for now.
	- *Performance -> Transaction Log* is disabled as default. Added a slider to set sampling interval.
	- 'Add Node' and 'Add Existing Node' now has a data directory input field to change the data directory used for the new node.

Patch Release: May 24th, 2017
``````````````````````````````````

* Build: 
	- clustercontrol-1.4.1-3181

* UI:
	- Alarm category in the Activity Viewer is now correctly showing the component name instead of the type name.
	- Fix to show correct server name in the individual server load graphs.
	- Fix regression/empty table for *Performance -> DB Variables*.
	- Fix to enable editable dropdown to the Add Existing Keepalived form for HAProxy.
	- Support for using a custom port when adding a MySQL Asynchronous Slave (MySQL Replication)
	- Fix for *Configuration Management -> Change* to list only valid nodes.
	- *Performance -> Status Time Machine* is now deprecated/removed.

Patch Release: May 20th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-controller-1902

* Controller:
	- Disable by default tx deadlock detection as it takes a lot of CPU. Added new param: ``db_deadlock_check_interval``
	- How often to check for deadlocks. 0 means disabled (default). Specified in seconds. (default: 0).
	- Enable in ``/etc/cmon.d/cmon_X.cnf`` (if you want to enable it, then 20 is a good value) and restart cmon.
	- Sample controller IP seen by MySQL nodes once after every cmon restart.
	- logrotate (wtmp) more often and restart accounts-daemon
	- A fix of ``show_db_users`` and ``show_db_unusued_accounts`` java scripts.

Patch Release: May 12th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-1.4.1-3121
	- clustercontrol-controller-1.4.1-1890
	- clustercontrol-cmonapi-274

* UI:
	- ProxySQL: Fix wrong IP in proxysql selected node header.
	- PostgreSQL fixes:
		- Overview page no longer cause high load on the web client
		- *Performance -> DB Variables* is now loading up correctly
		- Tooltips added for the graphs
	- LDAP authentication attempts are logged to a separate log file, ``{webdir}/clustercontrol/app/log/cc-ldap.log``
	- Minor improvements on how multiple recipients for email notifications are added.

* Controller:
	- Galera: Fixed a bug in clone cluster
	- Deployment: Fixed a bug using hostnames, which could cause grant/privilege errors from controller preventing the controller to connect to the managed nodes.
	- ProxySQL: hashing of passwords in the ``mysql_users`` table.
	- Backup Reports: Properly transform IP's into hostnames in backup report (due to a previous UI bug, some backups&schedules are used IPs instead of hostnames)
	- MongoDB: Degraded cluster state reported after removing shard

* CMONAPI:
	- Fixed an issue causing not all recipients to be listed under Settings (top menu) -> Email Notifications

Patch Release: April 24th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-1.4.1-3048
	- clustercontrol-controller-1.4.1-1856
	- clustercontrol-cmonapi-266

* UI:
	- Fix for empty databases list with MySQL backups.
	- MySQL Variables page now use the RPC API.
	- Improved deployment wizard placeholders descriptions.
	- Enable 'restore backup' for PostgreSQL.
	- Enable using a custom PostgreSQL port (default 5432) for deployments.
	- Fix for allowing negative port numbers in the load balancer forms.
	- Fix empty details on the keepalived node page.
	- Fix for saving timezone settings other than GMT+0 with email notifications.

* Controller:
	- Fix for deploying a single MySQL replication node cluster.
	- Require set 'force' to stop a read-write MySQL server (MySQL Replication).
	- Fix for node(s) reconnection issue to restored master after a restore backup.
	- Fix configuration (my.cnf) import to start immediately after a MySQL replication slave has been added (Galera) 
	- Job log improvement. Show the command/action that was requested.
	- Fix with MaxScale to show correct list of masters and slaves in the console.

Patch Release: April 12th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-1.4.1-3002
	- clustercontrol-controller-1.4.1-1834

* UI:
	- Fixed a bug making it impossible to restart failed jobs.
	- Fixed a bug in the Nodes graphs which made them render wrongly
	- Replication: Extended the Import dialog (Replication cluster) with a few more options (enable information schema queries).
	- Galera: Added Multi Nic support for Add Replication Slave
	- Fixed the title for the Nodes page
	- ProxySQL: Handle latency (us/ms) and improvements to graphs.
	- Query Monitor: Top queries useless with more than 20 queries
	- New Operation Report - Schema Change Report. With this feature you can spot changes in your database schemas and ensure changes are sound on your system.

* Controller:
	- Fixed a bug making it impossible add an existing replication slave.
	- Replication (Percona,MySQL): print out messages to show progress while applying relay log.
	- Java script fixes to take the enable_is_queries setting into account
	- SSH alarms re-organised and an alarm is raised if SSH access is determined to be too slow.
	- GroupRepl: fixing add-replication slave bug
	- A JS script to change password on all MySQL servers (mainly useful only for NDB)
	- ProxySQL: small fix for ‘latency’. Older versions used Latency_ms, newer Latency_us.
	- User option: ``enable_is_queries`` = 0|1
	- Detect schema changes (CREATE and ALTER TABLE. Drop table is not supported yet). New options: ``schema_change_detection_address``, ``schema_change_detection_databases``, ``schema_change_detection_pause_time_ms`` must be set in ``/etc/cmon.d/cmon_X.cnf`` to enable the feature. A new Operation Report (Schema Change) must be scheduled. 
	- Creating a report of 100 000 schemas and tables will take about 5-10 minutes depending on hardware. Configure the ``schema_change_detection_address`` to run on a replication slave or an async slave connected to e.g a Galera or Group Replication Cluster. For NDB this ``schema_change_detection_address`` should be set to a MySQL server used for admin purposes. Throttle the detection process with ``schema_change_detection_pause_time_ms. schema_change_detection_databases`` is a comma separated string of database names and also supports wildcards, e.g 'DB%', will evaluate all database starting with DB.


Initial Release: April 4th, 2017
``````````````````````````````````

* Build:
	- clustercontrol-1.4.1-2967
	- clustercontrol-cmonapi-1.4.1-257
	- clustercontrol-nodejs-1.4.1-86
	- clustercontrol-controller-1.4.1-1811

In this release we have added additional management functions for ProxySQL. You can now view queries passing through ProxySQL, create and edit query rules, host groups/servers, users and variables. We also have support for managing MySQL Galera and Replication clusters using separate managment and data/database IPs for improved security.

* ProxySQL (v2):
	- Support for MySQL Galera in addition to Replication clusters.
	- Support for active-standby HA setup with KeepAlived.
	- Use the Query Monitor to view query digests.
	- Manage Query Rules (Query Caching, Query Rewrite).
	- Manage Host Groups (Servers).
	- Manage ProxySQL DB Users.
	- Manage ProxySQL System Variables.

* Multiple Networks/Segmented Traffic
	- Manage MySQL Galera and Replication clusters with management/public IPs for monitoring connections and data/private IPs for replication traffic.
	- Add Galera nodes or Replication Read Slaves with managament and data IPs.

Changes in v1.4.0
-----------------

Patch Release: March 29th, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.4.0-2912
	- clustercontrol-controller-1.4.0-1798

* UI:
	- Create/Import NDB Cluster changes (remove the 15 node limitation)

* Controller:
	- Create NDB Cluster failed due to a bug in RAM detection.
	- Replication: Roles were not updated correctly when autorecovery was disabled.

Patch Release: March 13th, 2017
````````````````````````````````

* Build: 
	- clustercontrol-1.4.0-2812
	- clustercontrol-controller-1.4.0-1769

* UI:
	- Fix for 'Copy Log' to work again
	- Fix broken Galera SSL encryption indicator
	- Added support to change default ProxySQL listening port 
	- Further hostname fixes for ProxySQL
	- License handling fix with notifications
	
* Controller:
	- Added support to change default ProxySQL listening port
	- Syslog logging fix (command line param ``--syslog``) by adding ``ENABLE_SYSLOG=1`` into ``/etc/default/cmon`` file

Patch Release: Feb 28th, 2017
````````````````````````````````

* Build: 
	- clustercontrol-1.4.0-2743
	- clustercontrol-controller-1748

* UI:
	- Rebuild Replication Slave did not present available masters
	- ProxySQL deployment sends IP instead of hostnames when required 
	- Further improvements to handle RPC API token mismatches

* Controller:
	- Workaround to handle IP addresses instead of hostnames for ProxySQL deployments
	- Improvements to avoid create zombie processes
	- Remove false positive SSH alarms when using a hostname in the ``cmon.cnf`` file
	- Sending backup failure mails as "critical" notification


Patch Release: Feb 15th, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.4.0-2709
	- clustercontrol-controller-1725

* UI:
	- The Cluster list is no longer disappearing when the CMON process is either restarted, stopped or down
	- Rebuild slave/change master dialog correctly populates the nodes dropdown
	- Selecting a node action could at time cause a wrong dialog to show up
	- Improvements to RPC API Token mismatch error messages
	- 'Check for updates’ in the Settings page is deprecated/removed

* Controller:
	- Galera: ``wsrep_notify_cmd`` pointing to the script ``wsrep_notify_cc`` (discontinued) was invalidated wrongly.
	- Galera: Fixes in configuration to support 2.4.5 of Percona Xtrabackup and MariaDb Cluster 10.1, due to this bug https://bugs.launchpad.net/percona-xtrabackup/+bug/1647340.
	- Avoid samping from a failed node
	- Deployment: removed ``--purge`` from ``apt-get remove``, to handle ``/var/lib/mysql`` as a mountpoint.

Patch Release: Feb 8th, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.4.0-2659

* UI:
	- Correct filtering with config parameters in the Configuration Management
	- Read-Only switcher removed from the Overview Page. You can now only change the read-only status from the Nodes page's action menu
	- Fix issue with the Nodes page's action menu where the wrong action item was selected and could accidentally be performed instead
	- Improvements to the cluster and node status updates cycles. 
	- New <webdir>/clustercontrol/bootstrap.php variable to control refresh intervals: ``define('STATUS_REFRESH_RATE', 10000);``. Default is now 10s from before 30s.

Patch Release: Feb 5th, 2017
````````````````````````````````

* Build:
	- clustercontrol-controller-1.4.0-1703 

* Controller:
	- Permanently disabled the 'system_check.js' script as it was causing problems for some users
	- Automatic log rotate of ``/var/log/wtmp`` when it reaches 10MB in size. 10 files are stored for history, and runs at 02:00am.
	- Replication: A backup stored on the controller and restored on another host than the backup was created from would restore the backup on the wrong host (created host).
	- Replication: ``FLUSH LOGS`` after failover to update ``SHOW SLAVE HOSTS``.
	- Galera: Percona XtraDb Cluster 5.5 for Debian/Ubuntu failed to install.
	- Clear Alarms: specify ``send_clear_alarm=1`` in ``/etc/cmon.d/cmon_X.cnf`` and restart cmon to receive email notification when a Cluster Failure, SSH failure, MySQL Disconnected, Node/Cluster Failed Recovery, and Cluster Split alarms have been resolved. ``send_clear_cluster_failure`` is an alias for this option.
	- OS detection: Failed to detect Debian version if ``lsb_release`` was not installed.
	- Aborted jobs now have the correct status. 

Patch Release: Jan 24th, 2017
````````````````````````````````
* Build:
	- clustercontrol-1.4.0-2617

* UI
	- Fix for wrong scheduled time shown in Operational Reports.
	- Fix for inconsistent MongoDB menus.
	- Fix for confusing 'Change Organizations' option. 
	- You can more easily create a SuperAdmin/Root user to manage all your organizations/teams.

Patch Release: Jan 20th, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.4.0-2601
	- clustercontrol-controller-1.4.0-1675 

* UI
	- Manage -> Configurations: Wrong args sent to change_config_param.js script

* Controller
	- Fix of crashing bug during partial restore.
	- Graph missing from Operational Report.
	- Replication: Stop Slave (from UI) auto restarted the slave.
	- Adding a MySQL Node and having HAProxy caused a problem creating the s9smysqlchk user.

Patch Release: Jan 13th, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.4.0-2585

* UI
	- Fix for an issue with having clusters from multiple controllers in one UI.

Patch Release: Jan 11th, 2017
````````````````````````````````

* Build:
	- clustercontrol-controller-1.4.0-1651

* Controller
	- Migration of backups: better error messages and corrections the if backup files does not exist.
	- Sudo: corrects an issue where the sudo configuration (in case of using sudo with password) would overwrite the sudo settings.
	- Backup: an overlapping backup schedule will fail to execute and the user is prompted to correct the backup schedule.

Patch Release: Jan 3rd, 2017
````````````````````````````````

* Build:
	- clustercontrol-1.4.0-2542
	- clustercontrol-controller-1.4.0-1641

* Controller
	- New advisor: s9s/mysql/galera/check_gra_log_files.js monitors the growth of GRA log files.
	- ProxySQL failed to install on Centos/RHEL7 when mysql client is missing. 
	- SMTP/TLS bug improvements for email notifications.
	- Backup Retention: Backups matching the retention period as not removed.
	- Restore of Partial Backup (xtrabackup) shutdown the db nodes, but it is not necessary. 
	- Stop Garbd failed on Centos/RHEL7 .

* UI
	- Fix in the "enable/disable node/cluster recovery" to show a confirmation dialog when changing settings.
	- Small fix in query monitoring dialog.

Patch Release: Dec 22th, 2016
````````````````````````````````

* Build: 
	- clustercontrol-1.4.0-2527
	- clustercontrol-controller-1.4.0-1630

* Controller
	- New Advisor (Top Queries) and fixes.
	- Updated MySQL Group Replication (GA) to install from Oracle default MySQL repositories instead of MySQL Labs releases.
	- Improvements to support Galera 3.19.
	- Maintenance mode related fix for deployment jobs.
	- ProxySQL: additional deployment option (implicit transactions).
	- If 'vendor' is not set in the cluster's /etc/cmon.d/cmon_X.cnf file (X is the cluster id), then cmon will attempt to auto-detect the vendor. For MySQL based setups, please ensure the correct vendor is set to one of the following: percona, oracle, codership, mariadb. E.g vendor=mariadb, if you are using a mariadb based setup.

* UI
	- Query sampling time is no longer needed/used (Query Monitor settings).
	- Added option for Implicit Transactions (ProxySQL).
	- Text clarification when saving an existing DB user twice.
	- Fix for correctly saving mail server settings.
	- Fix for inconsistent password styles.

Initial Release: Dec 12th, 2016
````````````````````````````````

* Build:
	- clustercontrol-1.4.0-2491
	- clustercontrol-cmonapi-1.4.0-247
	- clustercontrol-nodejs-1.4.0-82
	- clustercontrol-controller-1.4.0-1614

* ProxySQL
	- Deploy ProxySQL on MySQL Replication clusters (support for additional database types coming).
	- Monitor ProxySQL performance (v1).

* Experimental support for Oracle MySQL Group Replication
	- Deploy Group Replication Clusters.

* HAProxy
	- Support Read-Write split configuration at deployment for MySQL Replication clusters.

* MySQL Replication
	- Enhanced multi-master deployment.
	- Flexible replication-topology management.
	- Replication error handling (Errant transactions).
	- Automated failover.

* MongoDB
	- Convert a ReplicaSet cluster to a sharded cluster.
	- Add or Remove shards from a sharded cluster.
	- Add Mongos/Routers to a sharded cluster.
	- Step down or freeze a node.

* New Advisors
	- Backup, Query Monitor and Advisors

* UI
	- A re-designed streamlined view into your scheduled and completed backups.
	- A re-designed Query Monitor with query execution plan output (explain) for MySQL.
	- A re-designed Advisors page that makes easier to see what needs to be acted upon.

* Misc
	- Support for Percona XtraDB Cluster 5.7
	- New Operational Report generating available software and security packages to upgrade.
	- New header with navigation breadcrumbs.
	- Activity Viewer showing Cluster Logs/Events. See more fine grained levels of logs and events generated and captured by ClusterControl.
	- Support for maintenance mode. Put individual nodes into maintenance mode which prevents ClusterControl to raise alarms and notifications during the maintenance period.

.. Attention:: Upload/Download backups to AWS S3 has been temporarily removed.

Changes in v1.3.2
-----------------

Path release: Oct 14th, 2016
````````````````````````````

* Build:
	- clustercontrol-1.3.2-2167
	- clustercontrol-controller-1.3.2-1504

* Controller
	- Allow two MongoDB Replica Set nodes to be deployed. Add an arbiter via 'Add Node'
	- Enable MariaDB 10.0 version for Repository mirroring

* UI
	- Fixes to database growth tables. Enable sorting on database or table columns 

Patch release: Sep 19th, 2016
``````````````````````````````

* Build:
	- clustercontrol-1.3.2-2066
	- clustercontrol-cmonapi-1.3.2-233
	- clustercontrol-controller-1.3.2-1455

* Controller
	- Support for v7.4.12 in Create/Deploy MySQL/NDB Cluster (starting from controller build #1446)
	- Option to select MongoDB consistent backup (https://github.com/Percona-Lab/mongodb_consistent_backup) is now properly shown for MongoDB Cluster if it is installed
	- Fix importing existing MySQL Cluster/NDB cluster (added mgm nodes)

* UI
	- Fix page refresh issues on Logs->Job
	- Fix saving confirmation issues to the Configuration Management (MySQL)
	- Fix empty Nodes->DB Variables page (MySQL) 
 

Patch release: Sep 5th, 2016
``````````````````````````````

* Build:
	- clustercontrol-1.3.2-2023
	- clustercontrol-controller-1.3.2-1431

* UI
	- Create/Import Cluster Wizard cosmetic fixes
	- Fix Operational Reports and MySQL User Management ACL settings for custom user profiles
	- Fix empty graphs on MongoDB Nodes->DB Performance page

* Controller
	- Fix a bug about restoring partial xtrabackups which did not work at all earlier. Now the partial xtrabackups are restored to a particular directory and the user must manually restore the tablespaces to the datadir.
	- Fix of a bug that in some situations could cause a node to not be fully removed.

Initial Release: Aug 8th, 2016
````````````````````````````````
* Build:
	- clustercontrol-1.3.2-1910
	- clustercontrol-cmonapi-1.3.2-226
	- clustercontrol-nodejs-1.3.2-73
	- clustercontrol-controller-1.3.2-1391

* MongoDB
	- Deploy or add existing MongoDB Sharded clusters (Percona MongoDB and MongoDB Inc v3.2)
	- Minor re-designed overview page for sharded clusters and performance graphs
	- Support for writing MongoDB based Advisors
	- Support for managing MongoDB configurations
	- Support for Percona consistent mongodb backup, https://github.com/Percona-Lab/mongodb_consistent_backup (if installed on the ClusterControl host)

* New Activity Viewer
	- Easily see Alarms and Jobs for all clusters consolidated in a single view

* New Deployment and Add Existing Cluster and Servers Dialog
	- Re-designed dialog for deploying and adding clusters
	- Supports MySQL Replication, MySQL Galera, MySQL/NDB, MongoDB ReplicaSet, MongoDB Shards and PostgreSQL

Changes in v1.3.1
-----------------

Patch release: Jul 28th, 2016
``````````````````````````````

* Build:
	- clustercontrol-controller-1.3.1-1372

* Controller
	- Fix for a new Percona 5.6 systemd script 
	- Fix for a new MariaDb 10.1 systemd script
	- Fix a busy loop issue (happening after some time with Proxmox provisioned LXC containers)
	- Recovery job marked as succeed when it is actually failed

Patch release: Jul 5th, 2016
``````````````````````````````

* Build:
	- clustercontrol-1.3.1-1820
	- clustercontrol-controller-1.3.1-1364
	- clustercontrol-cmonapi-1.3.1-215 

* Controller
	- Fix for digest mails (encoding and empty bodies) with MS Exchange
	- Fix for reports generation crashes

* UI
	- Fix for 'Create Database' returning 'unable to find host'
	- Support for HAProxy 1.6 new stats URL format
	- Moving File privilege to the Administration section for 'Create Account'
	- Updated AWS SDK to 2.8.30 and removed deprecated requirement on AWS SSH Private Key File


Patch Release: Jun 20th, 2016
````````````````````````````````

* Build:
	- clustercontrol-1.3.1-1655
	- clustercontrol-controller-1.3.1-1324
	- clustercontrol-cmonapi-1.3.1-198 

* Controller
	- Backup: Fixed an issue with long running backups and overrun of backup log entries (backup would not terminate properly)
	- Fix for automatically correcting a wrongful 'sudo' configuration.

* UI
	- Alarms: fixed inconsistent alarm count
	- Jobs: Fixed a number of issues such as being able to Restart failed jobs

Patch Release: Jun 16th, 2016
````````````````````````````````

* Build:
	- clustercontrol-controller-1.3.1-1304
	- clustercontrol-1.3.1-1580

* Controller
	- Galera: Fixed a version detection issue of the galera wsrep component.

* UI
	- Performance -> Database Growth: Fixed a JavaScript error.

Initial Release: May 31st, 2016
````````````````````````````````
* Build:
	- clustercontrol-1.3.1-1562
	- clustercontrol-controller-1.3.1-1296
	- clustercontrol-cmonapi-1.3.1-195
	- clustercontrol-nodejs-1.3.1-64

* MySQL based clusters
	- MySQL Replication
		- Create MySQL Replication Clusters (master + N slaves) with Percona (5.6|5.7), MariaDB (10.1) or Oracle (5.7) packages
		- Enable SSL client/server encryption
		- Enable/Disable automatic management of the server read_only variable by setting 'auto_manage_readonly=true|false' in the cmon.cnf file of the replication clusters. Default is true.
	- MySQL/NDB Cluster
		- Add Existing MySQL/NDB Cluster. Add an existing production deployed NDB Cluster. 2 MGMT Nodes, X SQL Nodes, Y Data Nodes.
	- New Backup and Restore options
		- Explicitly select a backup failover host to use instead of auto selecting a failover host
		- Improved restore mysqldump files
	- MySQL User Management
		- General UI improvements
		- Set accounts to require encrypted connections by enabling "REQUIRE SSL"

* Key Management
	- Import existing SSL certificates and keys. Upload your certificate, private key and CA (if any) to the ClusterControl Controller host and then import the certificate to be managed by ClusterControl. 

* Other
	- Support for installing ClusterControl on MySQL 5.7
	- Correctly show nodes that are in maintenance mode, e.g., during node recovery 
	- Simplified MariaDB MaxScale deployment. No need to enter a MariaDB enterprise repository URL
	- Added "Restart Node" action for all cluster types
	- Upgrade to CakePHP 2.8.3
	- Job Log improvements

Changes in v1.3.0
-----------------

Patch release: May 9th, 2016
``````````````````````````````

* Build:
    - clustercontrol-controller 1.3.0-1262

* ClusterControl Controller:
    - Ubuntu 15.04 fix to handle that my.cnf is a symlink
    - Missing SUPER privilege in Create Cluster causing the Incremental Xtrabackup to fail.

Patch release: May 3rd, 2016
``````````````````````````````

* Build: 
    - clustercontrol-1.3.0-1438
    - clustercontrol-controller 1.3.0-1257

* ClusterControl UI:
    - Permission problem in a web folder
    - Fix upgrade issue for 1.3.0 on centos/rhel

* ClusterControl Controller:
    - Fixed a compatibility issue with xtrabackup 2.2.x

Patch release: May 2nd, 2016
``````````````````````````````

* Build number:
    - clustercontrol-1.3.0-1420
    - clustercontrol-controller-1.3.0-1252

* ClusterControl UI:
    - Alllow 'strange characters' in user name (now all ASCII is supported except ` ´` ' ). UTF-8 characters are not supported. 
    - Made "Disable Firewall" default choice for Redhat/Centos when creating clusters.
    - A directory, WWWROOT/cmon, was never created during installation which affected uploading of files.
    - Postgres fixes to start a node from UI.
    - Wrong status for  nodes in MySQL Cluster.

* ClusterControl Controller:
    - MySQL standalone nodes were deployed as read only.
    - Mongo/HAProxy config file parsing issues fixed.
    - Failed to detect CentOS 6.6
    - Some settings (thresholds) set in the front-end was not respected by the controller.
    - Fixed a compatibility issue with xtrabackup 2.1.x.

Patch release: May 25th, 2016
``````````````````````````````

* Build number:
    - clustercontrol-controller 1.3.0-1242

* ClusterControl Controller:
    - mysqldump fails for MariaDb 10.x with an erroneous parameter being used.  

Patch release: Apr 24th, 2016
``````````````````````````````

* Build number:
    - clustercontrol-1.3.0-1393
    - clustercontrol-controller 1.3.0-1240

* ClusterControl UI:
    - New "Install Software" option for Galera Cluster with "Create Database Cluster" and "Create Database Node"
    - Default "Yes" act as before where ClusterControl provisions the database nodes with required packages and any existing packages could be uninstalled if required.
    - If set to "No" then no provisioning of packages or uninstallation of any existing packages are done. It is assumed that the DB nodes have been provisioned by for example a configuration management system with all required database packages. The create cluster/node jobs will then only provision out our Galera my.cnf file and then bootstrap the cluster without doing any provisioning of software. It is important that the mysql server process is stopped before running the job with "install Software" set to "No".
		- MongoDB arbiter is now shown on the "Nodes" page

* ClusterControl Controller:
    - Correct wrong assets path. Fixes missing logo in operational reports.
    - Manual fix: Move /usr/share/cmon/assets/assets to /usr/share/cmon/assets 
    - Support for "Install Software" option for Galera Clusters with "Create Database Cluster" and "Create Database Node"

Patch release: Apr 21st, 2016
``````````````````````````````

* Build number: 
    - clustercontrol-1.3.0-1375

* UI:
    - Fix broken Add Existing Server/Cluster dialog. 

Patch release: Apr 19th, 2016
``````````````````````````````

* Build number:
    - clustercontrol-controller 1.3.0-1234
    - clustercontrol-1.3.0-1355

* ClusterControl Controller:
    - Prefer "netcat-openbsd" over other variants when provisioning a node.
    - epel-release URL fix for Centos 7 (using time-proof urls).
    - Auto schema upgrade fixes in /etc/init.d/cmon
    - The cmon init script in 1.3.0 automatically tries to upgrade the cmon schema to the current version. 

* ClusterControl UI:
    - Create Cluster Job: Remove unused/wrong keys from the json format. 
    - Key Management: Fix reload issues with manage key's content table.
    - Manage-Hosts: Fix Unknown status for HAProxy and Keepalived. 

Initial Release: Apr 18th, 2016
``````````````````````````````````

* Build number:
    - clustercontrol 1.3.0-1347
    - clustercontrol-controller 1.3.0-1228
    - clustercontrol-cmonapi 1.3.0-183
    - clustercontrol-nodejs 1.3.0-56

* Key Management allows you to manage a set of SSL certificates and keys that can be provisioned on your clusters
    - Create certificate authority certificates or self-signed certificates and keys
    - Easily Enable and Disable SSL encrypted client-server connections for MySQL and Postgres based clusters

* Additional Operational Reports
    - Generate an Availability Summary of uptime/downtime for your managed clusters and see node availability and cluster state history during the reported period
    - Generate a backup summary of backup success/failure rates for your managed clusters 

* Improved Security
    - From this version we are setting an unique Controller RPC API Token which enables token authentication for your managed clusters. No user intervention is needed when upgrading older ClusterControl versions. An unique token will be automatically generated, set and enabled for existing clusters.
    - Custom scripts/applications utilizing the RPC API need to pass the correct token for the clusters, see http://severalnines.com/downloads/cmon/cmon-docs/current/ccrpc.html... for details on how to pass the token correctly. 

* Create/Mirror Repository
    - Mirror your database vendor’s software repository without having to actually deploy a cluster. A mirrored local repository is used in scenarios where you cannot upgrade a cluster and must lock the db versions to use.

* Additional Backup Retention Periods
    - Enable shorter retention periods

* MySQL based clusters
    - NDB/MySQL Cluster
        - Create a production setup of NDB/MySQL Cluster from ClusterControl
        - Deploy Management Nodes, SQL/API Nodes and Data Nodes
    - MySQL Replication/Standalone 
        - Easily toggle read-only mode on and off for MySQL nodes

* MongoDB based clusters
    - Create MongoDB ReplicaSet Node
    - Support for Percona MongoDB 3.x
    - MongoDb 2.x is no longer supported.

Changes in v1.2.12
------------------

Patch release: Apr 3rd, 2016
``````````````````````````````

* Build number: 
    - clustercontrol-controller-1.2.12-1201
    - clustercontrol-1.2.12-1261

* ClusterControl Controller:
    - Javascript fixes (1) : schema_check_nopk.js and schema_check_myisam.js did not always complete.
    - Javascript fixes (2):  validate_sst_auth.js did not complete ok if the wsrep_sst_auth parameter contained quotes.
    - xtrabackup failed if there monitored_mysql_root_user was anything else than ‘root’, i.e the value of monitored_mysql_root_user in cmon.cnf was not respected.
    - xtrabackup failed if executed on an asynchronously slave connected to a Galera node.

* ClusterControl UI: 
    - MongoDb: Shards was not presented correctly

Patch release: March 20th, 2016

* Build number:
    - clustercontrol-controller-1.2.12-1184
    - clustercontrol-1.2.12-1261

* ClusterControl Controller:
    - Restore: Copying files larger than 2GB failed.
    - Clear alarms when removing a node
    - Galera: Setting up asynchronous slave connected to Galera failed for MariaDb 10.x 

* ClusterControl UI: 
    - MaxScale: displayed as a slave in the Overview
    - MongoDb: Shards was not presented correctly
    - MySQL Transaction Log: Pagination issue 

Patch release: March 4th, 2016
````````````````````````````````

* Build number: 
    - clustercontrol-controller-1.2.12-1158
    - clustercontrol-1.2.12-1195
    - clustercontrol-cmonapi-1.2.12-171. 

* ClusterControl Controller:
    - Very old backup schedules could sometimes cause problems
    - Improved handling for checking mount points that does not exits
    - Query Monitor: Running Queries did not always show b/c of a problem in processlist.js
    - MySQL User Privileges: edit privileges failed due to bug in javascript
    - Missing explains

* ClusterControl UI: 
    - Occasionally upgrades could fail because a UI cache was not cleared
    - LDAP fixes related to issues when upgrading from 1.2.10 to 1.2.12
    - Showed too many node types in Query Monitor -> Running Queries drop down
    - Missing possibility to hide graphs opened by 'Show Servers'

* ClusterControl CMONAPI:
    - Fixes to queries showing explains


Initial Release: Feb 25th, 2016
````````````````````````````````

* Build number:
	- clustercontrol-1.2.12-1007
	- clustercontrol-cmonapi-1.2.12-156
	- clustercontrol-controller-1.2.12-1096
	- clustercontrol-nodejs-1.2.12-51

* Operational Reports (BETA). Generate, schedule and email out operational reports. The current default report shows a cluster's health and performance at the time it was generated compared 1 day ago.
	- The report provides information on Node availability, Backup summary, Top queries, Host and Node stats. We will add more options and report types in future releases.

* Custom Advisor dialog. Create threshold based advisors with host or MySQL stats without needing to write your own JS script.

* Notification Services (new clustercontrol-nodejs package). Currently only email and pagerduty notifications are used by custom advisors. More to come. 

* Local Mirrored Repository. Create a local mirror of a database vendor's software repository. This allows you to "freeze" the current versions of the software packages used to provision a database cluster for a specific vendor and you can later use that mirrored repository to provision the same set of versions when adding more nodes or deploying other clusters.

* Export graphs as CSV, XLS files

* Search the content in the system logs

* MySQL based clusters
	- Galera
		- MariaDB 10.1 support.
		- Enable binary logging for a node. This node can then be used as the master for a replication slave or use the binary log for point in time recovery.
		- Delayed replication option when adding slave to the Galera Cluster. Delay the replication with N seconds.
		- Enable/Disable SSL encryption of Galera replication links.

	- MySQL Replication Master
		- Oracle MySQL 5.7 as vendor. Limitation: Percona Xtrabackup is not supported for MySQL 5.7 yet.
		- Semi-sync replication option
		- Find the most advanced MySQL slave server to use for Master promotion

	- MySQL Replication Slave
		- Delayed replication option (MySQL 5.6). Delay the replication with N seconds.
		- New table lists delayed replication slaves in the cluster

	- New Backup options
		- Auto Select backup host. Allow ClusterControl to automatically select which node to take the backup on.
		- Enable backup failover node. If the selected backup node is down a failover node will be elected. 
		- Galera: De-syncing a node with the highest local index and then used as the backup failover node.
		- MySQL Replication: Random slave node used as the backup failover node.
		- "No backup locks" for xtrabackup/innobackupex. Use FLUSH NO_WRITE_TO_BINLOG TABLES and FLUSH TABLES WITH READ LOCK instead of LOCK TABLES FOR BACKUP.

	- Configuration Management
		- Manage, Garbd and MaxScale configurations. Limitation: Maxscale does not support 'reload' (https://mariadb.atlassian.net/browse/MXS-99)  meaning the operator must restart (e.g from the UI) the maxscale daemon.

* MongoDB
	- Support for MongoDB 3.2

* Postgres
	- Support for Postgres 9.5

Changes in v1.2.11
------------------

*Dec 11th, 2015*

* Build number:
	- clustercontrol-1.2.11-899
	- clustercontrol-cmonapi-1.2.11-141
	- clustercontrol-controller-1.2.11-1052

* Controller:
	- Backup: supports group [mysqldump] in my.cnf file 
	- Developer Studio:  Fixed bugs in import/export of advisors
	- Scalability fix: Use poll instead of select

*Dec 4th, 2015*

* Build number:
	- clustercontrol-1.2.11-899
	- clustercontrol-cmonapi-1.2.11-141
	- clustercontrol-controller-1.2.11-1039

* UI
	- Finer granularity on Range Selection without using date selector (15 mins, 30 mins, 45 mins)
	- Removed obsolete data columns (Connections and Queries) from cluster bar
	- Role and Manage Organizations fixes

* Controller:
	- Fixed a bug when using internal repos
	- A config file parser fix for include files (parser tried to treat a directory as a file)
	- NDB nodes statues was reported as "9999" (mysql-unknown) when auto-recovery is disabled
	- Mariadb repo creation bugfix 
	- Fixed a crashing bug when having many clusters on one controller. 

*Nov 15th, 2015*

* Build number:
	- clustercontrol-1.2.11-883
	- clustercontrol-cmonapi-1.2.11-138
	- clustercontrol-controller-1.2.11-1023

* UI
	- Fixes to User/Organization management 

* Controller
	- Xtrabackup: corrected --no-timestamp option (was -no-timestamp)
	- Implemented max-request-size handling for the REST API calls to limit transfers between the controller and REST consumers (such as the UI)
	- MySQL Cluster
		- Stop Node job could fail unnecessarily.  / Start Node job stuck in RUNNING state for too long.
	- Keepalived
		- Corrected vrrp_script chk_haproxy (was rrp_script chk_haproxy)

*Nov 6th 2015*

* Build number:
	- clustercontrol-1.2.11-854
	- clustercontrol-cmonapi-1.2.11-135
	- clustercontrol-controller-1.2.11-1007

* UI
	- Default "Admin" Role is missing ACLs settings for Create DB Node and Dev Studio
	- When viewing Global Jobs, the installation Progress window cannot be resized vertically.
	- DB Variables page does not load properly
	- Find Most Advanced Node job sent with the wrong cluster id (0) causing it to fail.

* Controller:
	- Postgres: postgres|postmaster executable names are both supported meaning that the postmaster process is now properly handled.
	- Javascript: s9s/host/disk_space_usage.js could not handle multiple partitions
	- Javascript: s9s/mysql/schema/schema_check_*.js  - prevent it to run if there are > 1024 tables (configurable) to prevent I_S caused stalls.
	- Reading disk partition information failed as non root user 

*Nov 2nd, 2015*

* Build number:
	- clustercontrol-1.2.11-842
	- clustercontrol-cmonapi-1.2.11-135
	- clustercontrol-controller-1.2.11-998

* Bugs fixed
	- UI
		- Change the favicon for ClusterControl to the one that is used on our site www.severalnines.com
		- MongoDB add node to replica set looks wrong
		- Global Job Messages: Local cluster jobs are shown in the popup dialog
		- Fix in Manage -> Schema Users. Drop user even if user is empty (‘’@‘localhost’)
		- Add/Register Existing Galera Node: The "Add Node" button does not react/work if there is no configuration files in the dropdown for the "Add New DB Node" form
		- MongoDB add node to replica set dialog - text was cut
		- Add/Register Existing Galera Node: The "Add Node" button does not react/work if there is no configuration files in the dropdown for the "Add New DB Node" form
		- [PostgreSQL] Empty "DB Performance" graphs
		- Installation progress window text disappears while scrolling back
	- Controller:
		- Galera: Register_node job: registers node with wrong type
		- Create DB Cluster: Checking OS is the same on all servers
		- Create DB Cluster/Node, Add Node: Install cronie on Redhat/Centos
		- Scheduled backups that are stored both on controller and on node (full and incremantals) fail to restore. 
		- Increase size of ‘properties’ column in server_node table to contain 16384 characters. The following is needed on the cmon db: ALTER TABLE server_node MODIFY properties VARCHAR(16384) DEFAULT '';
		- Character set on connection + cmon.tx_deadlock_log, change to use utf8mb4 to properly encode characters in Performance -> Transaction Log preventing data from being shown. Do mysql -ucmon -p -h127.0.0.1 cmon < /usr/share/cmon/cmon_db.sql to recreate this table.

		- CmonHostManager::pull(..): lets properly handle if JSon parse failes...
		- MongoDB
			- Check if there is a new member in the replica set and then reload the config
		- MySQL
			- Bugfix for replication mysqldump backuping issues (appeared recently): lets exclude the temporary (name starts with #) DBs from backup
		- Postgres
			- Add existing replication slave failed.
	
*Oct 23rd, 2015*

* Build number:
	- clustercontrol-1.2.11-826
	- clustercontrol-cmonapi-1.2.11-131
	- clustercontrol-controller-1.2.11-985

* Controller:
	- Backup fix to support xtrabackup 2.3
	- Start-up bugs to initialise internal host structures
	- netcat port defaults to 9999 (and impossible to change)
	- Cluster failure with "Unknown database some_schema" message
	- Remove Node: wsrep_cluster_address is not updated
	- Corrected printout in backup
	- Corrected sampling of wsrep_flow_cntr_sent/recv

* UI:
	- In Cluster jobs list, Delete and Restart buttons do not work
	- Add Replication Slave UI Dialog not showing properly
	- Editing a previously created backup schedule alters the hostname, and backup job fails
	- Number counter on 'Alarms' and 'Logs' tabs doesn't make sense
	- User Management - refresh/reload button and corrected text for CREATE USER

*Oct 16th, 2015*

* Build number:
	- clustercontrol-1.2.11-808
	- clustercontrol-cmonapi-1.2.11-128
	- clustercontrol-controller-1.2.11-974

* This is a our best release yet for Postgres with a number of improvements.
	- Create a new Postgres Node/cluster from the "Create Database Node" dialog or add an new node with a few clicks 
	- You can now easily add a new replication slave for your Postgres master node
	- The replication peformance and status is shown on the overview page for the slave
	- You can restore a backup created by ClusterControl on a specific node
	- Create your own dashboard with stats to chart/graph on the overview page like MySQL based clusters
	- DB performance charts on the Nodes page
	- View database status and variables on your postgres nodes side by side
	- Enabled DevStudio, i.e., our JavaScript based advisors which was introduced in 1.2.10 for Postgres as well
	- Create your own postgres "advisors/DB minions" for alarms or email notifications

* MaxScale for MySQL based clusters. MariaDB MaxScale is an open-source, database-centric proxy that works with MariaDB Enterprise, MariaDB Enterprise Cluster, MariaDB 5.5, MariaDB 10 and Oracle MySQL.
	- Deploy MaxScale instance for round-robin or read/write splitter with a customizable configuration  
	- Add an existing running MaxScale instance
	- Send commands to "maxadmin" and view the output in ClusterControl

* MySQL Based Clusters.
	- You can now use CoderShip as the Galera vendor for Create Cluster and Database node
	- Create a MySQL Replication Master Node from the Create DB Node dialog. Currently only Percona as vendor is supported
	- Add/Register an existing running MySQL slave without stopping and provisioning the dataset from the cluster
	- Create Cluster and Database Node now support using "internal repositories" for environments where you do not have internet access and have internal repostory servers instead
	- Removed the limit of only being able to chart 8 DB stats. You can now arrange the charts in a layout with 2 or 3 columns and chart up to 20 stats
	- Fixes to Clone Cluster and the UI notification system/look
	- Backup individual schemas
	- Option to enable 'wsrep_desync' during backup for Galera clusters to workaround stalls/issues with FLUSH TABLES WITH READ LOCK. Puts the backup node into 'Donor/Desynced' state during the backup.
	- Manage Email Notifications for all users at once
	- New Database Logs page
		- We have a new page specifically for database logs that you access from Logs->Database Logs
		- A tree view lists your DB nodes so you can simply pick the nodes that you want to check the mysql error log for
	- Revamped Configuration Management
		- New implementation and look using our JS engine and a set of js scripts
		- Group Changes. Automatically change and persist individual database variables across your DB nodes at once. If it's a dynamic variable we'll change it directly on the nodes
	- Revamped MySQL User Management
		- New implementation and look using our JS Engine and a set of js scripts
		- We' removed the old implementation where we maintained users created from ClusterControl separately
		- Users and privileges are set directly and retrieved from your cluster so you are always in sync
		- Create your users across more than one cluster at once

* HAProxy and KeepAlived
	- You can now add existing running HAProxy and Keepalived instances that have been installed outside of ClusterControl
	
Patch release: Oct 23rd, 2015
``````````````````````````````

* Build number:
	- clustercontrol-1.2.11-826
	- clustercontrol-cmonapi-1.2.11-131
	- clustercontrol-controller-1.2.11-985

* Bugs fixed:
	- UI:
		- In Cluster jobs list, Delete and Restart buttons do not work
		- Add Replication Slave UI Dialog not showing properly
		- Editing a previously created backup schedule alters the hostname, and backup job fails
		- Number counter on 'Alarms' and 'Logs' tabs doesn't make sense
		- User Management - refresh/reload  button and corrected text for CREATE USER
	- Controller:
		- Backup fix to support xtrabackup 2.3
		- Start-up bugs to initialise internal host structures
		- netcat port defaults to 9999  (and impossible to change)
		- Cluster failure with "Unknown database some_schema" message
		- Remove Node: wsrep_cluster_address is not updated
		- Corrected printout in backup
		- Corrected sampling of wsrep_flow_cntr_sent/recv

Patch release: Nov 2nd, 2015
``````````````````````````````

* Build number:
	- clustercontrol-1.2.11-842
	- clustercontrol-cmonapi-1.2.11-135
	- clustercontrol-controller-1.2.11-998

* Bugs fixed:
	- UI:
		- Change the favicon for ClusterControl to the one that is used on our site www.severalnines.com
		- MongoDB add node to replica set looks wrong
		- Global Job Messages: Local cluster jobs are shown in the popup dialog
		- Fix in *Manage > Schema Users*. Drop user even if user is empty (‘’@‘localhost’)
		- Add/Register Existing Galera Node: The "Add Node" button does not react/work if there is no configuration files in the dropdown for the "Add New DB Node" form
		- MongoDB add node to replica set dialog - text was cut
		- Add/Register Existing Galera Node: The "Add Node" button does not react/work if there is no configuration files in the dropdown for the "Add New DB Node" form
		- [PostgreSQL] Empty "DB Performance" graphs
		- Installation progress window text disappears while scrolling back
	- Controller:
		- Galera: Register_node job: registers node with wrong type
		- Create DB Cluster: Checking OS is the same on all servers
		- Create DB Cluster/Node, Add Node: Install cronie on Redhat/Centos
		- Scheduled backups that are stored both on controller and on node (full and incremantals) fail to restore. 
		- Increase size of ‘properties’ column in server_node table to contain 16384 characters. The following is needed on the cmon db: ``ALTER TABLE server_node MODIFY properties VARCHAR(16384) DEFAULT '';``
		- CmonHostManager::pull(..): lets properly handle if JSon parse failes.
		- MongoDB: Check if there is a new member in the replica set and then reload the config.
		- MySQL: Bugfix for replication mysqldump backuping issues (appeared recently): lets exclude the temporary (name starts with #) DBs from backup
		- Postgres: Add existing replication slave failed.
		- Character set on connection + cmon.tx_deadlock_log, change to use utf8mb4 to properly encode characters in *Performance > Transaction Log* preventing data from being shown. Do ``mysql -ucmon -p -h127.0.0.1 cmon < /usr/share/cmon/cmon_db.sql`` to recreate this table.

Patch release: Nov 6th, 2015
`````````````````````````````

* Build number:
	- clustercontrol-1.2.11-854
	- clustercontrol-cmonapi-1.2.11-136
	- clustercontrol-controller-1.2.11-1007

* Bugs fixed:
	- UI:
		- Default "Admin" Role is missing ACLs settings for Create DB Node and Dev Studio
		- When viewing Global Jobs, the installation Progress window cannot be resized vertically.
		- DB Variables page does not load properly
		- Find Most Advanced Node job sent with the wrong cluster id (0) causing it to fail.
	- Controller:
		- Postgres: postgres|postmaster executable names are both supported meaning that the postmaster process is now properly handled.
		- Javascript: ``s9s/host/disk_space_usage.js`` could not handle multiple partitions.
		- Javascript: ``s9s/mysql/schema/schema_check_*.js`` - prevent it to run if there are more than 1024 tables (configurable) to prevent I_S caused stalls.
		- Reading disk partition information failed as non root user.

Changes in v1.2.10
------------------

*May 27th, 2015*

* Introducing our new powerful ClusterControl DSL (Domain Specific Language) which allows you to create Advisors, AutoTuners or "mini Programs" on our ClusterControl platform! (BETA)
	- JavaScript based language syntax (not 100% JavaScript compatible) with extensions to provide access to ClusterControl's internal data structures and functions!
	- Allows you to execute SQL statements and/or run shell commands/programs across all your cluster hosts and retrieve results to be processed for advisors/alerts/actions etc.
	- SDK documentation 

* Integrated Developer's Studio (Developer IDE)
	- Provides a simple but elegant environment to quickly create/edit, compile, run/test and schedule your JS programs. 

* ClusterControl Advisors/JS bundle for MySQL based clusters - feel free to modify and share your changes with the community!
	- A set of basic advisors with rules, alerts and actions that you can use as a base for your own customizations.

* Import ClusterControl JS bundles from the community or our partners. 
* Export ClusterControl JS bundles for others to use/try out. 
* Galera Cluster
	- Create a Galera Cluster with up to 9 nodes for local/on-premise deployments.
	- New cluster action that shows you the most advanced (last committed) node in your cluster, simplifying manual cluster recovery.
	- Show long running and deadlocked transactions, great for performance tuning.
	- Actions that can be performed on a Node is now also available directly from the overview page.
	- New Add Node option to Add an Existing DB Node, i.e., a node that has been provisioned without ClusterControl.  
* MySQL Replication clusters using GTIDs support Failover and Slave Promotion (manual). 
* Overview page's cluster load graph and the Nodes's page graphs have been migrated to use the faster CMON RPC API.
* Configuration Management uses the CMON RPC API to manage configurations.
* General frontend optimizations for better UI performance.
* Fixed bugs in the SSL/TLS email protocol

Changes in v1.2.9
-----------------

*Feb 8th, 2015*

* MySQL Replication (master <-> master) should not upgrade.
* Support for PostgreSQL Servers!
	- Add Existing PostgreSQL Server (standalone). Only v9.x supported. 
	- Monitor and schedule backups
	- Query Monitor

* Port 9500 must be open on the controller for internal communication between UI and the CMON process
* Port 9999 (by default) must be open bi-directionally between controller and data nodes for streaming backups (mysqldumps, xtrabackup, pgdump)
* Galera Cluster
	- Bootstrap Cluster. Select a DB node to initialize the cluster from. Optionally enable/force SST for joining nodes and forcefully stop (SIGKILL) nodes
	- Stop Cluster forcefully (SIGKILL) or with a graceful shutdown time
	- Start DB node. Optionally enable SST at startup
	- Stop DB node forcefully (SIGKILL) or with a graceful shutdown time 
	- Make a non-primary DB node primary
	- Replication Slave Setup for Galera Cluster (GTID support). Slaves are bootstrapped with a Xtrabackup stream from a chosen Master
	- Failover replication (GTID only) slave from to a new master
	- Stage replication slave from master (Xtrabackup streamed from master to slave), useful in event of slave corruption
	- Enable SSL Replication Encryption on the Galera Cluster. 2048-bit default key and certificate generated on the ClusterControl node and transferred to all the Galera nodes automatically
	- SSL support between controller and managed nodes
	- wsrep-recover is used to discover the most advanced Galera Node for recovery operations
	- Removed manipulation of wsrep_cluster_address in my.cnf files meaning ClusterControl no longer makes any alterations of a node's configuration file
	- Backup functionality completely re-written, and netcat port for streaming backups is user specified
	- Restore ClusterControl originated or external made backups on selected hosts
	- Alarm is raised if a node has set wsrep_cluster_address=gcomm://
	- Improved logging and hints to assist with failed recovery attempts
	- Enable/Disable Node/Cluster Auto Recovery from UI

* Advanced HAProxy Deployment Settings 
	- Set for example client and server timeouts, max connections for frontend and backend. Select which backend servers are 'active' or 'backup'
	- It is possible to enable/disable nodes part of a load balancer.
	- Built-in HAProxy Statistics. No longer need to launch separate window to monitor the HAProxy performance
	- Template configuration is stored on the controller in /usr/share/cmon/templates/haproxy,cfg ,  mysqlchk.*, and mysqlchk_xinetd and allows for pre-install modifications.

* Deadlock and long running queries detection
	- ``db_long_query_time_alarm`` (specify in cmon.cnf). If a query takes longer than ``db_long_query_time_alarm`` to execute an alarm will be raised containing detailed information about blocked and long running transactions. ``db_long_query_time_alarm = 0`` (disable), default value 5

* MySQL Replication / Single MySQL Server
	- Failover replication (GTID only) slave from to a new master
	- Stage replication slave from master (Xtrabackup streamed from master to slave), useful in event of slave corruption

* MongoDB Cluster
	- New Overview page with global lock stats.

* A new more “modern” front-end theme
	- Re-organized Cluster specific actions into an easy to access list.
	- A global alarm list which shows alarms per cluster. No need to drill into each cluster to see the alarms anymore. 

* Deprecated scripts (most of the below functionalities are now handled directly by the Controller process):
	- s9s_haproxy 
	- s9s_backup/s9s_backupc
	- s9s_galera (—install/remove-garbd)
	- s9s_sw_update deprececated for mariadb/percona apt/yum installs

* Chef recipe & Puppet manifest for ClusterControl Controller (CMON)

* Zabbix Template, see http://www.severalnines.com/blog/clustercontrol-template-zabbix

* Changes in the Controller (CMON)
	- New configuration options (cmon.cnf):
		- enable_mysql_timemachine =[0|1]  ,  default is 0, meaning it is disabled.
		- cmondb_ssl_key= path to SSL key, for SSL encryption between CMON and the CMON DB.
		- cmondb_ssl_cert = path to SSL cert, for SSL encryption between CMON and the CMON DB
		- cmondb_ssl_ca = path to SSL CA, for SSL encryption between CMON and the CMON DB
		- cluster_ssl_key= path to SSL key, for SSL encryption between CMON and managed MySQL Servers.
		- cluster_ssl_cert = path to SSL cert, for SSL encryption between CMON and managed MySQL Servers.
		- cluster_ssl_ca = path to SSL CA, for SSL encryption between CMON and managed MySQL Servers.
		- cluster_certs_store = path to storage location of SSL related files, defaults to ``/etc/ssl/<clustertype>/<cluster_id>``
	- Monitoring:
		- New binary format for host statistics which consumes less space (cpu, memory, disk, network stats).
		- Fixed disk statistics collector to support non 4K block sizes.
	- Security:
		- E-mails do not contain IP addresses when hostnames are specified in the cmon configuration
		- Password will not be logged (to jobs for example) or sent anymore
	- Alarms:
		- Alarm will be raised when there is a missing MySQL GRANT
		- Alarm will be raised/sent when there is a high IO wait for a period (>=50% average in 10 minutes)
		- New alarm for Galera configuration problems
		- Improved alarm emails (for example: high cpu/mem usage mails will contain the output of 'top' command)
	- RPC:
		- Several new RPC interfaces (directly on the daemon) for jobs and statistics handling
		- The web client has started to migrate over to use RPC API calls instead of the CMON API
	- Testing:
		- Acceptance testsuite which runs daily using vm instances

* Other:
	- Job failures are much better explained
	- Huge refactor for cluster handling, it is now mostly unified
	- Improved host/node handling (makes it possible later on to add support for multiple services on a single host)
	- Better CentOS7 / systemd support
	- cmon init script updates (and unified across distros [redhat/debian])
	- Support for more detailed SSH logging if needed
	- Agents are no longer supported

Changes in v1.2.8
-----------------

*Sep 17th, 2014*

* Create Single DB Node. Launch/provision a single MySQL Galera node or MongoDB ReplicaSet member node to a host.
* Create MySQL DB Users and Privileges across several DB clusters at once.
* LDAP improvements. Better support for AD. Added member+dn support. Groups and Users can be on different baseDN.
* Support for Alerts and Incident tracking with external providers using a new Alarm/Events plugin system. PagerDuty plugin/integration available.
* Unified Event Viewer. Show merged log entries (entries from multiple log sources) correlated with alarms/events occurrences.
* New alarms/email notification system. Daily alarm digests (summary). Fine-tune email delivery of different alarms/events.
* "Capacity Planner" (ALPHA). Add this constant to the UI's bootstrap.php file, define('RPC_PORT','9500'); to enable access to it.
* Three new default MySQL dashboards. InnoDB IO, Query Performance and Galera Flow Control graphs.
* Audit logging. User activity tracking. Username and originating IP is logged in the Job log.
* Add Node (MySQL/MongoDB) improvements.
* yum/apt repo server for ClusterControl!

Changes in v1.2.6
-----------------

*Apr 22nd, 2014*

* LDAP Authentication (BETA)
* User Role based access to ClusterControl functions
* OpenStack: Launch OST instances & Deploy a Galera Cluster (BETA)
* Manage multiple Galera Clusters with a single ClusterControl Controller host
* Show Master and Slaves added to a Galera Cluster
* Manage/Monitor MySQL Servers (auto detects if replication is enabled)
* Embedded Classic DB Configurations Wizard deprecated/removed!

Changes in v1.2.5
-----------------

*Feb 11th, 2014*

* Support for Galera 3.x builds (Codership & PXC 5.6)
* AWS VPC (Create/Delete and Deploy) BETA
* Custom Expressions (User defined alerts/alarms)
* Support for agent-less monitoring 
* Minor UI changes

Changes in v1.2.4c (maintenance release)
-----------------------------------------

*Dec 13th, 2013*

* Updated s9s_sw_update to reflect changes in Percona Repositories for Ubuntu.
* Bug: Invalid clear of wsrep_cluster_addresses on controller startup.

Changes in v1.2.4
-----------------------

*Nov 19th, 2013*

* Online backup storage in AWS S3 and Glacier
* Multi-cluster support. Share one Controller Node with multiple clusters
* Add existing Galera cluster via ClusterControl to monitor and manage
* Galera database configurator facelift
* Automatically deploy Galera and MongoDB cluster from ClusterControl
* Time shift stats/graphs
* MongoDB ReplicaSet AWS Deployment for Dev/Test env.
* AWS deployments now use our web site to generate a database configuration. Deploy the latest GA version of Galera/MongoDB.
* InnoDB Status output
* Schema Analyzer (redundant indexes, myisam tables, missing primary keys)
* Mongodb: Stats counters for TokuMX
* Mongodb: auth support (mongodb_user and mongodb_password)

Changes in v1.2.3
------------------

*July 15th, 2013*

* Clone Galera Cluster via the GUI (s9s_clone)
* Deploy HAProxy and Keepalived with VIP via the GUI
* User defined "dashboards" in the Overview page (quickly select your favorite graphs to show)
* New Overview page for Galera clusters
* MySQL Query Histogram added to the Performance page
* New view for DB variables and status (MySQL) added to the Performance tab. Easier to view and compare status/variables across all nodes
* Execute external/user made scripts (on the controller node)
* Customizable refresh rate (DB variables and status)
* Centralized backups
* Start/stop and rebuild MySQL replication slave for MySQL 5.6
* Reboot host from UI
* Improved sampling of statistics (better resolution)
* MongoDB
	 - Replica set support 
	 - Backups with mongodump
	 - Tokumx support
	 - Arbiter support (add/remove from cmd line)

Changes in v1.2.2
------------------

*May 16th, 2013*

* Deploy Galera cluster nodes on multi AZs and regions on AWS (great for test/dev)
* The Job log is available now in the 'Logs' view
* Simple database schema and user management (feature set from our classic cmon gui)
* Activate/deactivate monitoring of external processes (Mangage-Process)
* Add node for MariaDB
* Logfile Analyzer - automatically checks and detects problems found in mysql error logs.

Changes in v1.2.1
----------------------

*May 2nd, 2013*

* Added support for MongoDB backup
* New database growth graph
* MySQL status time machine table (show status value differences over time)
* Deploy Galera cluster on AWS (only on a single AZ). Great for test/dev.
* Moved settings (Configurations, Hosts, Processes, Software Packages, Upgrade, Schema graphs) views to new 'Manage' tab
* Fixed bugs in add node
* centralized backup, store backup data on controller by using s9s_backupc
* replication 5.6 aware (GTID)
* s9s_backup was changed, upgrade of s9s_backup on all nodes is required.
* email bug for SMTP notifications.
* recovery improvements in galera (refuse to recover cluster if a majority of the nodes cannot be reached), and recovery will be retried for a much long period of time (to avoid Galera node recovery blocked messages).
* s9s-admin tools (on controller do: git clone git://github.com/severalnines/s9s-admin.git ) for more details.
* check /usr/lib64/ for libgalera_smm.so

Changes in v1.2.0
------------------

*March 14th, 2013*

* Improved alarms
* Improvements to support ClusterControl GUI
* Bug fixes

Changes in v1.1.33
-------------------

*August 1st, 2012*

* Notes:
	- Controller: Added alarms for Replication, in case a MySQL Server crashes
	- Controller: Alarms for Galera, in case a MySQL Server crashes
	- Controller: Removed redundant messages and newlines from log messages
	- Controller: Persisting db|host_stats_collection interval to cmon db
	- Query Monitor: log_queries_not_using_indexes now settable from the Web Interface
	- Query Monitor: Set long query time via Web interface. Setting upper bound (1MB) on query size to be parsed. 
	- Query Monitor: Possibility to override CMON settings in favor for local my.cnf settings 
	- WWW + Controller: Reworked Configuration Management + web interface
	- WWW + Controller: Last mysql error now saved in mysql_server table
	- RRD: Optimized rrd graph creation, optimized galera stats collection to reduce db writes
	- WWW: Added ‘clear all jobs’ button 
	- MySQL Cluster: Display an error in the Web UI if an SQL Node is not connected to the cluster
	- Galera: Improvements in availability handling, in case createPrimary fails

* Bug fixes:
	- Replication: serverid + autoincrement sedding fixed
	- Replication: Fixed MaxConnection bug in Replication
	- MySQL Cluster: Fixed Index/DataMemory collection problem if MemoryReportFrequency is not set
	- MySQL Cluster: Fixed bug in MGM status info, preventing rolling restarts
	- MySQL Cluster: Fixed bug in stop node (SQL/Data node)
	- Galera: Make node statistics less jumpy during restarts/recovery
	- Controller: Clear MySQL replication links when a MySQL Server is removed from the cluster
	- Controller: fixed bug causing multiple email messages to be sent in case of an alarm
	- Controller: Fixed ProcessList bug if pidfile already had a path to prevent concatenation with datadir 
	- Controller: Added printout to error log if a pidfile could not be opened by the Process Manager
	- Controller: Prevent autorestart of failed agents from happening too fast
	- Backup: Fixes in length of file issue  (backup file size was 0 sometimes)

* Upgrade Instructions:
	- http://support.severalnines.com/entries/21095371-cmon-1-1-32-releas...

Changes in v1.1.32
-------------------

*June 25th, 2012*

* Notes:
	- Added load averages in ClusterControl Web interface
	- Removed unnecessary log messages
	- Added new configuration parameter to cmon.cnf: enable_autorecovery=1   (default 1 == enabled, 0 means disabled - only manual recovery).
	- Galera: It is now possible to manually recover a non-Primary Galera node from the ClusterControl web interface.
	- Galera: Improved handling of cluster recovery. Pass 1: find the best node to recover from and make it the new Primary. Pass 2: Recover the remainder of the nodes from the new Primary

* Galera: Cleaned up redundant table galera_status_history

* Bug fixes:
	- Fixed buffer overrun in query profiling and anonymizing queries (affects agents only)
	- Disable autorestart of failed agents from happening too fast
	- Galera: Handling of existing provider_options when setting pc.bootstrap
	- Buffer overrun in log message
	- Backups: Fixed issue with a stale mysql connection
	- Added error handling to process stat collection (a process could have existed when a vector of pids were assembled, but process terminated before being used)
* RRD: Fixed "ERROR: /var/lib/cmon//cluster_1_stats.rrd: expected 9 data source readings (got 1) from N"

