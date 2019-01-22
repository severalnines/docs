.. _MySQL - Manage:

Manage
-------

Hosts
+++++

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

.. Attention:: We strongly recommend users to avoid removing node from this page if it still holds a role inside ClusterControl.

.. _MySQL - Manage - Configurations:

Configurations
+++++++++++++++

Manages the configuration files of your database, HAProxy and Garbd nodes. For MySQL database, changes can be persisted to database variables across one node or a group of nodes at once, dynamic variables are changed directly without a restart.

.. Note:: ClusterControl does not store configuration changes history so there is no versioning at the moment. Only one version exists at one time. It imports the latest configuration files every 30 minutes and overwrites it in CMON database. This limitation will be improved in the upcoming release where ClusterControl shall support configuration versioning with dynamic import interval.

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

.. _MySQL - Manage - Configurations - Base Template Files:

Base Template Files
````````````````````

All services configured by ClusterControl use a base configuration template available under ``/usr/share/cmon/templates`` on the ClusterControl node. You can directly modify the file to suit your deployment policy however, this directory will be replaced after a package upgrade.

To make sure your custom configuration template files persist across upgrade, store your template files under ``/etc/cmon/templates`` directory (ClusterControl 1.6.2 and later). When ClusterControl loads up the template file for deployment, files under ``/etc/cmon/templates`` will always have higher priority over the files under ``/usr/share/cmon/templates``. If two files having identical name exist on both directories, the one located under ``/etc/cmon/templates`` will be used.

The following are template files provided by ClusterControl related to MySQL/MariaDB:

============================ ===========
Filename.                    Description
============================ ===========
config.ini.mc                MySQL Cluster configuration file (config.ini)
garbd.cnf                    Galera arbitrator daemon (garbd) configuration file.
haproxy.cfg                  HAProxy configuration template for Galera Cluster.
haproxy_rw_split.cfg         HAProxy configuration template for read-write splitting.
keepalived-1.2.7.conf        Legacy keepalived configuration file (pre 1.2.7). This is deprecated.
keepalived.conf              Keepalived configuration file.
keepalived.init              Keepalived init script.
MaxScale_template.cnf        MaxScale configuration template.
my.cnf.galera                MySQL configuration template for Galera Cluster.
my57.cnf.galera              MySQL configuration template for Galera Cluster on MySQL 5.7.
my-cnf-backup-secrets.cnf    MySQL configuration template for generated backup user.
my.cnf.grouprepl             MySQL configuration template for MySQL Group Replication.
my.cnf.gtid_replication      MySQL configuration template for MySQL Replication with GTID.
my.cnf.mdb10x-galera         MariaDB configuration template for MariaDB Galera 10 and later.
my.cnf.mdb10x-replication    MariaDB configuration template for MariaDB Replication 10 and later.
my.cnf.mysqlcluster          MySQL configuration template for MySQL Cluster.
my.cnf.pxc55                 MySQL configuration template for Percona XtraDB Cluster v5.5.
my.cnf.repl57                MySQL configuration template for MySQL Replication v5.7.
my.cnf.repl80                MySQL configuration template for MySQL Replication v8.0.
my.cnf.replication           MySQL configuration template for MySQL/MariaDB without MySQL’s GTID.
my-root.cnf                  Config file used by mysql in order to perform log rotation.
mysqlchk.galera              MySQL health check script template for Galera Cluster.
mysqlchk.mysql               MySQL health check script template for standalone MySQL server.
mysqlchk_rw_split.mysql      MySQL health check script template for MySQL Replication (master-slave).
mysqlchk_xinetd              Xinetd configuration template for MySQL health check.
mysqld.service.override      Systemd unit file template for MySQL service.
mysql_logrotate              Log rotation configuration template for MySQL.
proxysql_galera_checker.sh   ProxySQL health check script for Galera Cluster.
proxysql_logrotate           Log rotation configuration template for ProxySQL.
proxysql_template.cnf        ProxySQL configuration template.
============================ ===========

.. _MySQL - Manage - Configurations - Dynamic Variables:

Dynamic Variables
``````````````````

There are a number of configuration variables which are configurable dynamically by ClusterControl. These variables are represented with a capital letter enclosed by at sign ‘@’, for example ``@DATADIR@``. The following shows the list of variables supported by ClusterControl for MySQL-based clusters:

============================ ==============
Variable                     Description
============================ ==============
``@BASEDIR@``                Default is ``/usr``. Value specified during cluster deployment takes precedence.
``@DATADIR@``                Default is ``/var/lib/mysql``. Value specified during cluster deployment takes precedence.
``@MYSQL_PORT@``             Default is 3306. Value specified during cluster deployment takes precedence.
``@BUFFER_POOL_SIZE@``       Automatically configured based on host's RAM.
``@LOG_FILE_SIZE@``          Automatically configured based on host's RAM.
``@LOG_BUFFER_SIZE@``        Automatically configured based on host's RAM.
``@BUFFER_POOL_INSTANCES@``  Automatically configured based on host's CPU.
``@SERVER_ID@``              Automatically generated based on member's ``server-id``.
``@SKIP_NAME_RESOLVE@``      Automatically configured based on MySQL variables.
``@MAX_CONNECTIONS@``        Automatically configured based on host's RAM.
``@ENABLE_PERF_SCHEMA@``     Default is disabled. Value specified during cluster deployment takes precedence.
``@WSREP_PROVIDER@``         Automatically configured based on Galera vendor.
``@HOST@``                   Automatically configured based on hostname/IP address.
``@GCACHE_SIZE@``            Automatically configured based on disk space.
``@SEGMENTID@``              Default is 0. Value specified during cluster deployment takes precedence.
``@WSREP_CLUSTER_ADDRESS@``  Automatically configured based on members in the cluster.
``@WSREP_SST_METHOD@``       Automatically configured based on Galera vendor.
``@BACKUP_USER@``            Default is ``backupuser``.
``@BACKUP_PASSWORD@``        Automatically generated and configured for ``backupuser``.
``@GARBD_OPTIONS@``          Automatically configured based on garbd options.
``@READ_ONLY@``              Automatically configured based on replication role.
``@SEMISYNC@``               Default is disabled. Value specified during cluster deployment takes precedence.
``@NDB_CONNECTION_POOL@``    Automatically configured based on host's CPU.
``@NDB_CONNECTSTRING@``      Automatically configured based on members in the MySQL cluster.
``@LOCAL_ADDRESS@``          Automatically configured based on host's address.
``@GROUP_NAME@``             Default is ``grouprepl``. Value specified during cluster deployment takes precedence.
``@PEERS@``                  Automatically configured based on members in the Group Replication cluster.
============================ ==============

.. _MySQL - Manage - Load Balancer:

Load Balancer
++++++++++++++

Manages deployment of load balancers (HAProxy, ProxySQL and MaxScale), virtual IP address (Keepalived) and Garbd. For Galera Cluster, it is also possible to add Galera arbitrator daemon (Garbd) through this interface.

ProxySQL
````````

Introduced in v1.4.0 and exclusive for MySQL-based clusters. By default, ClusterControl deploys ProxySQL in read/write split mode - your read-only traffic will be sent to slaves while your writes will be sent to a writable master by creating two host groups. ProxySQL will also work together with the new automatic failover mechanism added in ClusterControl 1.4.0 - once failover happens, ProxySQL will detect the new writable master and route writes to it. It all happens automatically, without any user intervention.

.. seealso:: `Database Load Balancing for MySQL and MariaDB with ProxySQL - Tutorial <https://severalnines.com/resources/tutorials/proxysql-tutorial-mysql-mariadb>`_.

Deploy ProxySQL
''''''''''''''''

**Choose where to install**

Specify the host that you want to install ProxySQL. You can use an existing database server or use another new host by specifying the hostname or IPv4 address.

* **Server Address**
	- List of existing servers provisioned under ClusterControl.

* **Port**
	- ProxySQL load-balanced port. Default is 6033.

* **Add a new address**
	- Specify the hostname or IP address of the host. This host must be accessible via passwordless SSH from ClusterControl node.

**ProxySQL Configuration**

* **Import Configuration**
	- Deploys a new ProxySQL based on an existing ProxySQL instance. The source instance must be added first into ClusterControl. Once added, you can choose the source ProxySQL instance from a dropdown list.

**ProxySQL User Credentials**

Two ProxySQL users are required, one for administration and another one for monitoring. ClusterControl will create both during deployment.

* **Administration User**
	- ProxySQL administration user name.

* **Administration Password**
	- Password *Administration User*.

* **Monitor User**
	- ProxySQL monitoring user name.

* **Monitor Password**
	- Password for *Monitor User*

**Add database user**

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

**Select instances to balance**

Choose which server to be included into the load balancing set.

* **Server Instance**
	- List of MySQL servers monitored by ClusterControl.
	
* **Include**
	- Toggle to YES to include it. Otherwise, choose NO.

* **Max Replication Lag**
	- How many seconds replication lag should be allowed before marking the node as unhealthy. Default value is 10.

* **Max Connection**
	- Maximum connections to be sent to the backend servers. It's recommended to match or lower than the ``max_connections`` value of the backend servers.

* **Weight**
	- This value is used to adjust the server's weight relative to other servers. All servers will receive a load proportional to their weight relative to the sum of all weights. The higher the weight, the higher the priority.

**Implicit Transactions**

* **Are you using implicit transactions?**
	- YES - If you rely on ``SET AUTOCOMMIT=0`` to create a transaction.
	- NO - If you explicitly use ``BEGIN`` or ``START TRANSACTION`` to create a transaction.
	
Import ProxySQL
'''''''''''''''

If you already have ProxySQL installed in your setup, you can easily import it into ClusterControl to benefit from monitoring and management of the instance.

**Existing ProxySQL location**

* **Server Address**
	- Specify the hostname or IP address. You can choose from the dropdown list and type in the new host.

* **Listening Port**
	- ProxySQL load-balanced port. Default is 6033.
	
**ProxySQL Configuration**

* **Import Configuration**
	- Adds an existing ProxySQL instance and import the configuration from another existing instance. The source instance must be added first into ClusterControl. Once added, you can choose the source ProxySQL instance from a dropdown list.

**ProxySQL User Credentials**

* **Administration User**
	- ProxySQL administration user name.

* **Administration Password**
	- Password for *Administration User*.

HAProxy
````````

Installs and configures an :term:`HAProxy` instance. ClusterControl will automatically install and configure HAProxy, install ``mysqlcheck`` script (to report the MySQL healthiness) on each of database nodes as part of xinetd service and start the HAProxy service. Once the installation is complete, MySQL will listen on *Listen Port* (3307 by default) on the configured node.

This feature is idempotent, you can execute it as many times as you want and it will always reinstall everything as configured.

.. seealso:: `MySQL Load Balancing with HAProxy - Tutorial <http://www.severalnines.com/resources/clustercontrol-mysql-haproxy-load-balancing-tutorial>`_.

Deploy HAProxy
'''''''''''''''

* **Server Address**
	- Select on which host to add the load balancer. If the host is not provisioned in ClusterControl (see `Hosts`_), type in the IP address. The required files will be installed on the new host. Note that ClusterControl will access the new host using passwordless SSH.

* **Policy**
	- Choose one of these load balancing algorithms:
		- leastconn - The server with the lowest number of connections receives the connection.
		- roundrobin - Each server is used in turns, according to their weights.
		- source - The same client IP address will always reach the same server as long as no server goes down.

* **Listen Port (Read/Write)**
	- Specify the HAProxy listening port. This will be used as the load balanced MySQL connection port for read/write connections.

* **Install for read/write splitting (master-slave replication)**
	- Toggled on if you want HAProxy to use another listener port for read-only. A new text box will appear right next to the *Listen Port (Read/Write)* text box. Default to 3308.
	
* **Build from Source**
	- ClusterControl will compile the latest available source package downloaded from http://www.haproxy.org/#down. 
	- This option is only required if you intend to use the latest version of HAProxy or if you are having problem with the package manager of your OS distribution. Some older OS versions do not have HAProxy in their package repositories.

**Advanced Settings**
	
* **Stats Socket**
	- Specify the path to bind a UNIX socket for HAProxy statistics. See `stats socket <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#stats%20socket>`_.

* **Admin Port**
	- Port to listen HAProxy statistic page. 
	
* **Admin User**
	- Admin username to access HAProxy statistic page. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.
	
* **Admin Password**
	- Password for *Admin User*. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.

* **Backend Name**
	- Name for the backend. No whitespace or tab allowed.
	
* **Timeout Server (seconds)**
	- Sets the maximum inactivity time on the server side. See `timeout server <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#timeout%20server>`_.

* **Timeout Client (seconds)**
	- Sets the maximum inactivity time on the client side. See `timeout client <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-timeout%20client>`_.
	
* **Max Connections Frontend**
	- Sets the maximum per-process number of concurrent connections to the HAProxy instance. See `maxconn <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#maxconn>`_.

* **Max Connections Backend/per instance**
	- Sets the maximum per-process number of concurrent connections per backend instance. See `maxconn <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#maxconn>`_.

* **xinetd allow connections from**
	- The specified subnet will be allowed to access the ``mysqlcheck`` (or ``mysqlcheck_rw_split`` for read/write splitting) via as xinetd service, which listens on port 9200 on each of the database nodes. To allow connections from all IP address, use the default value, 0.0.0.0/0.

**Server instances in the load balancer**

* **Include**
	- Select MySQL servers in your cluster that will be included in the load balancing set.

* **Role**
	- Supported roles:
		- Active - The server is actively used in load balancing.
		- Backup - The server is only used in load balancing when all other non-backup servers are unavailable.

* **Connection Address**
	- Pick the IP address where HAProxy should be listening to on the host.

Import HAProxy
''''''''''''''

* **HAProxy Address**
	- Select on which host to add the load balancer. If the host is not provisioned in ClusterControl (see `Hosts`_), type in the IP address. The required files will be installed on the new host. Note that ClusterControl will access the new host using passwordless SSH.

* **cmdline**
	- Specify the command line that ClusterControl should use to start the HAProxy service. You can verify this by using ``ps -ef | grep haproxy`` and retrieve the full command how the HAProxy process started. Copy the full command line and paste it in the textfield.

* **Port**
	- Port to listen HAProxy admin/statistic page (if enable).
	
* **Admin User**
	- Admin username to access HAProxy statistic page. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.

.. Note:: You will need an admin user/password set in HAProxy configuration otherwise you will not see any HAProxy stats.
	
* **Admin Password**
	- Password for *Admin User*. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.

* **LB Name**
	- Name for the backend. No whitespace or tab allowed.
	
* **HAProxy Config**
	- Location of HAProxy configuration file (haproxy.cfg) on the target node.

* **Stats Socket**
	- Specify the path to bind a UNIX socket for HAProxy statistics. See `stats socket <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#stats%20socket>`_. 
	- Usually, HAProxy writes the socket file to  ``/var/run/haproxy.socket`` . This is needed by ClusterControl to monitor HAProxy. This is usually defined in the ``haproxy.cfg`` file, and the line looks like:

.. code-block:: bash

	stats socket /var/run/haproxy.socket user haproxy group haproxy mode 600 level

Keepalived
``````````

:term:`Keepalived` requires two HAProxy nodes or two or more ProxySQL instances in order to provide virtual IP address failover. By default, this IP address will be assigned to instance 'Keepalived 1'. If the node goes down, the IP address will be automatically failover to 'Keepalived 2' accordingly.

Deploy Keepalived
'''''''''''''''''

* **Load balancer type**
	- Only two types of loadbalancers are supported to integrate with Keepalived, HAProxy and ProxySQL. For ProxySQL, you can deploy more than 2 Keepalived instances.

* **Keepalived 1**
	- Select the primary Keepalived node (installed or imported using `HAProxy`_ or `ProxySQL`_).
	
* **Add Keepalived Instance**
	- Shows additional input field for secondary Keepalived node.

* **Remove Keepalived Instance**
	- Hides additional input field for secondary Keepalived node.

* **Virtual IP**
	- Assigns a virtual IP address. The IP address should not exist in any node in the cluster to avoid conflict.

* **Network Interface** 
	- Specify a network interface to bind the virtual IP address. This interface must able to communicate with other Keepalived instances and support IP protocol 112 (VRRP) and unicasting.
	
Import Keepalived
'''''''''''''''''

* **Keepalived 1**
	- Specify the IP address or hostname of the primary Keepalived node.
	
* **Add Keepalived Instance**
	- Shows additional input field for secondary Keepalived node.

* **Remove Keepalived Instance**
	- Hides additional input field for secondary Keepalived node.

* **Virtual IP**
	- Assigns a virtual IP address. The IP address should not exist in any node in the cluster to avoid conflict.

Garbd
``````

Exclusive for Galera Cluster. Galera arbitrator daemon (:term:`garbd`) can be installed to avoid network partitioning or split-brain scenarios.

Deploy Garbd
''''''''''''

* **Server Address**
	- Manually specify the new garbd hostname or IP address or select a host from the list. That host cannot be an existing Galera node.
    
* **CmdLine**
	- Garbd command line to start garbd process on the target node.
    
Import Garbd
'''''''''''''

* **Garbd Address**
	- Manually specify the new garbd hostname or IP address or select a host from the list. That host cannot be an existing Galera node.
    
* **Port**
  - Garbd port. Default is 4567.

* **CmdLine**
	- Garbd command line to start garbd process on the target node.

MaxScale
````````

MaxScale is an intelligent proxy that allows forwarding of database statements to one or more database servers using complex rules, a semantic understanding of the database statements and the roles of the various servers within the backend cluster of databases.

You can deploy or import existing MaxScale node as a load balancer and query router for your Galera Cluster, MySQL/MariaDB replication and MySQL Cluster. For new deployment using ClusterControl, by default it will create two production services:

* RW - Implements a read-write split access.
* RR - Implements round-robin access.

To remove MaxScale, go to *ClusterControl > Nodes > MaxScale node* and click on the '-' icon next to it. We have published a blog post with deployment example in this blog post, `How to Deploy and Manage MaxScale using ClusterControl <http://severalnines.com/blog/how-deploy-and-manage-maxscale-using-clustercontrol>`_.

Deploy MaxScale 
''''''''''''''''

Use this wizard to install MariaDB MaxScale as MySQL/MariaDB load balancer.

* **Server Address**
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

* **CLI Port (Port for command line)**
	- Port for MaxAdmin command line interface. Default is 6603

* **RR Port (Port for round robin listener)**
	- Port for round-robin listener. Default is 4006.

* **RW Port (Port for read/write split listener)**
	- Port for read-write split listener. Default is 4008.

* **Debug Port (Port for debug information)**
	- Port for MaxScale debug information. Default it 4442.

* **Include**
	- Select MySQL servers in your cluster that will be included in the load balancing set.

Import MaxScale
'''''''''''''''

If you already have MaxScale installed in your setup, you can easily import it into ClusterControl to benefit from health monitoring and access to MaxAdmin - MaxScale’s CLI from the same interface you use to manage the database nodes. The only requirement is to have passwordless SSH configured between ClusterControl node and host where MaxScale is running.

* **MaxScale Address**
	- IP address of the existing MaxScale server.

* **CLI Port (Port for the Command Line Interface)**
	- Port for the MaxAdmin command line interface on the target server.

.. _MySQL - Manage - Processes:

Processes
++++++++++

Manages external processes that are not part of the database system, e.g. a load balancer or an application server. ClusterControl will actively monitor these processes and make sure that they are always up and running by executing the check expression command.

* **Host/Group**
	- Select the managed host.

* **Process Name**
	- Enter the process name. E.g: "Apache 2".

* **Start Command**
	- OS command to start the process. E.g: "/usr/sbin/apache2 -DFOREGROUND".

* **Pidfile**
	- Full path to the process identifier file. E.g: "/var/run/apache2/apache2.pid".

* **GREP Expression**
	- OS command to check the existence of the process. The command must return 0 for true, and everything else for false. E.g: "pidof apache2".

* **Remove**
	- Removes the managed process from the list of processes managed by ClusterControl.

* **Deactivate**
	- Disables the selected process.

.. _MySQL - Manage - Schemas and Users:

Schemas and Users
+++++++++++++++++

Manages database schemas and users' privileges. 

Users
``````

Shows a summary of MySQL user and privileges for the cluster. All of the changes are automatically synced to all database nodes in the cluster. For master-slave setup, ClusterControl will create the schema and user on the active master.

You can filter the list by username, hostname, database or table in the text box. Click on *Edit* to update the existing user or *Drop User* to remove the existing user. Click on *Create New User* to open the user creation wizard:

* **Username**
	- MySQL username.

* **Password**
	- Password for *Username*. Minimum requirement is 4 characters.

* **Hostname**
	- Hostname or IP address of the user or client. Wildcard (%) is permitted.

* **Max Queries Per Hour**
	- Available if you click *Show Advanced Options*. Maximum queries this user can perform in an hour. Default is 0 (unlimited).

* **Max Updated Per Hour**
	- Available if you click *Show Advanced Options*. Maximum update operations this user can perform in an hour. Default is 0 (unlimited).

* **Max Connections Per Hour**
	- Available if you click *Show Advanced Options*. Maximum connections allowed for this user in an hour. Default is 0 (unlimited).

* **Max User Connections**
	- Available if you click *Show Advanced Options*. Maximum connections allowed for this user. Default is 0 (unlimited).

* **Requires SSL**
	- Available if you click *Show Advanced Options*. Toggle on the option if this user must be authenticated using SSL. Default is false.

* **Privileges**
	- Specify the privilege for this user. If the *Privileges* text box is active, it will list out all possible privileges on the server.
	- Specify the database or table name. It can be in ``*.*``, ``{database_name}``, ``{database_name}.*`` or ``{database_name}.{table_name}`` format.

* **Add Statement**
	- Add another *Privileges* statement builder entry for this user.

Inactive Users
````````````````

Shows all accounts across clusters that are not been used since the last server restart. Server must have been running for at least 1 hour to check for inactive accounts.

You can drop particular accounts by clicking the *Drop User* button to initiate the action.

Import Database Dumpfile
``````````````````````````

Upload the schema and the data files to the selected database node. Currently only mysqldump is supported and must not contain sub-directories. The following formats are supported:

* dumpfile.sql
* dumpfile.sql.gz
* dumpfile.sql.bz2

* **Import dumpfile on**
	- Perform import operation on the selected database node.

* **Import dumpfile to database**
	- Specify the target database.

* **Specify path to dumpfile**
	- The dumpfile must be located on the ClusterControl server.

Create Database
````````````````

Creates a database in the cluster:

* **Database Name**
	- Enter the name of the database to be created.

* **Create Database**
	- Creates the database. ClusterControl will ensure the database exists on all nodes in the cluster.

.. _MySQL - Manage - Upgrades:

Upgrades
++++++++

Performs minor software upgrade, for example from MySQL 5.7.x to MySQL 5.7.y in rolling upgrade fashion. The job will perform the software upgrade based on what is available on the package repository for the particular vendor.

.. Attention:: MySQL major version upgrade is not supported by ClusterControl. Major version upgrade has to be performed manually as it involves some risks like database package removal, configuration compatibility concern, connectors compatibility, etc.

* **Upgrade**
	- Upgrades are online and are performed on one node at a time. The node will be stopped, then software will be updated, and then the node will be started again. If a node fails to upgrade, the upgrade process is aborted.
	- Upgrades should only be performed when it is as little traffic as possible on the cluster.
	- If the MySQL server is installed from package repository, clicking on this will trigger an upgrade job using the respective package manager.

* **Rolling Restart**
	- Performs a rolling node restart. This stops each node one at a time, waits for it to restart with the new version, before moving to the next node. The cluster is upgraded while it is online and available.

* **Stop/Start**
	- If an online upgrade using rolling restart is not supported, e.g., if it is a major version upgrade with incompatible changes, you will need to perform an offline stop/start. This will let ClusterControl stop the cluster, perform the upgrade and then restart the cluster with the new version.

For a step-by-step walkthrough of how to perform database software upgrades, please look at this blog post, `Patch Updates and New Version Upgrades of your Database Clusters <http://www.severalnines.com/blog/patch-updates-and-new-version-upgrades-your-database-clusters>`_.

.. _MySQL - Manage - Custom Advisors:

Custom Advisors
+++++++++++++++

Manages threshold-based advisors with host or PostgreSQL statistics without needing to write your own JavaScript script (like all the default scripts under `Developer Studio`_). The threshold advisor allows you to set threshold to be alerted on if a metric falls below or raises above the threshold and stays there for a specified timeframe.

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
``````````

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

Advisor Description
````````````````````

Describe the Advisor and provide instructions on what actions that may be needed if the threshold is triggered. Available variables substitutions:

================= ============
Variable          Description
================= ============
%CLUSTER%         Selected cluster
%CONDITION%       Condition
%DURATION%        Duration
%HOSTNAME%        Selected host or node
%METRIC%          Metric
%METRIC_GROUP%    Group for the selected metric
%RESOURCE%        Selected resource
%TYPE%            Type of the custom advisor
%CRITICAL_VALUE%  Critical Value
%WARNING_VALUE%   Warning Value
================= ============

.. _MySQL - Manage - Developer Studio:

Developer Studio
++++++++++++++++

Provides functionality to create Advisors, Auto Tuners, or Mini Programs right within your web browser based on :ref:`ClusterControl DSL`. The DSL syntax is based on JavaScript, with extensions to provide access to ClusterControl's internal data structures and functions. The DSL allows you to execute SQL statements, run shell commands/programs across all your cluster hosts, and retrieve results to be processed for advisors/alerts or any other actions. Developer Studio is a development environment to quickly create, edit, compile, run, test, debug and schedule your JavaScript programs.

Advisors in ClusterControl are powerful constructs; they provide specific advice on how to address issues in areas such as performance, security, log management, configuration, storage space, etc. They can be anything from simple configuration advice, warning on thresholds or more complex rules for predictions, or even cluster-wide automation tasks based on the state of your servers or databases. 

ClusterControl comes with a set of basic advisors that include rules and alerts on security settings, system checks (NUMA, Disk, CPU), queries, InnoDB, connections, PERFORMANCE_SCHEMA, configuration, NDB memory usage, and so on. The advisors are open source under MIT license, and publicly available at `GitHub <https://github.com/severalnines/s9s-advisor-bundle>`_. Through the Developer Studio, it is easy to import new advisors as a JS bundle, or export your own for others to try out.

* **New**
	- Name - Specify the file name including folders if you need. E.g. ``shared/helpers/cmon.js`` will create all appropriate folders if they don't exist yet.
	- File content:
		- Empty file - Create a new empty file.
		- Template - Create a new file containing skeleton code for monitoring.
		- Generic MySQL Template - Create a new file containing skeleton code for generic MySQL monitoring.

* **Import**
	- Imports advisor bundle. Supported format is ``.tar.gz``. See `s9s-advisor-bundle <https://github.com/severalnines/s9s-advisor-bundle>`_.

* **Export**
	- Exports the advisor's directory to a ``.tar.gz`` format. The exported file can be imported to Developer Studio through *ClusterControl > Manage > Developer Studio > Import* function.

* **Advisors**
	- Opens the Advisor list page. See :ref:`MySQL - Performance - Advisors`.

* **Save**
	- Saves the file.
	
* **Move**
	- Moves the file around between different subdirectories.

* **Remove**
	- Removes the script.

* **Compile**
	- Compiles the script.

* **Compile and run**
	- Compile and run the script. The output appears under *Message*, *Graph* or *Raw response* tab underneath the editor.
	- The arrow next to the "Compile and Run" button allows us to change settings for a script and for example, pass some arguments to the ``main()`` function.

* **Schedule Advisor**
	- Schedules the script as an advisor.

.. seealso:: `Introducing ClusterControl Developer Studio and Creating your own Advisors in JavaScript <https://severalnines.com/blog/introducing-clustercontrol-developer-studio-and-creating-your-own-advisors-javascript>`_.

For full documentation on ClusterControl Domain Specific Language, see :ref:`ClusterControl DSL`.
