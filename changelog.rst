.. _changelog:

Change Logs
===========

These change logs list details about updates in each version of ClusterControl.

Changes in v1.2.10
-------------------

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
		- cluster_certs_store = path to storage location of SSL related files, defaults to /etc/ssl/<clustertype>/<cluster_id>
	* Monitoring:
		- New binary format for host statistics which consumes less space (cpu, memory, disk, network stats)
		- Fixed disk statistics collector to support non 4K block sizes
	* Security:
		- E-mails do not contain IP addresses when hostnames are specified in the cmon configuration
		- Password will not be logged (to jobs for example) or sent anymore
	* Alarms:
		- Alarm will be raised when there is a missing MySQL GRANT
		- Alarm will be raised/sent when there is a high IO wait for a period (>=50% average in 10 minutes)
		- New alarm for Galera configuration problems
		- Improved alarm emails (for example: high cpu/mem usage mails will contain the output of 'top' command)
	* RPC:
		- Several new RPC interfaces (directly on the daemon) for jobs and statistics handling
		- The web client has started to migrate over to use RPC API calls instead of the CMON API
	* Testing:
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

