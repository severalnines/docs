.. _Changelog:

Change Logs
===========

This change logs list details about updates in each version of ClusterControl.

Changes in v1.7.3
-----------------

.. Warning:: We have encountered a severe bug in a library clustercontrol 1.7.3 relies on for SSH communication and may cause severe side effects. If you are using ClusterControl 1.7.3 then you must upgrade the ``clustercontrol-controller`` package to version 1.7.3-3440 (release date September 24th, 2019) or later.

Maintenance Release: October 21\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.3-3496

* Controller:
	- HAProxy: Added ``tcp-check connect`` to configuration templates.

Maintenance Release: October 20\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.3-3494
	- clustercontrol-1.7.3-6429

* Frontend (UI):
	- PostgreSQL: Fixed an issue with the charts on the *Cluster Overview* page.

* Controller:
	- PostgreSQL: *Query Monitor -> Query Statistics*, Exclusive Lock Waits was not working correctly and did not display all data.
	- Dashboard/SCUMM: Fixed an issue recoverying Prometheus exporters in case of co-located cluster nodes by multiple-clusters.
	- MongoDB: Importing a single node will now fail if the node is not set up as a replica set member. Thus, it is the user's responsibility to convert the node to a member before importing it.

Maintenance Release: October 13\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.3-3482
	- clustercontrol-1.7.3-6403

* Frontend (UI):
	- Dashboards/PostgreSQL: Fixed an issue with *Idle* and *Active Connections*.
	- Backup: Can't use backup verification server due to a bug in Host discovery.
	- Email Notifications: Improvements to email validation of 'External' users and Adding/Removing of these 'External' users.

* Controller:
	- Dashboards/PostgreSQL: *Active* and *Idle* connection dashboards aren't working for PostgreSQL. A redeploy of the ``postgres_exporter`` is needed.
	- Dashboards/PostgreSQL: Reverted back to ``postgres_exporter`` 0.4 as 0.5 was buggy.
	- Node Charts: Node CPU chart was incorrect on Centos6/RHEL6 because it had one less column (no ``guest-low`` counter value).
	- Notification: Fix daily limit handling of e-mail message recipients where "-1" was not handled correctly.
	- Error-reporting: There was a problem with file listing when multiple files was specified since we bash-escape the paths for safety.
	- HAProxy: Fixed an issue parsing the HAProxy config file.
	- HAProxy: While setting up haproxy for PostgreSQL reading old password from checker script fails.
	- PostgreSQL: Importing a node/cluster: If ``logging_collector=OFF`` and user has not specified a custom log file then the job will be aborted and the user must specify it.

Maintenance Release: September 29\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build: 
	- clustercontrol-controller-1.7.3-3450
	- clustercontrol-1.7.3-6368

* Frontend (UI):
	- MySQL: *Performance -> Transaction Log* uses timestamp and not epoch.
	- Fixes usability issues with Runtime Configuration making it easier to read
	- PostgreSQL: Fixes in *Import/Add Replication Slave* dialogs with respect to Port and Logfile fields.

* Controller:
	- MySQL: *Performance -> Transaction Log* uses timestamp and not epoch.
	- MySQL: Fixed an issue with excessive logging of long running queries.
	- HAProxy: Fix of parsing errors during ``collect_configs`` cronjob (in case of HAProxy and ProxySQL nodes).
	- error-reporter: Include complete cmon log files and not only the last rows.

Maintenance Release: September 24\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build: 
	- clustercontrol-controller-1.7.3-3440

* Controller:
	- MySQL based systems: Fixed and issue with excessive logging of long running queries.
	- SSH Communication: A number of improvements which fixes intermittent errors like 'test sudo failed' and 'SUDO failed'.

Maintenance Release: September 17\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.3-3428

* Controller:
	- MySQL Replication: A fix to update the status of the failed server in ProxySQL. The old master will now be marked as ``OFFLINE_SOFT``. Any node that is not part of the replication topology is marked as ``OFFLINE_SOFT``.
	- Added a fix that could cause a crash if a database connection could not be established.

Maintenance Release: September 10\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.3-3413

* Controller:
	- MariaDB: Setting ``innodb_thread_concurrency=0`` due to https://jira.mariadb.org/browse/MDEV-20247

Maintenance Release: September 8\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.3-6340
	- clustercontrol-controller-1.7.3-3407

* Frontend (UI):
	- Backup: Fixed an issue with scheduling a backup. If using cron settings, then due to TZs and conversions to UTC then a specified hour could be converted to an hour belonging to another day.
	- LDAP: Wrong LDAP status was shown in the UI
	- Email Notifications: Adding a recipient without having any clusters installed failed

* Controller:
	- ProxySQL: Inserting a query rule with a duplicate query rule id caused the query rule ids smaller than the duplicate to become negative.
	- Prometheus version bump to v2.12
	- PostgreSQL: On RedHat systems the default datadir was set to ``main`` instead of ``data``.
	- MongoDB: Retention fails because all mongo backups were recognised as partial, and partial can only be removed if there are more than one "full" backups.
	- A fix for an infinite amount of 'Job query is working again.' log messages in the cmon log.
	- Removing storage of log messages in a deprecated table called ``collected_logs``.

Maintenance Release: August 24\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.3-6322
	- clustercontrol-controller-1.7.3-3388

* Frontend (UI):
	- PostgreSQL: Add Slave: help text next to "logfile" text box.

* Controller:
	- Import/Add Cluster: Specified sudo password was not respected.
	- MongoDB: Importing a cluster failed even if the CAFile is specified following an error where it was not specified, because existing cert data was not updated in cmon's certificate storage.
	- Controller: Must keep trying to connect to the MySQL server even if the MySQL server is not started, instead giving up and exit.
	- PostgreSQL: Whitelist is not working as documented.
	- SCUMM/Prometheus: General small improvements with disk device detection and mapping.

Maintenance Release: August 17\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.3-3374

* Controller:
	- PostgreSQL: a crashing bug was fixed that was caused by assuming that ``cluster_name`` always have a value.
	- PostgreSQL/pgbackrest: Fixed an issue when the backup.manifest is encrypted the backup appeared as failed. Please note that the backup.manifest record is not decrypted so some meta data information may not be updated (pending feature request).
	- Controller backup/save controller: Fixed an issue saving the controller with a non-quoted password causing mysqldump to fail.
	- ProxySQL: Fixed an issue where an error message was repeated due to trying to connect from a remote node using the 'admin' user, which is forbidden in ProxySQL.
	- Error Reporting: Fixed a user handling issue, causing the error report to fail.
	- MySQL: Database Growth, adding more verbose logging in case of issue.

Maintenance Release: August 15\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.3-6298
	- clustercontrol-controller-1.7.3-3370

* Controller
	- *Performance -> Transaction Log*: Fixed an issue with pagination.

* Frontend
	- *Performance -> Transaction Log*: Fixed an issue with pagination.
	- Fixed an issue with JS code generation for older browsers by upgrading corejs.

Maintenance Release: July 29\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.3-6279
	- clustercontrol-controller-1.7.3-3336

* Controller:
	- Added support for openntpd as an alternative to the ntp dependency.
	- MySQL 8.0: Fixed an issue where the keyword 'groups' was used in a query.
	- Improved error reporting in case of SSH errors when trying to determine the MySQL connect string.
	- PostgreSQL: Create a symlink to custom log file during add existing cluster as well, not only during add exisitng node.
	- PostgreSQL: When adding an existing cluster, a custom specified log file will be be used  if ``logging_collector`` is off.
	- PostgreSQL: Fixed an issue detecting log files.
	- MySQL: A password could be visible in the ``ps`` output of a node when the cmon database was updated at controller startup.
	- Create/register cluster: Handle 'company_id' if provided, otherwise we try to query it up by ``user_id`` as a fallback.

* Frontend (UI):
	- Fixed an issue where a cluster could not be registered due to a missing company id/team id.

Maintenance Release: July 24\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.3-6270

* Frontend (UI):
	- Fix an issue saving and pushing out edited configuration files (Configuration Management).
	- Fix an issue with the *Overview* page not being properly shown after switching between tabs (PostgreSQL).

Maintenance Release: July 18\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.3-6255
	- clustercontrol-controller-1.7.3-3319

* Controller:
	- PostgreSQL: Fixes in log file handling to check if the log collector is enabled already. This could result in e.g the wrong log file was used.
	- PostgreSQL: A fix in multi-node support when adding nodes that could lead to nodes not being part of the replication topology.
	- PostgreSQL: Fixed an issue when the logfile was not owned by the postgres user.
	- PostgreSQL: Updated the repository signature.
	- TimeScaleDB: Fixed an issue adding a replication slave due to a version mismatch.
	- TimeScaleDB: Fixed an issue when rebooting TimeScaleDB and PostgreSQL master results in two master nodes.
	- MariaDB/Replication: Fixed an issue with *Promote Slave* (switch-over).
	- MariaDB/Galera: Fixed a check for the ``wsrep_sst_method`` to check whether xtrabackup vs. mariabackup is used.
	- MySQL/MariaDB: Importing a cluster could fail as it assumed ``bind_address`` existed as a server system variable.

* Frontend (UI):
	- Add a workaround to sort the cluster list by name, status, type with a new ``bootstrap.php`` variable (instead of using ``cluster_id`` by the default):
		- ``define('CLUSTER_LIST_SORT_BY', 'name');   # sort by cluster name``
	- Add additional information on how to use the 'Stanza Name' with PgBackRest backups
	- Add missing confirmation dialog for MongoDB restore backup

Maintenance Release: July 16\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.3-6242

* Frontend (UI):
	- Fix a HTML formatting issue when trying to change non-dynamic parameters in Configuration Management (MySQL).
	- Fix an issue with the *Nodes->DB Performance* chart which requested unfiltered datasets.  

Maintenance Release: July 12\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.3-6226

* Frontend (UI):
	- Fix missing mysqldump backups (PITR) for 'Add Replication Slave' when rebuilding with a backup.
	- Fix incompatible array notation with PHP v5.3.

Initial Release: July 2\ :sup:`nd`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++

* Build: 
	- clustercontrol-1.7.3-6209
	- clustercontrol-controller-1.7.3-3293
	- clustercontrol-cloud-1.7.3-217
	- clustercontrol-ssh-1.7.3-79
	- clustercontrol-notifications-1.7.3-182

In this release we have added support for running multiple PostgreSQL instances on the same server with improvements to PgBackRest to support those environments. 
We have also added additional cluster types to our cloud deployment and support for scaling out cloud deployed clusters with automated instance creation. Deploy MySQL Replication, PostgreSQL, and TimeScaleDB clusters on AWS, GCE, and Azure. 

**Feature Details**

* PostgreSQL
	- Manage multiple PostgreSQL instances on the same host.
	- Improvements to pgBackRest with non-standard instance ports and custom stanzas.
	- New Configuration Management page to manage your database configuration files.
	- Added metrics to monitor Logical Replication clusters.
	
* Cloud Integration
	- Automatically launch a cloud instance and scale out your database cluster by adding a new DB node (Galera) or replication slave (Replication).
	- Deploy following new replication database clusters:
		- Oracle MySQL Server 8.0
		- Percona Server 8.0
		- MariaDB Server 10.3
		- PostgreSQL 11.0 (Streaming Replication).
		- TimescaleDB 11.0 (Streaming Replication).

* Misc
	- Backup verification jobs with xtrabackup can use the ``--use-memory`` parameter to limit the memory usage.
	- A running backup verification server will show up in the Topology view as well.
	- MongoDB sharded clusters can add/register an existing MongoDB configuration node.
	- The clustercontrol-cmonapi (CMON API) package is deprecated from now on and no longer required.
	- A few more legacy ExtJS pages have been migrated to AngularJS:
		- Configuration Management for MySQL, MongoDB, and MySQL NDB Cluster.
		- Email Notifications Settings.
		- Performance -> Transaction Logs.

Changes in v1.7.2
-----------------

Maintenance Release: June 12\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.2-3142

* Controller:
	- Fixed a CmonDB schema issue on older MySQL server versions manifesting itself as ``Specified key was too long; max key length is 767 bytes``.
	- MaxScale: A fix for imported MaxScale. When importing MaxScale, the utility ``maxctrl`` is used and works currently only with socket communication on the MaxScale host itself.
	- Jobs: Log files contain job spec with sensitive data.
	- MariaDB: Fixed and issue with deployment of MariaDB 10.0 on Centos 6 failed.
	- Postgres: Fixed a bug that could crash cmon in case wal log retention was disabled and fixed a printout in PITR job output.

Maintenance Release: May 24\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.2-6137

* Frontend (UI):
	- Memory leak fixes when leaving the web application open for extended periods of time (days).
	- Fixes to the database software upgrades form to show correct versions supported. 
	- Note: Only upgrades within minor versions are supported.

Maintenance Release: May 24\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.2-6069
	- clustercontrol-controller-1.7.2-3199

* Frontend (UI):
	- Deployments: Custom configuration templates can now be selected at deployment.
	- Cluster Overview:
		- 'Server Load' graphs were not properly displayed (PostgreSQL).
		- Changing the 'Server Load' graph would not accurately show only one metric (PostgreSQL).
		- Disk Reads/Writes and Uptime were set to 0 (PostgreSQL).
		- Disk bytes read/written were not calculated with correct sector value of 512 bytes.
		- Switching between dashboards with a specific set of steps could cause the overview page to render an empty page.

* Controller:
	- Deadlock detection temporarily disabled for MySQL/Percona 8.0. It will be supported in the next major release.
	- mysqldump failed with MySQL/Percona 8.0 because of missing ``show_compatibility_56=ON`` setting. It is now on for versions >= 5.7.6.
	- Agent Based Monitoring (Prometheus):
		- Uptime were set to 0.
		- Disk stats for the controller is now also available.
		- ``node_disk_written_bytes_total`` | ``node_disk_read_bytes_total`` are now also collected.
	- Reverting to nc instead of socat on Ubuntu 16.04 due to a bug with socat's server name resolve when it starts with a number.
	- Manual failover with MariaDB 10.1 for MySQL Replication cluster is now correctly flushing logs before switchover.
	- Restore backup on Mongos (routers) failed to copy the data dir.

Maintenance Release: May 16\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.2-3185
	- clustercontrol-1.7.2-6032

* Frontend (UI):
	- Nodes Page: Fixed an issue with y-axis scaling on the Disk Utilization chart.
	- Nodes Page: Selecting the menu 'Add Replication Slave' and start adding slave was impossible when a Node recovery job was running
	- MongoDB: Fixed an issue where the Restore backup dialog would not close after pressing "Finish".

* Controller:
	- Monitoring/SCUMM: PostgreSQL exporter and MySQL exporter URL password encoding fix which could cause a "No data points" in *Dashboards -> Postgres Overview*.
	- Monitoring/SCUMM: A fix for disk stats to be properly shown when using LVM volumes in the *Nodes -> Disk* charts.

Maintenance Release: May 7\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.2-3167

* Controller:
	- MySQL 8.0: Updated imperative language files to support the previous release build issue: "Fixed an issue preventing db users from being created on MySQL 8.0".

Maintenance Release: May 6\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.2-5997
	- clustercontrol-controller-1.7.2-3163

* Frontend (UI):
	- Filtering out incomplete/failed backups from restore backup dialogs.
	- MySQL Single (standalone servers): Fixed filtration logic to show the Master Nodes for MySQL Single clusters.

* Controller:
	- MySQL 8.0: Fixed an issue preventing db users from being created on MySQL 8.0.
	- Config file handling fix for docker (we mount ``/etc/cmon.d`` there and ``/etc/cmon.d/cmon.cnf`` is the main config)

Maintenance Release: April 30\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.2-5989
	- clustercontrol-controller-1.7.2-3155

* Frontend (UI):
	- Query Monitor > Query Outliers: Fixed an issue related to date range.
	- Performance > Innodb  Status: Fixed an issue when the InnoDB Status was not always shown.

* Controller:
	- ProxySQL: Fixed an issue with importing users on MariaDB 10.2 and later.
	- Galera: Fixed an issue when the recovery job was closed prematurely. This had the effect that *Create Cluster* could fail.
	- SCUMM: Preserve the exporters of other clusters in Prometheus configuration during (re)deployment. (Note: Users with multiple clusters and wrong Prometheus configuration may need to re-deploy the promethus on the affected [No data point] clusters).
	- Query Monitor: Fixed an issue where queries were dropped following a schema update when upgrading clustercontrol-controller.


Maintenance Release: April 19\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.2-5959
	- clustercontrol-controller-1.7.2-3141

* Frontend (UI):
	- Query Monitor: Selecting/clicking on a query didn't show the query details. 
	- Query Monitor: Top queries page were empty for a single node galera cluster.
	- MongoDB:
		- Restore backup menu item was missing.
		- Restore backup dialog form was empty for single node replica sets.
	- Spotlight: Performance improvements when you have several clusters/nodes.
	- Cloud deployments now use the same package versions as the on-premise deployments.

* Controller:
	- MySQL Replication: Fixed an issue with slave promotion causing an errant transaction to appear.
	- Security: Fixed permissions on all cmon generated config files to be 0600.
	- Galera (MariaDb):  Increased start timeout for a longer SST in the mariadb.service override systemd file.


Initial Release: April 4\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.2-5926
	- clustercontrol-controller-1.7.2-3117
	- clustercontrol-cmonapi-1.7.2-342
	- clustercontrol-notifications-1.7.2-176
	- clustercontrol-ssh-1.7.2-73
	- clustercontrol-cloud-1.7.2-196

We are proud to announce an expansion of the databases we support to include `TimescaleDB <https://github.com/timescale/timescaledb>`_, a revolutionary new time-series that leverages the stability, maturity and power of PostgreSQL. TimescaleDB can ingest large amounts of data and then measure how it changes over time. This ability is crucial to analyzing any data-intensive, time-series data. For ClusterControl, this marks the first time for supporting time-series data; strengthening our mission to provide complete life cycle support for the best open source databases and expanding our ability to support applications like IoT, Fintech and smart technology. 

In this release you can now deploy a TimescaleDB and also turn an existing PostgreSQL server to a TimescaleDB server. PostgreSQL clusters also support a new backup method `pgBackRest <https://pgbackrest.org>`_, database growth charts and improvements to manage your configuration files. 

MySQL users can start to deploy and import **MySQL 8.0** servers with Percona and Oracle MySQL and our new **Spotlight** search helps you navigate through pages, find nodes and perform actions faster. 

Finally, we are also providing a beta version to setup CMON / Controller High Availability using several ClusterControl instances wired with a consensus protocol (raft) between them.

**Feature Details**

* TimescaleDB - optimized for time-series data using SQL -- **more documentation coming soon!**
	- Deploy a TimescaleDB server with PostgreSQL (v9.6, v10.x and v11.x).
	- Turn an existing PostgresQL server (v9.6, v10.x and v11.x) into a TimescaleDB server.

* PostgreSQL
	- Database growth graphs. Track the dataset growth on your databases.
	- Support for pgBackRest as a backup tool:
		- Create full, differential and incremental backups.
		- Restore full, differential, incremental backups.
		- PITR - Point In Time Recovery is supported.
		- Enable compression and specify compression level.

* MySQL 8.0 Support
	- Cluster deployment and import of 'replication' type clusters available with:
		- Percona Server for MySQL 8.0
		- Oracle MySQL 8.0 Server
	- Support for ``caching_sha2_password``.

* CC Spotlight
	- Use our new spotlight search to quickly open pages, find nodes/hosts and perform cluster and node actions.
	- Click on the search icon or use the keyboard shortcut CTRL+SPACE to bring up the spotlight.

* CMON / Controller High Availability (BETA)
	- CMON HA is using a consensus protocol (raft) to provide a high availability setup with more than one cmon process.
	- Setup a 'leader' CMON process and a set of 'followers' which share storage and state using a MySQL Galera cluster.
* Misc
	- Support the use of private IPs when you deploy a cluster to AWS.
	- MaxScale - improved support for v 2.2 and later using maxctrl.
	- Automatic vendor/version detection for importing MariaDb/MySQL based clusters.

Changes in v1.7.1
-----------------

Maintenance Release: March 25\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.1-3085

* Controller:
	- Resolve hostnames (to IPv4) when checking a host if it exists already in other clusters.
	- MongoDB: adding missing sharding:clusterRole:shardsrv value in mongod.conf when add node job is used.
	- MaxScale: connection not authorized after the deploy with CC. More fixes to improve 2.3 and later support.
	- Backup: Do not fail backup if wsrep desync can't be turned off, and we must set the retention on backup report even if it was marked as failed.
	- Monitoring/SCUMM: ``haproxy_exporter``: Don't append ``--haproxy.scrape-uri`` if it is already set.
	- Replication: Can't add replication slave to an existing slave. Let's be stricter and do not tolerate >1 writable when setting up.
	- s9s_error_reporter: make sure cmon is started, also print out the service status.
	- PostgreSQL: Fixing an issue when a system file protection method denied the proxy-disable file removal
	- Package handling/YUM: Fix for a situation when package update gets stuck on user input (to accept some GPG signature).
	- SSH: A fix/workaround to handle the 'forced user password change' situation if user password expires (``passwd --expire USERNAME``) and is prompted to change upon a successful authentication.
	- SSH: Limit the number of sent newline chars.
	- Updated Oracle repository key due to expiration.
	

Maintenance Release: March 18\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.1-5812

* Frontend:
	- Allow empty SMTP username and SMTP password for the SMTP configuration.
	- Fix an issue for failing to stop MySQL slave threads (IO and SQL).

Maintenance Release: March 5\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.1-3056

* Controller:
	- Advisors: Fixed an issue with the ``wsrep_cluster_address.js`` where an ``internalHostName`` method was missing.
	- MongoDb: Use the mongodb OS user depending on the OS and package when setting up ssl.
	- PostgreSQL:  Fixed a PostgreSQL grant failure because of client locale setting.
	- PostgreSQL: Workaround a PostgreSQL service initdb bug. Now we call directly the ``initdb`` binary. The relevant original bug report: https://www.postgresql.org/message-id/20171208104120.21687.74167@wrigleys.postgresql.org


Maintenance Release: February 27\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-notifications-1.7.1-173

* Notifications:
	- Fix for cmon-events to prevent Avast to report it as a malware (Telegram API).
	- Fix for cmon-events to start even if the MySQL server has not started first.


Maintenance Release: February 20\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.1-5720

* Frontend (UI):
	- Keepalived: Fixed an issue importing Keepalived.
	- HAProxy: Dashboard fixes (SCUMM).
	- Nodes Page: Removed the tab 'Logs' as it is deprecated and found in *Logs > System Logs* instead.


Maintenance Release: February 18\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++


* Build: 
	- clustercontrol-controller-1.7.1-3032

* Controller:
	- Maria Backup: Fixed an issue parsing LSN in mariabackup >= 10.2.22.
	- Prometheus: Fixed an issue when restarting a failed exporter.

Maintenance Release: February 13\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.1-3027 
	- clustercontrol-1.7.1-5700

* Frontend (UI):
	- ProxySQL: Fixed an issue in the pagination structure in ProxySQL sync making it impossible to Import/Export/Sync ProxySQL Configurations
	- Fixed an issue regarding REPLICATION LAG where the lag was presented as a derived value instead of an absolute when viewing the individual servers.
	- Fixed an issue with rebuild replication slave from incremental backup dialog.

* Controller:
	- Fixed an issue regarding stats aggregation. This could manifests itself as spikes in particularly the REPLICATION_LAG.
	- Keepalived:  Small update for registering keepalived; the service port must be corrected to 112.
	- Process Management: A fix for a file descriptor leak when an internal object was reused.
	- MongoDb 4.0: A fix for creating mongodb replica sets by checking executed mongodb commands for more error messages.
	- Galera: A fix to the ``wsrep_cluster_address.js`` advisor to also check the internal/private hostname/IP-addresses.
	- MySQL: skip missing grant alarms on backup-verification nodes.

 
Maintenance Release: February 6\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.1-3016
	- clustercontrol-1.7.1-5673

* Frontend (UI):
	- Deploy HAProxy on PostgreSQL: Fixed an issue where the dialog was stripped and did not load completely.
	- Performance -> DB Variables: Variables with different values are not marked in red
	- Dashboards: System Overview, improved the readability of the CPU Usage chart.
	- PostgreSQL Query Monitor: Removed tuning advise and the option to purge queries as it is not possible at all.

* Controller:
	- Configuration Changes: Fixed an issue where the owner and privileges of a config file was not preserved.
	- Deploy/Create Cluster From Backup: A fix to prevent the restore backup from running in another job.
	- ProxySQL: Replaced old galera_checker script for proxysql to a new 2.0 version one
	- ProxySQL: Improved s9s CLI and cmon such that making a proxysql configuration backup can be performed using the s9s CLI.
	- Advisors: A new script to check prepared statement exec limits. The advisor script must be manually scheduled by the administrator.
	- Alarm Notifications: The Memory Utilisation alarm was not showing all processes in the included 'top' view.


Maintenance Release: January 22\ :sup:`nd`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.1-2294
	- clustercontrol-notifications-1.7.1-168

* Backend:
	- MySQL/Galera: Fixed a bug in related to the loading of Disk/CPU/Net stats on the *Cluster Overview* page.
	- HAProxy/ProxySQL/Garbd: Disable firewall/selinux (if requested by the job, default is true for both values).
	- Replication:  Added a small hint about ``--report-host`` argument being required for add existing slaves.
	- MongoDB: Fixed an issue when an rolling restart was attempted, but a stop/start of the cluster is required when setting up SSL. 
	- MongoDB: Added a ``server_selection_try_once``, ``server_selection_timeout_ms`` to allow the user to fine tune connection settings when e.g the network is slow. Run ``cmon --help-config`` to see the complete description.

* ClusterControl notifications:
	- Fixes to logging.
	- The license check failed due to the wrong field name, preventing e.g notification plugins from receiving alarm events.

Maintenance Release: January 13\ :sup:`th`\ , 2019
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.1-2985

* Backend:
	- Bugfix for SSH connection negotiation failure on compression methods.
	- HAProxy: A configuration error could occur when adding a new node, a 'none' word was wrongly added to the HAProxy configuration.
	- HAProxy: Deploying HaProxy fails when it builds from source. Missing zlib1g-dev and zlib dependency.
	- HAProxy: xinetd port was missing a default value. It now defaults to port 9200.
	- Point in-time Recovery (MySQL): Binary logs could be applied in the wrong order.
	- MySQL Replication: Switchover hooks do not work (``replication_pre_switchover_script`` and ``replication_post_switchover_script`` are now executed upon *Promote Slave*).
	- ProxySQL: Importing a user from MySQL fails to duplicate the grants.
	- Prometheus: A fix to collect the log file from the Prometheus host, instead of the exporter host.
	- Create cluster job fails on permissions of ssh user when the username contained ``\``.
	- NDB Cluster: Updated to use MySQL Cluster 7.5.12 binaries.
	- Operational Reports: A fix to avoid repetition of node information in the 'System Report'.
	- Cloud: A fix to improve the auto registration of the cmon-cloud binary and improved logging. This also requires a new version of cmon-cloud (new build coming soon).

Maintenance Release: December 29\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.1-5622
	- clustercontrol-notifications-1.7.1-159

* Frontend (UI):
	- MySQL Galera: Fix 'Add Node' regression where the template file was not set in the job specification.
	- Prevent cmon-events to crash if cmon is not running.

Initial Release: December 21\ :sup:`st`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.1-2854
	- clustercontrol-1.7.1-5617
	- clustercontrol-cloud-1.7.1-163
	- clustercontrol-notifications-1.7.1-157
	- clustercontrol-ssh-1.7.1-70
	- clustercontrol-cmonapi-1.7.1-338

In this release we have primarily continued to add improvements to our agent based monitoring dashboards and PostgreSQL. 

**Feature Details**

* Agent Based Monitoring:
	- Install/enable Prometheus exporters on your nodes and hosts with MySQL, PostgreSQL and MongoDB based clusters.
	- Customize collector flags for the exporters (Prometheus). This allows you for example to disable collecting from MySQL's performance schema if you experience load issues on your server.
	- Supported Exporters:
		- Node/host metrics
		- Process - /proc metrics
		- MySQL server metrics
		- PostgreSQL metrics
		- ProxySQL metrics
		- HAProxy metrics
		- MongoDB metrics
	- Dashboards:
		- System Overview
		- Cluster Overview
		- MySQL Server - General
		- MySQL Server - Caches
		- MySQL InnoDB Metrics
		- Galera Cluster Overview
		- Galera Server Overview
		- PostgreSQL Overview
		- ProxySQL Overview
		- HAProxy Overview
		- MongoDB Cluster Overview
		- MongoDB ReplicaSet
		- MongoDB Server

* Backup:
	- Create a cluster from an existing backup with MySQL Galera or PostgreSQL.

* PostgreSQL:
	- Query Monitoring improvements - View query statistics:
		- Access by sequential or index scans
		- Table I/O statistics
		- Index I/O statistics
		- Database Wide Statistics
		- Table Bloat And Index Bloat
		- Top 10 largest tables
		- Database Sizes
		- Last analyzed or vacuumed
		- Unused indexes
		- Duplicate indexes
		- Exclusive lock waits
	- Verify/restore backup on a standalone host.
	- Create a cluster from an existing backup.
	- Support for PostgreSQL 11. Deploy and import clusters.

* MongoDB:
	- Support to deploy/import and manage MongoDB Inc v4.0

* Misc:
	- New license format. Please contact sales@severalnines.com for a new license.
	- Continuing moving ExtJS pages to AngularJS. This time the load balancer and nodes page.
	- UI logging for troubleshooting web application issues.
	- ClusterControl Backup/Restore - This feature can be used to migrate a setup from one controller to another controller. Backup the meta data of an entire controller or individual clusters from the s9s CLI. The backup can then be restored on a new controller with a new hostname/IP and the restore process will automatically recreate database access privileges. 

Changes in v1.7.0
-----------------

Maintenance Release: December 21\ :sup:`st`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.0-2962

* Controller:
	- Bugfix for SSH connection negotiation failure on compression methods.
	- Added support for MaxScale 2.3
	- Exporters: New ``process_exporter`` version (0.10.10)
	- Error Reporting: ``s9s_error_reporter -i0`` collects all config files under ``/etc/cmon.d/``

Maintenance Release: December 12\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.0-5548
	- clustercontrol-controller-1.7.0-2939

* Frontend (UI):
	- Keepalived: Fixed an issue where it was listed as a 'master' in the Cluster Node bar.
	- Fixed an issue when the replication slaves of a Galera cluster was not shown under 'Show Server'
	- Config Mgmt: Removed the Configuration -> Template item as it is deprecated in its current form.

* Controller:
	- Error Report: Fixed an issue where passwords was not masked.
	- Deploy Mongodb: Fixed signing keys issues for APT/YUM repos.

Maintenance Release: December 10\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.0-2930

* Controller:
	- HAProxy: A fix to remove ``/dev/shm/proxyoff`` file when promoting a slave or rebuilding a slave.

Maintenance Release: December 7\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.0-2928 

* Controller:
	- PostgreSQL: Double-check if slave has properly configured the ``trigger_file`` option in ``recovery.conf``.
	- Fixed and issue with wrong owner of the stagingDir (``~/s9s_tmp``)
	- Updated a mongodb.org repo key (replaced the key Richard Kreuter <richard@10gen.com>, with MongoDB 3.4 Release Signing Key <packaging@mongodb.com>
	- ProxySQL: properly handling # when handling the monitor and admin users passwords.

Maintenance Release: November 27\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.0-5455
	- clustercontrol-controller-1.7.0-2904

* Frontend (UI):
	- PHP Sessions fix for PHP v5.3 and earlier: Added the possibility to fallback to previous filebased session handling. If you experience UI issue please set ``define('SESSIONS_FALLBACK', true);`` in ``/var/www/html/clustercontrol/bootstrap.php`` and reload the page.
	- Backup: Fixed an issue with cron schedule validation in Scheduled Backups.
	- Dashboards: Minor optimizations and re-organization of some dashboards.

* Controller:
	- Galera: Clone cluster did not handle default datadir and ``wsrep_cluster_name`` for cloning.
	- Backup: Backup dir starting with ``/sys`` can't be removed, fixed a security check.
	- Error Reporting: skip GRA* files from error report.
	- Operational Reports: system report: Customizable graphs interval (in days unit).
	- Operational Reports:  changed title from 'Daily System Report' to 'System Report'.
	- Fixed a bug escaping passwords.

Maintenance Release: November 13\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.0-5375
	- clustercontrol-controller-1.7.0-2876  

* Frontend(UI):
	- Fixed an issue with PHP session management on PHP 5.3 and earlier. This manifested itself as e.g the Node page was loading forever, no data in the UI and "Internal Error".

* Controller:
	- Backup [mariabackup/xtrabackup]: Clean up qpress archives after restoring an xtrabackup|mariabackup compressed backup
	- Verify Backup [mariabackup/xtrabackup]: Fixed a regression where the wrong restore method was selected.


Maintenance Release: October 30\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++


* Build:
	- clustercontrol-controller-1.7.0-2859
	- clustercontrol-1.7.0-5319
	- clustercontrol-cloud-1.7.0-154
	- clustercontrol-notifications-1.7.0-153
	- clustercontrol-ssh-1.7.0-66

* Frontend (UI):
	- Keepalived: Added a fix to show the role, i.e which keepalived node that has the VIP assigned.
	- Deploy: Added ``.`` (dot), (space) and ``/`` (backslash) as allowed symbols for the password field. 
	- ProxySQL:  corrected use of proxysql match digest/pattern fields.
	- General: Improved session handling.
	- SSE (Server Side Events): Improvements to show notifications.
	- OS service files fixes to handle non English locales for cmon-cloud, cmon-events, and cmon-ssh.

* Controller:
	- Deploy/Import Cluster: Fixed an issue to allow ``\`` (backslash) in the admin user password (mysql root password).
	- Backup: Restore backup on a Galera cluster (mariabackup/xtrabackup) to a single node shuts down whole cluster even if bootstrap cluster was disabled.
	- Backup: mariabackup qpress support.
	- Backup: Increased the size of the backup record (TEXT -> MEDIUMTEXT).
	- Backup: Fail early if an attempt is made to take an xtrabackup on a MariaDB 10.3 server, and warn if xtrabackup is attempted on the MariaDB 10.2 series. Using mariabackup on 10.2 and 10.3 is recommended.
	- Backup: Verification now supports ``--use-memory`` option.
	- Deploy MariaDB 10.3: Fix buggy ``galera_new_cluster`` (https://jira.mariadb.org/browse/MDEV-17379).
	- Galera: Fixed an issue with rebuilding node from the backup.
	- Galera/Replication: Fixed an issue preventing a node from being rebuilt if only mariabackup was available on the node. Also improved error messages.
	- Keepalived: Added information which node has the VIP assigned.


Maintenance Release: October 19\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.0-5281

* Frontend (UI):
	- Add Node with 'Rebuild from Backup': Fix wrong backup id parameter in the job spec.
	- Add Node: Moved rebuild backup dropdown.
	- Mail server configuration: Fix invalid port length.
	- Rebuild from backup: Fix to only show successful backups in the dropdown.
	- Removed xtrabackup option from MariaDB v10.3 clusters since it's no longer working with v10.3.

Maintenance Release: October 16\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.7.0-2832

* Controller:
	- MariaDB: Fixed an issue with rebuild replication slave to support MariaDb Backup.
	- Configuration Management: Fixed an issue preventing to assign decimal values to a database variable.

Maintenance Release: October 10\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.0-5259
	- clustercontrol-controller-1.7.0-2825

* Frontend:
	- SSE (Server Side Events): Fixed when a toaster was shown prompting configuration suggestions when a security token is invalid.
	- Advisors: Fixed an issue with importing of advisors and the overwrite flag was not respected.
	- Cloud: Fixed and issue with subnets and AZs
	- Backup: Added 'MySQL Db Only' as a dump type for mysqldump. This creates a dump of only the mysql database.

* Controller:
	- General: Fixed an issue to chown a dir only if ClusterControl created it.
	- Advisors: A fix to properly handle multiple partitions in ``s9s/host/disk_space_usage.js``.
	- MongoDb: Fixed an issue where a stepDown was attempted on a shard router (mongos), and the restart node job failed.
	- Prometheus: Fail install if a running Prometheus server is detected.
	- Prometheus: Updates to queries and optimisations.
	- Postgres: Fixed an issue when deploying 9.2.
	- Galera: Fixed a bug where the desync node did not work when using MariaDb Backup.
	- MySQL Replication: Fixed a bug when the node got the wrong node status after a restart.

Maintenance Release: September 26\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build: 
	- clustercontrol-1.7.0-5224
	- clustercontrol-controller-1.7.0-2798

* UI:
	- Nodes Page: Fixed a regression with the node charts where the last four graphs had "no data points".
	- User Management: Fixed a navigational issue making the Clusters list show up as empty.
	- Events (Server Side): Fixed an configuration issue regarding CMON events notifications, which could lead to a 'Enable Events' dialog showing up too frequently.

* Controller:
	- Operational Reports: Fixed an issue where the cluster type in the operational reports was missing
	- Operational Reports: Fixed an issue where the creation of operational reports could deadlock.
	- Deploy (MySQL based setups): Fixed a deployment issue where a sanity check failed to determine if percona-xtrabackup was successfully installed.
	- MongoDb: Fixed an issue with configuration file handling when mongos and mongod's are colocated.
	- Prometheus: A couple of minor optimisations to queries (improved filtering of disk device/fs)
	- ProxySQL: Fixed an installation issue on LXD containers.


Initial Release: September 24\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.7.0-5208
	- clustercontrol-controller-1.7.0-2792
	- clustercontrol-cmonapi-1.7.0-333
	- clustercontrol-cloud-1.7.0-147
	- clustercontrol-ssh-1.7.0-62
	- clustercontrol-notifications-1.7.0-139

In this release we are introducing support for agent based monitoring with Prometheus (open-source systems monitoring and alerting system). Enable your cluster to use Prometheus exporters to collect metrics on your nodes and hosts. Avoid excessive SSH activity for monitoring and metrics collections and use SSH connectivity only for management operations. 

You can use a set of new dashboards that uses Prometheus as the data source and gives access to its flexible query language and multi-dimensional data model with time series data identified by metric name and key/value pairs. In future releases we will be adding more features such as allowing you to create and import your own dashboards. 

We have also a new security feature to enable Audit Logging for MySQL based clusters. Enable policy-based monitoring and logging of connection and query activity executed on your MySQL servers. 

Finally we have added support to easily scale out your cloud deployed clusters by automating the cloud instance creation for the new DB node.  

**Feature Details**

* Agent Based Monitoring:
	- Install a Prometheus v2.3.x server on a specified host.
	- Install/enable Prometheus exporters on your nodes and hosts with MySQL and PostgreSQL based clusters.
	- Supported Exporters:
		- Node/machine metrics
		- Process - /proc metrics
		- MySQL server metrics
		- PostgreSQL metrics
		- ProxySQL metrics
* New dashboards:
	- Cross Server Graphs
	- System Overview
	- MySQL Overview
	- MySQL Replication
	- MySQL Performance Schema
	- MySQL InnoDB Metrics
	- Galera Cluster Overview
	- Galera Graphs
	- PostgreSQL Overview
	- ProxySQL Overview
* Security:
	- Enable/disable Audit Logging on your MySQL based clusters. Enable policy-based monitoring and logging of connection and query activity.
* Cloud:
	- Cloud Scaling. Automatically launch cloud instances and add nodes to your cloud deployed clusters.
* Misc:
	- Support for MariaDB v10.3.
	- New 'demote master to slave' action for MySQL replication clusters.
	- Customize the timezone for dates and time shown across the application.
	- UI toasters/notifications for CMON events and alarms. Enables 'Server Sent' events to be sent to the web application for a more dynamic updated user interface.
	- Improved workflow to enable PITR for PostgreSQL.
	- Added performance graphs for ProxySQL hosts.


Changes in v1.6.2
-----------------

Maintenance Release: September 14\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.6.2-5148
	- clustercontrol-controller-1.6.2-2769

* Controller:
	- Backup (MariaDB Backup): Use mbstream instead of xbstream. This removes the dependency to the Percona Xtrabackup package. 
	- Advisors (MySQL): Improved TimeZone advisor to check if the timezones on the MySQL servers are aligned. This fixes an issue with e.g CET and CEST timezone which are from MySQL's perspective treated the same.
	- Backup (Verify Backup):  Fixed an issue regarding connectivity. Now the *Verify Backup* does not rely on the MySQL system database tables from cluster db node to perform the verification. This removes the need for a port (9999 by default) to be opened between the cluster node(s) and the backup verification server.
	- Job handling: Improved parallelism.

* UI:
	- MaxScale: Fixed an issue in password validation
	- ACLs: Fixed a number of issues in ACL handling.

Maintenance Release: August 27\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.6.2-2726

* Controller:
	- ProxySQL: Fixed an issue with Sync Instance preventing query rules to become active on target instance.
	- Backup (MariaDB Backup): Fixed an issue where the incorrect encryption options were passed to Maria Backup. 
	- Backup (Percona XtraBackup/MariaDB Backup): Fixed the order so that backups are first compressed and then encrypted resulting in smaller backup sizes. 
	- Galera: Fixed a bug in 'Clone Cluster' which ignored the 'sudo' password (if set) leading to failed cloning.

Maintenance Release: August 21\ :sup:`st`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.6.2-5025
	- clustercontrol-controller-1.6.2-2718

* UI:
	- Fix broken "Resync Node" from backup (MySQL Galera).
	- Misc ACL privileges fixes to Deployments, Activity Viewer, Left Side Navigation, and default user.
	- Correctly handle empty responses on the User Management page.

* Controller:
	- Backup: Fixed an issue with parallel backups when executed on the controller.
	- Backup: A fix to recreate the backup user with the proper privileges.
	- ProxySQL: Fixed an issue with broken stats (e.,g the 'Questions' was not properly accounted for). 
	- ProxySQL: Fixed an issue with version detection (added fallbacks).
	- PostgreSQL: Added support for creating user entries with masks other than /32 via s9s cli.
	- PostgreSQL: Fixed an issue with connection errors from HAProxy to PostgreSQL with IPv6.
	- Replication: Failover scripts did not get executed.
	- MongoDb: Updated the repo key.


Maintenance Release: July 23\ :sup:`rd`\ , 2018
++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.6.2-4959

* UI:
	- Fix copy and paste in the Query Monitor (PostgreSQL)
	- Show trimmed query in full for Query Monitor (PostgreSQL)
	- Fix dialog labels for AppArmor/SELinux
	- Partial backup warning only for xtrabackup/mariadbbackup
	- Security page is currently only for MySQL/PostgreSQL and fixes for use existing certificates


Initial Release: July 16\ :sup:`th`\ , 2018
++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.6.2-4942
	- clustercontrol-controller-1.6.2-2662
	- clustercontrol-cmonapi-1.6.2-330
	- clustercontrol-cloud-1.6.2-141
	- clustercontrol-ssh-1.6.2-59
	- clustercontrol-notifications-1.6.2-136

Welcome to our new 1.6.2 release!

**Feature Details**

* Backup:
	- Continuous Archiving and Point-in-Time Recovery (PITR) for PostgreSQL.
	- Rebuild a node from a backup with MySQL Galera clusters to avoid SST.
	- Option to restore external backups stored on a DB node (instead of only the Controller host).

* MySQL/Galera:
	- Rebuild a Galera node from a backup to avoid SST.

* Security:
	- Consolidate security functionality on an easily accessible single page.
	- Enable/Disable:
		- Client/Server SSL encryption for MySQL based clusters.
		- SSL replication traffic encryption for MySQL Galera based clusters.

* ProxySQL:
	- Clear/Reset Top Queries.
	- Advanced query rules options: Error and OK messages, sticky connection and multiplex.
	- Autofill match digest for a query rule.

* Cloud:
	- Destructive actions now cleanup used cloud resources (accounting).

* Misc:
	- ClusterControl (CMON) Runtime Configuration page.
	- Support for MongoDB v3.6.

Changes in v1.6.1
-----------------

Maintenance Release: July 4\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.6.1-4896

* UI:
	- Identical host charts fix for SQL and Data Nodes with MySQL Cluster (NDB).
	- 'DB User Management' fix with MySQL Cluster (NDB). Create and edit DB users works again.

Maintenance Release: June 28\ :sup:`th`\ , 2018
++++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.6.1-2621

* Controller:
	- MariaDB: Deployment fix caused by a mix up of authentication_string and password in the ``mysql.user`` table.
	- Restore slaves/Rebuild nodes (MySQL, PostgreSQL) - Making the directory of the datadir backup configurable. Specify ``datadir_backup_path`` in ``/etc/cmon.d/cmon_X.cnf``. By default the datadir will be copied (after the server has been shutdown, but before restoring/rebuilding) using a filesystem copy to ``{datadir}_bak``. 
	- Error reporting: A fix to also include the include files of a database node configuration file.
	- Alarms: Fixed an issue when the measured value was a NaN or INF.
	- MySQL: Add Node could fail due to a bug in version detection.
	- General: A fix allowing other jobs to run in parallel with remove cluster jobs.

Maintenance Release: June 26\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++

* Build: 
	- clustercontrol-1.6.1-4865

* UI:
	- Remove the default 0 sized ``cc-ldap.log`` file from the package overwriting the existing ldap log file.

Maintenance Release: June 15\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.6.1-4848
	- clustercontrol-controller-1.6.1-2605
	- clustercontrol-notifications-111

* UI:
	- Fix schedule backup verification with mysqldump.
	- Fix empty configuration template dropdown for add node (MySQL Galera).
	- Fix to allow controller host timezone when scheduling maintenance mode.
	- Fix for the schedule maintenance mode dialog closing immediately.
	- Fix stuck scrolling with the PostgreSQL advisor's page.
	- Fix missing validation for the xtrabackup ``--use-memory`` option.
	- Add 'Lock DDL per table' option for xtrabackup.
	- Fixes to cmon-events to handle filtering correctly.

* Controller:
	- Alarms/Notifications: Fixed a bug refreshing alarm thresholds. This prevented user specified thresholds in cmon_X.cnf from being applied.
	- Mongo: Adding numa node number check before installing or using numactl for mongo.
	- PostgreSQL host granting (pg_hba) fixed
	- PostgreSQL: show error log when node failed to start.
	- PostgreSQL: fixed an issue with pg_hba file error when using IPv6
	- PostgreSQL/HAProxy: HAProxy did not refresh postgres node state after rebuild of a slave.
	- ProxySQL: Include more data in the error report.
	- ProxySQL: Adding sanity check on admin port for registering existing proxysql node.
	- ProxySQL: Updated ProxySQL galera checker script to use 1.4.8
	- Galera: Fixed a crashing bug in case of missing ``wsrep_sst_auth``
	- Maxscale: Fixed so that the software will not be installed if it is already installed on the node.

* Events/Notifications:
	- Fixed a bug which ignored the configured filter. This caused e.g a Warning alarm to create a notification, when only Critical was configured. 


Initial Release: May 25\ :sup:`th`\ , 2018
++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.6.1-4801
	- clustercontrol-controller-1.6.1-2572
	- clustercontrol-cmonapi-1.6.1-324
	- clustercontrol-notifications-1.6.1-94
	- clustercontrol-cloud-1.6.1-121
	- clustercontrol-ssh-1.6.1-53

**Feature Details**

* Backup:
	- Support for MariaDB Backup for MariaDB based clusters. MariaDB Server 10.1 introduced MariaDB Compression and Data-at-Rest Encryption which is supported by MariaDB Backup (a fork of Percona XtraBackup).
	- Support for Schema(``--no-data``) or Data(``--no-create-info``) only backups and skipping extended insert(``--skip-extended-insert``) with mysqldump.
	- Support for ``--use-memory`` with xtrabackup.
	- Support for custom backup subdirectory names:
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

* PostgreSQL:
	- Synchronous Replication Slaves.
	- Multiple NICs support:
		- Deploy DB nodes using management/public IPs for monitoring connections and data/private IPs for replication traffic. 
		- Deploy HAProxy using management/public IPs and private IPs for configurations.

* Misc:
	- ServiceNow has been added as a new notifications integration.
	- Support for MaxScale 2.2.
	- Database User Management (MySQL) can now search/filter accounts on username, hostname, schema or table.
	- Node page graphs are now showing accurate time ranges and datapoint gaps.
	- Query Monitoring is using the CMON RPC API.
	- Database Growth is using the CMON RPC API.
	- Support for PHP 7.2 with an upgraded CakePHP version 2.10.9

Changes in v1.6.0
-----------------

Maintenance Release: May 18\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.6.0-2553

* Controller:
	- PostgreSQL: Support for init scripts for RHSCL PosrgreSQL packages. Please note that further tuning of the environment may be needed.
	- PostgreSQL: Improved logic to locate the Postgres log files.
	- PostgreSQL: Verifying the configuration and ``listen_addresses`` before registering the node.
	- PostgreSQL: Better error reporting in case of connection timeouts.
	- PostgreSQL: Improvements and better messaging of slave recovery in case of the host being down.
	- MySQL/Galera: Properly handle quoted ``wsrep_sst_auth`` entries.
	- Backup: Running a backup prevented other jobs from being executed.
	- Backup: A fix to prevent a backup to be uploaded to the cloud when the user did not ask for it. 
	- Error reporting: A fix for 'Access denied' when S9s CLI created a user.
	- General: Removed the printout ``RPC: No variables available for...``.
 

Maintenance Release: May 17\ :sup:`th`\ , 2018
++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.6.0-4767

* UI:
	- Fix to add missing admin port option for ProxySQL installations and registrations.
	- Fix replication lag not shown properly for MySQL Replication clusters.
	- Fix to allow changing the default region with a cloud credential.
	- Fix to restart a failed PostgreSQL job. 

Maintenance Release: May 7\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-notifications-1.6.0-88
	- clustercontrol-cloud-1.6.0-115

* Cloud:
	- Fix an issue with the security group on AWS preventing cloud deployment to work if ClusterControl was installed in the same VPC.
	- Bump version of clustercontrol-notifications to 1.6.0.

Maintenance Release: May 4\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.6.0-4699

* UI:
	- Security Vulnerability: Fixed an issue where it was possible to perform a XSS attack.
	- Cloud Deployments: Fixed a missing validation of the SSH Key.
	- LDAP: Add support to get the user group from a 'memberof' attribute

Maintenance Release: May 2\ :sup:`nd`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.6.0-2514
	- clustercontrol-1.6.0-4682
	- clustercontrol-cmonapi-1.6.0-310

* UI:
	- LDAP: Fix an issue preventing users to login with anything else than an email address.
	- Changed default basedir to ``/usr`` for MySQL Cluster (NDB) import.
	- Fix for an issue where a failed ProxySQL node was added and then not removable.
	- Fix for and issue with a blank page with DB User Management when default anonymous users (test users) are detected.
	- Add validation when trying to use reserved words with PostgreSQL deployments.
	- Tune the custom advisor dialog for lower resolution screens.
	- Fix a regression preventing error reports from being created from the frontend.

* Controller:
	- NDB Cluster: SELinux settings were not checked correctly.
	- NDB Cluster: The Install Software option was not respected.
	- NDB Cluster: Fixed an issue detecting disk space and calculating the size of the REDO log.
	- Postgres: Add Replication Slave will fail if there is an existing Postgres server running on the node, and also check if the psql client is available.
	- PostgreSQL: Forbid using reserved SQL keywords as PostgreSQL username (as it is an identifier there which can not be a reserved keyword).
	- Backup (xtrabackup): Fixed an issue where an Incremental backup could be created without having a Full backup. Now the following will happen: If there is no Full backup, the Incremental backup will be executed as a Full backup.
	- MaxScale: Version 2.2.x support.

Initial Release: April 17\ :sup:`th`\ , 2018
++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.6.0-2493
	- clustercontrol-1.6.0-4566
	- clustercontrol-cmonapi-1.6.0-303
	- clustercontrol-cloud-1.6.0-104
	- clustercontrol-ssh-1.6.0-44

Welcome to our new 1.6.0 release! Restoring your database using only a backup for disaster recovery is at times not enough. You often want to restore to a specific point in time or transaction after the backup happened. You can now do Point In Time Recovery - PITR for MySQL based databases by passing in a stop time or an event position in the binary logs as a recovery target.

We are also continuing to add cloud functionality:
	- Launch cloud instances and deploy a database cluster on AWS, Google Cloud and Azure from your on-premise installation.
	- Upload/download backups to Azure cloud storage.
	- Our cluster topology view now supports PostgreSQL replication clusters and MongoDB ReplicSets and Shards. Easily see how your database nodes are related with each other and perform actions with intuitive drag and drop motion.

As in every release we continously work on improving our UX/UI experience for our users. This time around we have re-designed the DB User Management page for MySQL based clusters. It should be easier to understand and manage your database users with this new user interface.

**Feature Details**

* Point In Time Recovery - PITR (MySQL)
	- Position and timebased recovery for MySQL based clusters.
	- Recover until the date and time given by Restore Time (Event time - stop date&time).
	- Recover until the stop position is found in the specified binary log file. If you enter binlog.001827 it will scan existing binary logs files until binlog.001827 (inclusive) and not go any further.

* Deploy and manage clusters on public Clouds (BETA)
	- Supported cloud providers
		- Amazon Web Services (VPC)
		- Google Cloud
		- Microsoft Azure.
	- Supported databases:
		- MySQL Galera
		- PostgreSQL
		- MongoDB ReplicaSet
	- Current limitations:
		- There is currently no 'accounting' in place for the cloud instances. You will need to manually remove created cloud instances.
		- You cannot add or remove a node automatically with cloud instances.
		- You cannot deploy a load balancer automatically with a cloud instance.

* Topology View
	- Support added for:
		- PostgreSQL Replication clusters.
		- MongoDB ReplicaSets and Sharded clusters.

* Misc
	- Improved cluster deployment speed by utilizing parallel jobs. Deploy more than one cluster in parallel.
	- Re-designed DB User Management for MySQL based clusters.
	- Support to deploy and manage MongoDB cluster on v3.6

Changes in v1.5.1
-----------------

Maintenance Release: April 9\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.5.1-2467 

* Controller:
	- Monitoring: SSH Optimizations to reduce the number of SSH connections on remote nodes.
	- Monitoring: CPU temperature monitoring is now configurable (and disabled by default, ``monitor_cpu_temperature`` cmon configuration option)
	- Galera: Disable P_S queries in Query Monitor during upgrade.
	- Galera: Add node - check if MariaDB version is of 10.1.31 and above. In this case mariabackup will be used.
	- ProxySQL: Fixed an issue when modifying the variable values from the UI.
	- MaxScale: Template issue with a configuration parameter not compatible with MySQL Monitor module.
	- Maxscale: Debian 9 support
	- HAProxy: If xinetd failed to install it could lead to the controller crashing.
	- Fixing a license barrier when deploying Galera cluster causing an error: "Refusing to recover node (no license)"
	- Mariadb 10.1 now requires ``wsrep_sst_method=mariabackup`` (new deploys of mariadb will always use mariabackup for SST).


Maintenance Release: March 7\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.5.1-2411
	- clustercontrol-1.5.1-4434

* Frontend/UI:  
	- CRITICAL: Fixed another issue where the wrong node was selected due to an indexing problem, which could lead to an action being executed on the wrong node.

* Controller:
	- Fixed an issue when importing keepalived.

Maintenance Release: March 6\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++

* Build: 
	- clustercontrol-controller-1.5.1-2409
	- clustercontrol-1.5.1-4425

* Controller:
	- PostgreSQL: Explicitly grant nodes by IP (in addition to hostnames) in ``pg_hba.conf``.
	- PostgreSQL: config write with includes caused invalid syntax error issues.
	- MySQL Cluster: Bug fixes to Database Growth.
	- Operational Reports: Improved handling ofdifferent gnuplot versions.
	- General: Configurable ICMP pinging. Set ``enable_icmp_ping=false`` to disable ICMP pinging (Azure requires this). By default it is true (recommended).

* Frontend/UI:
	- Installer: Permissions fixed so there are no writable files after install.
	- Fixed an issue where the wrong node was selected due to an indexing problem, which could lead to an action being executed on the wrong node.
	- Improved handling of saving email notification settings.

Maintenance Release: Feb 24\ :sup:`th`\ , 2018
++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.5.1-2390
	- clustercontrol-1.5.1-4395

* Controller:
	- PostgreSQL: Fixed a bug in the PostgreSQL config parsing causing a syntax error using ``include``.
	- Advisors: Bug fixes and corrections
	- MySQL Cluster: Fixed a number of issues around hostnames and port settings, which caused node types (data node, management node) to be improperly identified.
	- Backup (Verify Backup): Fixed a number of issues handling the Backup Verification Server.
	- Backup (Verify Backup): A backup verification email is now sent when the backup has been verified.
	- Operational Reports: Availability report issues. The ``cluster_events``/``node_events`` tables were inadvertently dropped during ClusterControl upgrades causing the stats to be reset.
	- PostgreSQL: ``pg_basebackup`` executed on a slave failed on imported clusters due to a missing grant.
	- Remove Node: Fixes to make it possible to only unregister a node (remove the node from ClusterControl management).
	- Schema: DB schema fixes to the ``server_node`` properties column by extending the size.
	- Galera/Group Replication: properly write/update the cmon_X.cnf ``mysql_server_addresses`` field to mark non galera|group_repl nodes there correctly.

* Frontend/UI:
	- Remove Node: Improved consistency in ‘Remove Node’’ dialogs.
	- MySQL/Galera: New default value for binary logging path which is now outside of the datadir.
	- Backup (MySQL based clusters): Fixed an issue where Backup Method and Backup Host drop-downs were empty.
	- Deploy/Import/Add Node: Improved Host Discovery showing the actual SSH error.
	- Deploy/Import: SSH Key Path validation was missing.
	- Charts: It was possible to select a negative range (smaller end date than start date).
	- MongoDB: Add Shards dialog got stuck when entering a hostname (SSH check never terminated).

Maintenance Release: Feb 6\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.5.1-2362
	- clustercontrol-1.5.1-4356

* Controller:
	- Stats: RAM stats updated to also account for *SReclaimable*.
	- PostgreSQL: enable ``pg_stats_statements`` extension only on non read-only nodes. 
	- Error Reporter: include more info about PostgreSQL clusters (``pg_stat_replication`` table + recovery config file)
	- MySQL: Fixed an issue handling ``!include`` directives containing quotes. The import config job (automatically executed upon a controller restart) will auto-correct broken MySQL ``!include`` directives containing quotes.

* Frontend/UI:
	- Deployment/Import dialogs: Added validation for SSH Key Path.
	- ProxySQL: Filter users with unrelated hosts when deploying ProxySQL.
	- MongoDB: Fixed a problem specifying hostnames when performing "Add Shard".

Maintenance Release: Jan 29\ :sup:`th`\ , 2018
+++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.5.1-2346

* Controller
	- Galera: A path to a ``node_recovery_lock_file`` can now be specified in ``/etc/cmon.d/cmon_N.cnf``. If set and the lock file is found on the node then the node recovery will fail until an admin/script removes this file. The cmon controller process must be restarted when this parameter has been specified. This feature maybe useful for encrypted filesystems.
	- MySQL Cluster (NDB): Fix to allow deployments on other ports than 3306.
	- MySQL: Error code 2013 (lost connection during query) is not a reason to set a node into disconnected state.
	- MySQL: Fixed up handling of ``ignore-db-dir`` in config templates on MySQL 5.5 based servers 
	- ProxySQL: Improve ProxySQL support such that ``admin-admin_credentials`` may contain multiple credentials


Maintenance Release: Jan 23\ :sup:`rd`\ , 2018
++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.5.1-4335
	- clustercontrol-controller-1.5.1-2335

* Controller:
	- Load balancers: Fix to make it possible to remove HAProxy/MaxScale even if the host is not reachable.
 	- Error reporting: Fix to always include cluster id 0 jobs in the error report.
	- Galera: Fix to disallow garbd deployment to a host having a running mysqld.
	- Replication: Improve handling of ``read_only`` when importing existing replication cluster.
	- Replication: Alert if a mysql server is not connect to any master, i.e hanging loose.
	- Postgres: Fix to recover a failed PostgreSQL server in case there is only one single postgres node in the system.
	- Postgres: Fix to prevent PostgreSQL to be restarted in case sending SIGHUP (to reload config) failed.
	- Advisors: Fix to present a clear error message for the performance schema advisors in case performance schema tables are not available for a particular MySQL version.
	- Verify Backup: Fix to correctly stage the standalone node with mysql user info (cmon user, etc)

* UI:
	- Fix properly enabling read/write split with HAProxy and MySQL Galera.
	- Fix incorrect list of nodes showing up as bootstrapping candidate (Galera).
	- Fix leaving user records behind when deleting the whole team.
	- Add an option to limit network streaming bandwidth (Mb/s) when doing a backup.
	- Fix missing "read only" port when adding HAProxy for PostgreSQL.
	- Fix showing the correct "read/write" port when adding HAProxy for PostgreSQL.
	- Fix Query Monitor for PostgreSQL to show the complete query and not truncate it.
	- Fix misleading tooltips when deploying or importing a PostgreSQL cluster.
	- Remove requirement to have the binlog enabled when adding a "SQL Node" with MySQL NDB Cluster.
	- Remove incorrect software package option when adding a "SQL Node" with MySQL NDB Cluster.
	- Fix MaxScale console port issue with using Safari.
	- Fix schedule backups to work even when the verify backup option is enabled.

Initial Release: Dec 22\ :sup:`nd`\ , 2017
++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.5.1-4265
	- clustercontrol-controller-1.5.1-2299
	- clustercontrol-cmonapi-1.5.0-290
	- clustercontrol-notifications-1.5.0-70
	- clustercontrol-ssh-1.5.0-39
	- clustercontrol-cloud-1.5.0-31
	- clustercontrol-clud-1.5.0-31

In this release we have added support to optionally use our built-in AES-256 encryption for your backups. Secure your backups for offsite or cloud storage with a flip of a checkbox.

We have also added an option to use a custom retention period per backup schedule.

There is a new Topology view (BETA) initially with MySQL based clusters to show a replication topology (incl. any load balancers) for your cluster. Use drag and drop to perform node actions, for example drag a replication slave on top of a master node which will prompt you to either rebuild the slave or change the replication master.

A new left side navigation bar provides faster page access to some of our features and the node actions are now also accessible directly from the node list.

**Feature Details**

* AES-256 Backup Encryption (and Restore):
	- Supported backup methods:
		- mysqldump, xtrabackup (MySQL).
		- pg_dump, pg_basebackup (PostgreSQL).
		- mongodump (MongoDB).

* Topology View (BETA):
	- MySQL Replication Topology.
	- MySQL Galera Topology.

* MongoDB
	- Support for MongoDB v3.4.
	- Fix to add back restore from backup.
	- Multiple NICs support. Management/public IPs for monitoring connections and data/private IPs for replication traffic.

* Misc:
	- Left side navigation.
	- Global settings breakout.
	- Quick node actions.

Changes in v1.5.0
-----------------

Maintenance Release: Dec 12\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.5.0-2273

* Controller:
	- Backups: E-mails from hourly scheduled backups were not set sent.
	- Restore External Backups: Fixed a bug where the command was wrongly quoted.
	- MySQL Replication: Improved logging during apply relay log phase and improved logic.
	- MySQL Replication: A network outage on the master, could lead to the master wrongly join back again when the network became operational again.
	- Postgres: API changes to support version 10.x.
	- Postgres: Fixed a deployment problem of version 10.x on CentOS/RedHat.
	- Postgres: ``pg_basebackup`` fix for version 10.x.
	- NDB/MySQL Cluster: Respect job datadir parameters when deploying NDB cluster (for ndbd and ndb_mgmd nodes).
	- MongoDb: Ops Monitor, Running Operations showed blank page due to a bug in a JS script.
	- Developer Studio: Better error messages for the ``host::system(..)`` call.

Maintenance Release: Dec 11\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.5.0-4183

* UI:
	- Fix license check does not work correctly with WebSSH.
	- Fix Can't rebuild PostgreSQL slave - no masters to pick from.
	- Clarify how external backup works and remove unsupported options.
	- Add ';' as acceptable character for root password when importing existing cluster.
	- Fix issues with an empty *Performance > DB Variables* page for certain setups. 


Maintenance Release: Dec 4\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.5.0-2249

* Controller:
	- Monitoring: Revert to show more samples in Overview Graph
	- Make cmon stop faster when it couldn't connect to CmonDb
	- Error reporting: minor enhancements
	- NDB: Fix some issues around executable name handling
	- PostgreSQL ``pg_basebackup`` issue bugfixed
	- A bug fix fixing empty log file name handling (avoids annoying messages in the cmon log) 
	- Able to handle special chars in database names (mysql dir name decoding)
	- Backup/mysqldump: skip dynamic tables from mysql DB: ``innodb_index_stats``,``innodb_table_stats``
	- Fix to always send out operational reports by email.

Maintenance Release: Nov 17\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++++++++
* Build:
	- clustercontrol-controller-1.5.0-2230

* Deployment:
	- A fix to upgrade openssl if deemed necessary.

Initial Release: Nov 13\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++++

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

Patch Release: Oct 30\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.4.2-2189

* Controller:
	- MySQL based cluster: if the 'mysql' database was explicitly backed up, then it was restored in the wrong way causing permission denied and the restore to fail.
	- Galera: codership repository fixes
	- Debian Jessie (Debian 9) support.

Patch Release: Oct 25\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

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

Patch Release: Oct 3\ :sup:`rd`\ , 2017
+++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.4.2-2161
	- clustercontrol-notifications-1.4.2-62
	- clustercontrol-ssh-1.4.2-32

* Controller:
	- Backups: Always execute commands on controller, only use the seen address (from node's POV) for constructing the netcat sender command line.
	- s9s_error_reporter: Updates for better compatibility with all s9s cli version.
	- s9s_error_reporter: Prevent error reporting from being blocked by other jobs.
	- Deployment failure on MariaDB 10.2 and 10.1 for Galera Custer - mariadb-compat does not exist on Debian.
	- mysqldump: Handling the backup compression level (bugfix)
	- Galera (all vendors): ``mysql_upgrade`` must only run if ``monitored_mysql_root_password`` is set. The upgrade will failed if not possible to connect
	- Galera: Fix advisor to handle ``wsrep_cluster_address`` arguments

* UI:
	- System V Init - Prevent/disable the 'cmon-events' process to start (by cron or manually) when ``<webroot>/clustercontrol/bootstrap.php`` has set ``define('CMON_EVENTS_ENABLED', false);``.
	- System V Init - Prevent/disable the 'cmon-ssh' process to start (by cron or manually) when ``<webroot>/clustercontrol/bootstrap.php`` has set ``define('SSH_ENABLED', false);``.

Patch Release: Sept 11\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++

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

Patch Release: August 25\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.4.2-2063

* Controller:
	- HAProxy: A problem with hidden properties made it impossible to view HAProxy details in the UI unless the stats admin user and password was not admin/admin. 
	- Alarms: Possibility to disable the SwapV2 alarms (set ``swap_inout_period=0`` in cmon_X.cnf)

Patch Release: August 24\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.4.2-3629

* UI:
	- Configuration Management: Correctly exclude non DB nodes from drop downs.

Patch Release: August 22\ :sup:`nd`\ , 2017
+++++++++++++++++++++++++++++++++++++++++++

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

Patch Release: August 14\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++++
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

Patch Release: August 1\ :sup:`st`\ , 2017
++++++++++++++++++++++++++++++++++++++++++

* Build: 
	- clustercontrol-1.4.2-3538

* UI:
	- Fix password reset script for php v7.
	- Fix LDAP regression with Active Directory and "samba account".

Patch Release: July 31\ :sup:`st`\ , 2017
+++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.4.2-3531

* UI:
	- Fix host filtering for Query Monitor.
	- Fix LDAP login regression.
	- Fix to show all databases for Group Replication backups.

Patch Release: July 27\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++
* Build:
	- clustercontrol-ssh-1.4.2-26

* UI: 
	- Fix not fatal duplicated symlink error creation at post-installation.

Patch Release: July 24\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++

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

Patch Release: July 11\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++

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

Patch Release: July 4\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

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

Initial Release: June 21\ :sup:`st`\ , 2017
++++++++++++++++++++++++++++++++++++++++++++

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

Patch Release: June 20\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.4.1-3393

* UI:
	- Fix for a build issue on Ubuntu/Debian. 

Patch Release: June 19\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.4.1-3384

* UI:
	- Fix for setting the Settings->Backup's retention period. In future versions *Settings -> Backups* will be deprecated/removed and can be accessed from the Backup page instead.
	- Fix inconsistent backup executed and next execution time and timezones displayed. UTC timezone is used across the backup page for now.
	- *Performance -> Transaction Log* is disabled as default. Added a slider to set sampling interval.
	- 'Add Node' and 'Add Existing Node' now has a data directory input field to change the data directory used for the new node.

Patch Release: May 24\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

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

Patch Release: May 20\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1902

* Controller:
	- Disable by default tx deadlock detection as it takes a lot of CPU. Added new param: ``db_deadlock_check_interval``
	- How often to check for deadlocks. 0 means disabled (default). Specified in seconds. (default: 0).
	- Enable in ``/etc/cmon.d/cmon_X.cnf`` (if you want to enable it, then 20 is a good value) and restart cmon.
	- Sample controller IP seen by MySQL nodes once after every cmon restart.
	- logrotate (wtmp) more often and restart accounts-daemon
	- A fix of ``show_db_users`` and ``show_db_unusued_accounts`` java scripts.

Patch Release: May 12\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

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

Patch Release: Apr 24\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++++

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

Patch Release: Apr 12\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++++

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


Initial Release: Apr 4\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++++++

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

Patch Release: Mar 29\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.4.0-2912
	- clustercontrol-controller-1.4.0-1798

* UI:
	- Create/Import NDB Cluster changes (remove the 15 node limitation)

* Controller:
	- Create NDB Cluster failed due to a bug in RAM detection.
	- Replication: Roles were not updated correctly when autorecovery was disabled.

Patch Release: Mar 13\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++++

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

Patch Release: Feb 28\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

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


Patch Release: Feb 15\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

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

Patch Release: Feb 8\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.4.0-2659

* UI:
	- Correct filtering with config parameters in the Configuration Management
	- Read-Only switcher removed from the Overview Page. You can now only change the read-only status from the Nodes page's action menu
	- Fix issue with the Nodes page's action menu where the wrong action item was selected and could accidentally be performed instead
	- Improvements to the cluster and node status updates cycles. 
	- New <webdir>/clustercontrol/bootstrap.php variable to control refresh intervals: ``define('STATUS_REFRESH_RATE', 10000);``. Default is now 10s from before 30s.

Patch Release: Feb 5\ :sup:`th`\ , 2017
+++++++++++++++++++++++++++++++++++++++

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

Patch Release: Jan 24\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++
* Build:
	- clustercontrol-1.4.0-2617

* UI
	- Fix for wrong scheduled time shown in Operational Reports.
	- Fix for inconsistent MongoDB menus.
	- Fix for confusing 'Change Organizations' option. 
	- You can more easily create a SuperAdmin/Root user to manage all your organizations/teams.

Patch Release: Jan 20\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

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

Patch Release: Jan 13\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.4.0-2585

* UI
	- Fix for an issue with having clusters from multiple controllers in one UI.

Patch Release: Jan 11\ :sup:`th`\ , 2017
++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.4.0-1651

* Controller
	- Migration of backups: better error messages and corrections the if backup files does not exist.
	- Sudo: corrects an issue where the sudo configuration (in case of using sudo with password) would overwrite the sudo settings.
	- Backup: an overlapping backup schedule will fail to execute and the user is prompted to correct the backup schedule.

Patch Release: Jan 3\ :sup:`rd`\ , 2017
+++++++++++++++++++++++++++++++++++++++

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

Patch Release: Dec 22\ :sup:`nd`\ , 2016
++++++++++++++++++++++++++++++++++++++++

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

Initial Release: Dec 12\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++++

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
	- Upload/Download backups to AWS S3 has been temporarily removed.

Changes in v1.3.2
-----------------

Path release: Oct 14\ :sup:`th`\ , 2016
+++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-1.3.2-2167
	- clustercontrol-controller-1.3.2-1504

* Controller
	- Allow two MongoDB Replica Set nodes to be deployed. Add an arbiter via 'Add Node'
	- Enable MariaDB 10.0 version for Repository mirroring

* UI
	- Fixes to database growth tables. Enable sorting on database or table columns 

Patch release: Sep 19\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++

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
 

Patch release: Sep 5\ :sup:`th`\ , 2016
+++++++++++++++++++++++++++++++++++++++

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

Initial Release: Aug 8\ :sup:`th`\ , 2016
+++++++++++++++++++++++++++++++++++++++++
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

Patch release: Jul 28\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.3.1-1372

* Controller
	- Fix for a new Percona 5.6 systemd script 
	- Fix for a new MariaDb 10.1 systemd script
	- Fix a busy loop issue (happening after some time with Proxmox provisioned LXC containers)
	- Recovery job marked as succeed when it is actually failed

Patch release: Jul 5\ :sup:`th`\ , 2016
+++++++++++++++++++++++++++++++++++++++

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


Patch Release: Jun 20\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++

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

Patch Release: Jun 16\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++

* Build:
	- clustercontrol-controller-1.3.1-1304
	- clustercontrol-1.3.1-1580

* Controller
	- Galera: Fixed a version detection issue of the galera wsrep component.

* UI
	- Performance -> Database Growth: Fixed a JavaScript error.

Initial Release: May 31\ :sup:`st`\ , 2016
++++++++++++++++++++++++++++++++++++++++++
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

Patch release: May 9\ :sup:`th`\ , 2016
+++++++++++++++++++++++++++++++++++++++

* Build:
    - clustercontrol-controller 1.3.0-1262

* ClusterControl Controller:
    - Ubuntu 15.04 fix to handle that my.cnf is a symlink
    - Missing SUPER privilege in Create Cluster causing the Incremental Xtrabackup to fail.

Patch release: May 3\ :sup:`rd`\ , 2016
+++++++++++++++++++++++++++++++++++++++

* Build: 
    - clustercontrol-1.3.0-1438
    - clustercontrol-controller 1.3.0-1257

* ClusterControl UI:
    - Permission problem in a web folder
    - Fix upgrade issue for 1.3.0 on centos/rhel

* ClusterControl Controller:
    - Fixed a compatibility issue with xtrabackup 2.2.x

Patch release: May 2\ :sup:`nd`\ , 2016
+++++++++++++++++++++++++++++++++++++++

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

Patch release: May 25\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++

* Build number:
    - clustercontrol-controller 1.3.0-1242

* ClusterControl Controller:
    - mysqldump fails for MariaDb 10.x with an erroneous parameter being used.  

Patch release: Apr 24\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++

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

Patch release: Apr 21\ :sup:`st`\ , 2016
++++++++++++++++++++++++++++++++++++++++

* Build number: 
    - clustercontrol-1.3.0-1375

* UI:
    - Fix broken Add Existing Server/Cluster dialog. 

Patch release: Apr 19\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++

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

Initial Release: Apr 18\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++++

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

Patch release: Apr 3\ :sup:`rd`\ , 2016
+++++++++++++++++++++++++++++++++++++++

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

Patch release: Mar 4\ :sup:`th`\ , 2016
+++++++++++++++++++++++++++++++++++++++++

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


Initial Release: Feb 25\ :sup:`th`\ , 2016
++++++++++++++++++++++++++++++++++++++++++

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

Patch release: Dec 11\ :sup:`th`\ , 2015
++++++++++++++++++++++++++++++++++++++++

* Build number:
	- clustercontrol-1.2.11-899
	- clustercontrol-cmonapi-1.2.11-141
	- clustercontrol-controller-1.2.11-1052

* Controller:
	- Backup: supports group [mysqldump] in my.cnf file 
	- Developer Studio:  Fixed bugs in import/export of advisors
	- Scalability fix: Use poll instead of select

Patch release: Dec 4\ :sup:`th`\ , 2015
++++++++++++++++++++++++++++++++++++++++

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


Patch release: Nov 15\ :sup:`th`\ , 2015
++++++++++++++++++++++++++++++++++++++++

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

Patch release: Nov 6\ :sup:`th`\ , 2015
++++++++++++++++++++++++++++++++++++++++

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

Patch release: Nov 2\ :sup:`nd`\ , 2015
+++++++++++++++++++++++++++++++++++++++

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
	
Patch release: Oct 23\ :sup:`rd`\ , 2015
++++++++++++++++++++++++++++++++++++++++

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

Patch release: Oct 16\ :sup:`th`\ , 2015
++++++++++++++++++++++++++++++++++++++++

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

