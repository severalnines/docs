Manage
-------

Hosts
``````

Lists of hosts being managed by ClusterControl for the specific cluster. This includes:

* ClusterControl node
* MySQL nodes (Galera, replication, group replication and standalone)
* MySQL slave nodes (Galera and replication)
* garbd nodes (Galera)
* HAproxy nodes (Galera and MySQL Cluster)
* Keepalived nodes
* MaxScale nodes
* ProxySQL nodes
* MySQL API nodes (MySQL Cluster)
* Management nodes (MySQL Cluster)
* Data nodes (MySQL Cluster)

The list also contains respective hosts' operating system, host status, ping time and process ID of the main role.

To remove a host, just select the host and click on the *Remove* button. 

.. Attention:: We strongly recommend user to avoid removing nodes from this page if it still holds a role inside ClusterControl.

Configurations
``````````````

Manage the configuration files of your database, HAProxy and Garbd nodes. For MySQL database, changes can be persisted to database variables across one node or a group of nodes at once, dynamic variables are changed directly without a restart.

.. Note:: ClusterControl does not store configuration changes history so there is no versioning at the moment. Only one version is exist at one time. It always import the latest configuration files every 30 minutes and overwrite it in cmon DB. This limitation will be improved in the upcoming release where ClusterControl shall support configuration versioning with dynamic import interval.

* **Save**
	- Save the changes that you have made and push them to the corresponding node.

* **Import**
	- Re-import configuration if you have:
		- Performed local configuration changes directly on the configuration files.
		- Restarted the mysql servers/performed a rolling restart after a configuration change.
	- ClusterControl will trigger a job to fetch the latest modification from each MySQL, HAProxy and Garbd node.

* **Change Parameter**
	- The selected parameter will be changed or created in the specified group option. ClusterControl will attempt to dynamically set the configuration value if the parameter is valid. Then, the change can be persisted in the configuration file.
	- For example, if you want to turn off ``read_only`` which is a dynamic variable, choose it from the parameter list and specify a new value 0. ClusterControl will then perform the change using 'SET GLOBAL' statement and make it persisted in the config file accordingly.

.. Attention:: If you change a global system variable, the value is remembered and used ONLY for new connections.

Base Template Files
...................

All services configured by ClusterControl use a base configuration template available under ``/usr/share/cmon/templates`` on the ClusterControl node. The following are template files provided by ClusterControl:

======================== ===========
Filename                 Description
======================== ===========
config.ini.mc            MySQL Cluster configuration file (config.ini)
garbd.cnf                Galera arbitrater daemon (garbd) configuration file.
haproxy.cfg              HAProxy configuration template for Galera Cluster.
haproxy_rw_split.cfg     HAProxy configuration template for read-write splitting.
keepalived-1.2.7.conf    Legacy keepalived configuration file (pre 1.2.7). This is deprecated.
keepalived.conf          Keepalived configuration file.
keepalived.init          Keepalived init script.
MaxScale_template.cnf    MaxScale configuration template.
mongodb-2.6.conf.org     MongoDB 2.x configuration template.
mongodb.conf.org         MongoDB 3.x configuration template.
mongodb.conf.percona     MongoDB 3.x configuration template for Percona Server for MongoDB.
mongos.conf.org          Mongo router (mongos) configuration template.
my.cnf.galera            MySQL configuration template for Galera Cluster.
my57.cnf.galera          MySQL configuration template for Galera Cluster on MySQL 5.7.
my.cnf.grouprepl         MySQL configuration template for MySQL Group Replication.
my.cnf.gtid_replication  MySQL configuration template for MySQL Replication with GTID.
my.cnf.mysqlcluster      MySQL configuration template for MySQL Cluster.
my.cnf.pxc55             MySQL configuration template for Percona XtraDB Cluster v5.5.
my.cnf.repl57            MySQL configuration template for MySQL Replication v5.7.
my.cnf.replication       MySQL configuration template for MySQL/MariaDB without MySQL’s GTID.
mysqlchk.galera          MySQL health check script template for Galera Cluster.
mysqlchk.mysql           MySQL health check script template for standalone MySQL server.
mysqlchk_rw_split.mysql  MySQL health check script template for MySQL Replication (master-slave).
mysqlchk_xinetd          Xinetd configuration template for MySQL health check.
mysqld.service.override  Systemd unit file template for MySQL service.
proxysql_template.cnf    ProxySQL configuration template.
======================== ===========

Dynamic Variables
.................

There are a number of configuration variables configurable dynamically by ClusterControl. These variables are represented with a capital letter enclosed by at sign ‘@’, for example ``@DATADIR@``. The following shows the list of variables supported by ClusterControl for MySQL-based clusters:

============================ ==============
Variable                     Description
============================ ==============
``@BASEDIR@``                Default is ``/usr``. Value specified during cluster deployment takes precendence.
``@DATADIR@``                Default is ``/var/lib/mysql``. Value specified during cluster deployment takes precendence.
``@MYSQL_PORT@``             Default is 3306. Value specified during cluster deployment takes precendence.
``@BUFFER_POOL_SIZE@``       Automatically configured based on host's RAM.
``@LOG_FILE_SIZE@`  `        Automatically configured based on host's RAM.
``@LOG_BUFFER_SIZE@``        Automatically configured based on host's RAM.
``@BUFFER_POOL_INSTANCES@``  Automatically configured based on host's CPU.
``@SERVER_ID@``              Automatically generated based on member's ``server-id``.
``@SKIP_NAME_RESOLVE@``      Automatically configured based on MySQL variables.
``@MAX_CONNECTIONS@``        Automatically configured based on host's RAM.
``@ENABLE_PERF_SCHEMA@``     Default is disabled. Value specified during cluster deployment takes precendence.
``@WSREP_PROVIDER@``         Automatically configured based on Galera vendor.
``@HOST@``                   Automatically configured based on hostname/IP address.
``@GCACHE_SIZE@``            Automatically configured based on disk space.
``@SEGMENTID@``              Default is 0. Value specified during cluster deployment takes precendence.
``@WSREP_CLUSTER_ADDRESS@``  Automatically configured based on members in the cluster.
``@WSREP_SST_METHOD@``       Automatically configured based on Galera vendor.
``@BACKUP_USER@``            Default is backupuser.
``@BACKUP_PASSWORD@``        Automatically generated and configured for backupuser.
``@GARBD_OPTIONS@``          Automatically configured based on garbd options.
``@READ_ONLY@``              Automatically configured based on replication role.
``@SEMISYNC@``               Default is disabled. Value specified during cluster deployment takes precendence.
``@NDB_CONNECTION_POOL@``    Automatically configured based on host's CPU.
``@NDB_CONNECTSTRING@``      Automatically configured based on members in the MySQL cluster.
``@LOCAL_ADDRESS@``          Automatically configured based on host's address.
``@GROUP_NAME@``             Default is "grouprepl". Value specified during cluster deployment takes precendence.
``@PEERS@``                  Automatically configured based on members in the Group Replication cluster.
============================ ==============

Load Balancer
``````````````

Manage deployment of load balancers (HAProxy, ProxySQL and MaxScale), virtual IP address (Keepalived) and Garbd. For Galera Cluster, it is also possible to add Galera arbitrator daemon (Garbd) through this interface. You can monitor the status of the job under *ClusterControl > Logs > Jobs*.

HAProxy
.......

Installs and configures an :term:`HAProxy` instance on a selected node. ClusterControl will automatically install and configure HAproxy, install ``mysqlcheck`` script (to report the MySQL healthiness) on each of database nodes as part of xinetd service and start the HAproxy service. Once the installation is complete, MySQL will listen on *Listen Port* (3307 by default) on the configured node.

This feature is indempotent, you can execute it as many times as you want and it will always reinstall everything as configured.

.. seealso:: `MySQL Load Balancing with HAProxy - Tutorial <http://www.severalnines.com/resources/clustercontrol-mysql-haproxy-load-balancing-tutorial>`_.

Create a new HAproxy instance
'''''''''''''''''''''''''''''

* **HAProxy Address**
	- Select on which host to add the load balancer. If the host is not provisioned in ClusterControl (see `Hosts`_), type in the IP address. The required files will be installed on the new host. Note that ClusterControl will access the new host using passwordless SSH.

* **Listen Port**
	- Specify the HAProxy listening port. This will be used as the load balanced MySQL connection port.

* **Max backend connections**
	- Limit the number of connection that can be made from HAProxy to each MySQL Server. Connections exceeding this value will be queued by HAProxy. A best practice is to set it to less than the ``max_connections`` to prevent connections flooding.

* **Policy**
	- Choose one of these loadbalancing algorithms:
		- leastconn - The server with the lowest number of connections receives the connection.
		- roundrobin - Each server is used in turns, according to their weights.
		- source - The same client IP address will always reach the same server as long as no server goes down.

* **Install from Package Manager**
	- Install HAproxy package through package manager.
	
* **Build from Source**
	- ClusterControl will compile the latest available source package downloaded from http://www.haproxy.org/#down. 
	- This option is only required if you intend to use the latest version of HAProxy or if you are having problem with the package manager of your OS distribution. Some older OS versions do not have HAProxy in their package repositories.


Advanced Settings
'''''''''''''''''
	
* **Stats Socket**
	- Specify the path to bind a UNIX socket for HAproxy statistics. See `stats socket <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#stats%20socket>`_.

* **Admin Port**
	- Port to listen HAproxy statistic page. 
	
* **Admin User**
	- Admin username to access HAproxy statistic page. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.
	
* **Admin Password**
	- Password for *Admin User*. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.

* **Backend Name**
	- Name for the backend. No whitespace or tab allowed.
	
* **Timeout Server (seconds)**
	- Sets the maximum inactivity time on the server side. See `timeout server <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#timeout%20server>`_.

* **Timeout Client (seconds)**
	- Sets the maximum inactivity time on the client side. See `timeout client <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-timeout%20client>`_.
	
* **Max Connections Frontend**
	- Sets the maximum per-process number of concurrent connections to the HAproxy instance. See `maxconn <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#maxconn>`_.

* **Max Connections Backend/per instance**
	- Sets the maximum per-process number of concurrent connections per backend instance. See `maxconn <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#maxconn>`_.

* **xinetd allow connections from**
	- The specified subnet will be allowed to access the ``mysqlcheck`` via as xinetd service, which listens on port 9200 on each of the database nodes. To allow connections from all IP address, use the default value, 0.0.0.0/0.

Server instances in the load balancer
'''''''''''''''''''''''''''''''''''''

* **Include**
	- Select MySQL servers in your cluster that will be included in the load balancing set.

* **Role**
	- Supported roles:
		- Active - The server is actively used in load balancing.
		- Backup - The server is only used in load balancing when all other non-backup servers are unavailable.

* **Remove**
	- Remove the selected HAProxy node.

Add an existing HAproxy instance
''''''''''''''''''''''''''''''''

* **HAProxy Address**
	- Select on which host to add the load balancer. If the host is not provisioned in ClusterControl (see `Hosts`_), type in the IP address. The required files will be installed on the new host. Note that ClusterControl will access the new host using passwordless SSH.

* **cmdline**
	- Specify the command line that ClusterControl should use to start the HAproxy service.

* **Port**
	- Port to listen HAproxy admin/statistic page (if enable).
	
* **Admin User**
	- Admin username to access HAproxy statistic page. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.
	
* **Admin Password**
	- Password for *Admin User*. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.

* **LB Name**
	- Name for the backend. No whitespace or tab allowed.
	
* **HAproxy Config**
	- Location of HAproxy configuration file on the target node.

* **Stats Socket**
	- Specify the path to bind a UNIX socket for HAproxy statistics. See `stats socket <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#stats%20socket>`_.

Keepalived
..........

:term:`Keepalived` requires two HAProxy nodes in order to provide virtual IP address failover. By default, this IP will be assigned to HAProxy1 instance. If the node goes down, the IP will be automatically failover to HAProxy2.

Create a new Keepalived instance
'''''''''''''''''''''''''''''''''

* **Haproxy1**
	- Select the primary HAProxy node (installed or imported using `HAProxy`_).
	
* **Haproxy2**
	- Select the secondary HAProxy node (installed or imported using `HAProxy`_).

* **Virtual IP**
	- Assigns a virtual IP address. The IP address should not exist in any node in the cluster to avoid conflict.

* **Network Interface** 
	- Specify a network interface to bind the virtual IP address.

* **Install Keepalived**
	- Starts installation of Keepalived.
	
Add an existing Keepalived instance
'''''''''''''''''''''''''''''''''''

* **Haproxy1**
	- Select the primary HAProxy node (installed or imported using `HAProxy`_).
	
* **Haproxy2**
	- Select the secondary HAProxy node (installed or imported using `HAProxy`_).

* **Virtual IP**
	- Assigns a virtual IP address. The IP address should not exist in any node in the cluster to avoid conflict.

* **Network Interface** 
	- Specify a network interface to bind the virtual IP address.

* **Install Keepalived**
	- Starts installation of Keepalived.

Garbd
.....

Exclusie for Galera Cluster. Galera arbitrator daemon (:term:`garbd`) can be installed to avoid network partitioning/split-brain scenarios.

Create a new Garbd instance
'''''''''''''''''''''''''''

* **Garbd Address**
	- Manually specify the new garbd hostname or IP address or select a host from the list. That host cannot be an existing Galera node.
    
* **CmdLine**
	- Garbd command line used to start garbd process on the target node.

* **Install Garbd**
	- Starts the installation of garbd.
    
Add an existing Garbd instance
''''''''''''''''''''''''''''''

* **Garbd Address**
	- Manually specify the new garbd hostname or IP address or select a host from the list. That host cannot be an existing Galera node.
    
* **Port**
    - Garbd port. Default is 4567.

* **CmdLine**
	- Garbd command line used to start garbd process on the target node.

* **Install Garbd**
	- Starts the import of garbd.

Remove Garbd
'''''''''''''

* **Remove**
	- Remove the selected garbd node. This will:
    
		1. Stop garbd service on that node.
		2. Remove the process monitoring and node from ClusterControl.

.. Note:: Removing garbd from ClusterControl does not uninstall the existing garbd packages.

MaxScale
........

MaxScale is an is an intelligent proxy that allows forwarding of database statements to one or more database servers using complex rules, a semantic understanding of the database statements and the roles of the various servers within the backend cluster of databases.

You can deploy or add existing MaxScale node as a load balancer and query router for your Galera Cluster, MySQL/MariaDB replication and MySQL cluster. For new deployment using ClusterControl, by default it will create two production services:

* RW - Implements a read-write split access.
* RR - Implements round-robin access.

To remove MaxScale, go to *ClusterControl > Nodes > MaxScale node* and click on the '-' icon next to it. We have published a blog post with deployment example in `this blog post <http://severalnines.com/blog/how-deploy-and-manage-maxscale-using-clustercontrol>`_.

Create MaxScale Instance
'''''''''''''''''''''''''

Use this wizard to install MaxScale as MySQL load balancer.

* **MaxScale Address**
	- IP address of the node where MaxScale will be installed. ClusterControl must be able to perform passwordless SSH to this host. 

* **MaxScale Admin Username**
	- MaxScale admin username. Default is 'admin'.

* **MaxScale Admin Password**
	- Password for *MaxScale Admin Username*. Default is 'mariadb'.

* **MaxScale MySQL Username**
	- MariaDB/MySQL user that will be used by MaxScale to access and monitor the MariaDB/MySQL nodes in your infrastructure.

* **MaxScale MySQL Password**
	- Password of *MaxScale MySQL Username*

* **Threads**
	- How many threads MaxScale is allowed to use.

* **CLI Port**
	- Port for MaxAdmin command line interface. Default is 6603

* **RR Port**
	- Port for round-robin access. Default is 4006.

* **RW Port**
	- Port for read-write split access. Default is 4008.

* **Debug Port**
	- Port for MaxScale debug information. Default it 4442.

* **Include**
	- Select MySQL servers in your cluster that will be included in the load balancing set.

Add Existing MaxScale
'''''''''''''''''''''

If you already have MaxScale installed in your setup, you can easily add it to ClusterControl to benefit from health monitoring and access to MaxAdmin - MaxScale’s CLI from the same interface you use to manage the database nodes. 

The only requirement is to have passwordless SSH configured between ClusterControl node and host where MaxScale is running.

* **MaxScale Address**
	- IP address of the existing MaxScale server.

* **CLI Port**
	- Port for the MaxAdmin command line interface on the target server.
	
ProxySQL
.........

Introduced in v1.4.0 and exclusive for MySQL Replication. By default, ClusterControl deploys ProxySQL in read/write split mode - your read-only traffic will be sent to slaves while your writes will be sent to a writable master by creating two host groups. ProxySQL will also work together with the new automatic failover mechanism added in ClusterControl 1.4.0 - once failover happens, ProxySQL will detect the new writable master and route writes to it. It all happens automatically, without any need for the user to take action.

Choose where to install
''''''''''''''''''''''''

Specify the host that you want to install ProxySQL. You can use an existing database server or use another host by specifying the hostname or IPv4 address.

* **Server Address**
	- List of existing servers provisioned under ClusterControl.

* **Port**
	- ProxySQL service port. Default is 6032.

* **Add a new address**
	- Specify the hostname or IP address of the host. This host must be accessible via passwordless SSH from ClusterControl node.

Add ProxySQL Users
''''''''''''''''''

Two ProxySQL Users are required, one for administration and another one for monitoring. ClusterControl will create both during deployment.

* **Administration User**
	- ProxySQL administration user name.

* **Administration Password**
	- Password *Administration User*.

* **Monitor User**
	- ProxySQL monitoring user name.

* **Monitor Password**
	- Password for *Monitor User*

Add database user
'''''''''''''''''

You can use existing database user (created outside ProxySQL) or you can let ClusterControl create a new database user under this section. ProxySQL works in the middle, between application and backend MySQL servers, so the database users need to be able to connect from the ProxySQL IP address.

* **Use existing DB User**
	- DB User: The database user name.
	- DB User Password: Password for  *DB User*.
	
.. Note:: The user must exist on the DB nodes, and allowed access from the ProxySQL server.

* **Create new DB User**
	- DB User: The database user name.
	- DB Password: Password for *DB Users*.
	- DB Name: Database name in "database.table" format. To GRANT against all tables, use wildcard, for example: "mydb.*".
	- Type in the MySQL privilege(s): ClusterControl will load the privilege name along the key press. Multiple privileges is possible.

Select instances to balance
'''''''''''''''''''''''''''

Choose which server to be included into the load balancing set.

* **Server Instance**
	- List of MySQL Replication nodes.
	
* **Include**
	- Toggle to YES to include it. Otherwise, choose NO.

* **Max Replication Lag**
	- How many seconds replication lag should be allowed before marking the node as unhealthy. Default value is 10.

* **Max Connection**
	- Maximum connections to be sent to the backend servers. It's recommended to match the ``max_connections`` value of the backend servers.

* **Weight**
	- This value is used to adjust the server's weight relative to other servers. All servers will receive a load proportional to their weight relative to the sum of all weights. The higher the weight, the higher the priority.

Implicit Transactions
''''''''''''''''''''''

* **Are you using implicit transactions?**
	- YES - If you rely on ``SET AUTOCOMMIT=0`` to create a transaction.
	- NO - If you explicitly use ``BEGIN`` or ``START TRANSACTION`` to create a transaction.

Processes
`````````

Configures ClusterControl to monitor external processes that are not part of the cluster, e.g. a web server or an application server. ClusterControl will actively monitor these processes and make sure that they are always up and running by executing the check expression command.

To add a new process to be monitored by ClusterControl, click on *Add Custom Managed Process*.

* **Host/Group**
	- Select the managed host.

* **Process Name**
	- Enter the process name.

* **Start Command**
	- OS command to start the process.

* **Pidfile**
	- Full path to the process identifier file.

* **GREP Expression**
	- OS command to check the existence of the process.

* **Remove**
	- Remove the managed process from the list of processes managed by ClusterControl.

* **Deactivate**
	- Disable the managed process.

Schemas and Users
``````````````````

ClusterControl provides a simple interface to manage database schemas and privileges. All of the changes are automatically synced to all database nodes in the cluster.

Users
.....
Provides MySQL user management interface for this cluster. Users and privileges can be set directly and retrieved from the cluster so ClusterControl is always in sync with the managed MySQL databases. Users can be created across more than one cluster at once.

You can choose individual node by clicking on the respective node or all nodes in the cluster by clicking on the respective cluster in the side menu.

Active Accounts
'''''''''''''''
Shows all active accounts across clusters, which are currently active or were connected since the last server restart.

Inactive Accounts
'''''''''''''''''
Shows all accounts across clusters that are not been used since the last server restart. Server must have been running for at least 8 hours to check for inactives accounts.

You can drop particular accounts by clicking at the multiple checkboxes and click 'Drop User' button to initiate the action.

Create Account
'''''''''''''''
Creates a new MySQL user for the chosen MySQL node or cluster. 

================== ============
Field              Description
================== ============
Server             Hostname of the user. Wildcard (%) is permitted.
Username           Specify the username.
Password           Specify the password *Username*.
Verify Password    Re-enter the same password for *Username*.
All Privileges     Allow all privileges, similar to 'ALL PRIVILEGES' option.
Database           Specify the database or table name. It can be either in '*.*', 'db_name', 'db_name.*' or 'db_name.tbl_name' format.
Require SSL        Tick the checkbox if the user must be authenticate using SSL. The checkbox is disabled if you have not configured SSL encryption for MySQL server.
================== ============

Upload Dumpfiles
................

Upload the schema and the data files. Currently only mysqldump is supported and must not contain sub-directories. The following formats are supported:

* dumpfile.sql
* dumpfile.sql.gz
* dumpfile.sql.zip
 
In order to use this feature, set ``post_max_size`` and ``upload_max_filesize`` in ``php.ini`` to 256M or more. Make sure you restart Apache to apply the PHP changes. Location of :term:`php.ini` may vary depending on your operating system, infrastructure type and PHP settings.

* **Browse**
	- Browse the location of dump file to upload.

* **Upload**
	- Start the uploading process. If uploaded, the dump file should be located under ``[wwwroot]/cmon/upload/schema`` directory.

* **Reset**
	- Reset the file name specified.

The bottom of the page shows list of uploaded dump files. You can install the selected dump file into the database or remove the selected file from the ClusterControl repository.
 

Create Database
...............

Creates a database in the cluster:

* **Database Name**
	- Enter the name of the database to be created.

* **Create Database**
	- Creates the database. ClusterControl will ensure the database exists on all nodes in the cluster.

Software Packages
``````````````````

Allows users to manage packages, upload new versions to ClusterControl’s repository, and select which package to use for deployments. In order to use this feature, set ``post_max_size`` and ``upload_max_filesize`` in php.ini to 256M or more. Make sure you restart Apache to apply the PHP changes. Location of :term:`php.ini` may vary depending on your operating system, infrastructure type and PHP settings.

.. Note:: This feature is intended for packages installed without using package repository. If the MySQL server is installed through package repository and you want to upgrade your MySQL servers, please skip this and see `Upgrades`_ section.

* **Package Name**
	- Assign a name for the new package.

* **Create**
	- Create the package.

* **Upload**
	- Uploads files to an existing package.

* **Available Packages - Database Software**
	- List of softwares and packages. The package *Selected for Deployment* will be rolled out to new nodes, and used for upgrades.
	- Check *Delete* and click *Save*, to delete the selected package from ClusterControl server.

Upgrades
`````````

Performs software upgrade using the software uploaded at *ClusterControl > Manage > Software Packages*. ClusterControl will use the package you specified to perform the upgrade on all active database nodes.

* **Package Name**
	- Select package to perform the upgrade.

* **Install**
	- Installs the selected package on the active nodes.

.. Note:: The above two features are not applicable for the vendors that are installed using OS package repository, e.g, Percona XtraDB Cluster and MariaDB Galera Cluster

* **Upgrade**
	- Upgrades are online and are performed on one node at a time. The node will be stopped, then software will be updated, and then the node will be started again. If a node fails to upgrade, the upgrade process is aborted.
	- Upgrades should only be performed when it is as little traffic as possible on the cluster.
	- If the MySQL server is installed from package repository, clicking on this will trigger an upgrade job using the respective package manager.

* **Rolling Restart**
	- Performs a rolling node restart. This stops each node one at a time, waits for it to restart with the new version, before moving to the next node. The cluster is upgraded while it is online and available.

* **Stop/Start**
	- If an online upgrade using rolling restart is not supported, e.g., if it is a major version upgrade with incompatible changes, you will need to perform an offline stop/start. This will let ClusterControl stop the cluster, perform the upgrade and then restart the cluster with the new version.

For a step-by-step walkthrough of how to perform database software upgrades, please review `this blog post <http://www.severalnines.com/blog/patch-updates-and-new-version-upgrades-your-database-clusters>`_.

Custom Advisors
```````````````

Create threshold based advisors with host or MySQL statistics without needing to write your own JS script (like all the default scripts under "Developer Studio"). The threshold advisor allows you to set threshold to be alerted on if a metric falls below or raises above the threshold and stays there for a specified timeframe.

Clicking on 'Create Custom Advisor' and 'Edit Custom Advisor' will open a new dialog, which described as follows:

* **Type**
	- Type of custom advisor. At the moment, only Threshold is supported.

* **Applies To**
	- Choose the target cluster.

* **Resource**
	- Threshold resources.
		- Host: Host metrics collected by ClusterControl.
		- Node: Database node metrics collected by ClusterControl.

* **Hosts**
	- Target host(s) in the chosen cluster. You can select individual host or all hosts monitored under this cluster.

Condition
.........

* **If metric**
	- List of metrics monitored by ClusterControl. Choose one metric to create a threshold condition.

* **Condition**
	- Type of conditions for the Warning and Critical values.

* **For(s)**
	- Timeframe in seconds before falling/raising an alarm.

* **Warning**
	- Value for warning threshold.

* **Critical**
	- Value for critical threshold.

* **Max Values seen for selected period**
	- ClusterControl provides preview of already recorded data in a graph to help you determine accurate values for timeframe, warning and critical.

Notification Settings
.....................

Select the notification service configured under *ClusterControl > Settings > Notification Settings*. This notification service determines what is the endpoint of this advisors once conditions are met. It could be email and/or Pagerduty alert.

Description
...........

Describe the Advisor and provide instructions on what actions that may be needed if the threshold is triggered.

Available variables:

================= ============
Variable          Description
================= ============
%CLUSTER%         Selected cluster
%CONDITION%       Condition
%CRITICAL_VALUE%  Critical Value
%DURATION%        Duration
%HOSTNAME%        Selected host or node
%METRIC%          Metric
%METRIC_GROUP%    Group for the selected metric
%RESOURCE%        Selected resource
%TYPE%            Type of the custom advisor
%WARNING_VALUE%   Warning Value
================= ============

Developer Studio
````````````````

Provides functionality to create Advisors, auto tuners, or “mini programs” right within your web browser based on `ClusterControl DSL (Domain Specific Language) <../../dsl.html>`_. The DSL syntax is based on JavaScript, with extensions to provide access to ClusterControl’s internal data structures and functions. The DSL allows you to execute SQL statements, run shell commands/programs across all your cluster hosts, and retrieve results to be processed for advisors/alerts or any other actions. Developer Studio is a development environment to quickly create, edit, compile, run, test, debug and schedule your JavaScript programs.

Advisors in ClusterControl are powerful constructs; they provide specific advice on how to address issues in areas such as performance, security, log management, configuration, storage space, etc. They can be anything from simple configuration advice, warning on thresholds or more complex rules for predictions, or even cluster-wide automation tasks based on the state of your servers or databases. 

ClusterControl comes with a set of basic advisors that include rules and alerts on security settings, system checks (NUMA, Disk, CPU), queries, innodb, connections, performance schema, Galera configuration, NDB memory usage, and so on. The advisors are open source under an MIT license, and available on `GitHub <https://github.com/severalnines/s9s-advisor-bundle>`_. Through the Developer Studio, it is easy to import new advisors as a JS bundle, or export your own for others to try out.

* **New**
	- Name - Specify the file name including folders if you need. E.g. "shared/helpers/cmon.js" will create all appropriate folders if they don't exist yet.
	- File content:
		- Empty file - Creates a new empty file.
		- Galera Template - Creates a new file containing skeleton code for Galera monitoring.
		- Generic MySQL Template - Creates a new file containing skeleton code for generic MySQL monitoring.

* **Import**
	- Imports advisor bundle. Supported format is ``.tar.gz``. See `s9s-advisor-bundle <https://github.com/severalnines/s9s-advisor-bundle>`_.

* **Export**
	- Exports the advisor's directory to a ``.tar.gz`` file. The exported file can be imported to Developer Studio through *ClusterControl > Manage > Developer Studio > Import* function.

* **Advisors**
	- Opens the Advisor list page. See `Advisors <performance.html#advisors>`_ section.

* **Save**
	- Saves the file.
	
* **Move**
	- Moves the file around between different subdirectories.

* **Remove**
	- Removes the script.

* **Compile**
	- Compiles the script.

* **Compile and run**
	- Compiles and runs the script. The output appears under *Message*, *Graph* or *Raw response* tab down below.
	- The arrow next to the “Compile and Run” button allows us to change settings for a script and, for example, pass some arguments to the ``main()`` function.

* **Schedule Advisor**
	- Schedules the script as an advisor.

We have covered this in details `in this blog post <https://severalnines.com/blog/clustercontrol-developer-studio-write-your-first-database-advisor>`_. For full documentation on ClusterControl Domain Specific Language, see `ClusterControl DSL <../../dsl.html>`_ section.
