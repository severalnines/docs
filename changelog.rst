.. _changelog:

Change Logs
===========

These change logs list details about updates in each version of ClusterControl.

Changes in v1.3.1
-----------------

Patch Release: June 20th, 2016
````````````````````````````````

* Build :
  - clustercontrol-1.3.1-1655
  - clustercontrol-controller-1.3.1-1324
  - clustercontrol-cmonapi-1.3.1-198 

* Controller
  - Backup: Fixed an issue with long running backups and overrun of backup log entries (backup would not terminate properly)
  Fix for automatically correcting a wrongful 'sudo' configuration.

* UI
  - Alarms: fixed inconsistent alarm count
  - Jobs: Fixed a number of issues such as being able to Restart failed jobs

Patch Release: June 16th, 2016
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
MongoDB arbiter is now shown on the "Nodes" page

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
    - netcat port defaults to 9999  (and impossible to change)
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
		- Character set on connection + cmon.tx_deadlock_log, change to use utf8mb4 to properly encode characters in *Performance > Transaction Lo*g preventing data from being shown. Do ``mysql -ucmon -p -h127.0.0.1 cmon < /usr/share/cmon/cmon_db.sql`` to recreate this table.


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

