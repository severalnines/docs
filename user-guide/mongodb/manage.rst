Manage
-------

Hosts
``````

Lists of hosts being managed by ClusterControl for the specific cluster. This includes:

* ClusterControl node
* MongoDB shards nodes (MongoDB replica set and sharded cluster)
* MongoDB config nodes (MongoDB sharded cluster)
* MongoDB mongos nodes (MongoDB sharded cluster)
* MongoDB arbiter nodes (MongoDB replica set and sharded cluster)

To remove a host, just select the host and click on the *Remove* button. 

.. Attention:: We strongly recommend user to avoid removing nodes from this page if it still hold a role inside ClusterControl.

Configurations
``````````````

Manage the configuration files of your MongoDB nodes. For MongoDB server, changes can be persisted to database variables across one node or a group of nodes at once, dynamic variables are changed directly without a restart.

.. Attention:: ClusterControl does not store configuration changes history so there is no versioning at the moment. Only one version is exist at one time. It always import the latest configuration files every 30 minutes and overwrite it in cmon DB. This limitation will be improved in the upcoming release where ClusterControl shall support configuration versioning with dynamic import interval.

* **Save**
	- Save the changes that you have made and push them to the corresponding node.

* **Import**
	- Re-import configuration if you have:
		- Performed local configuration changes directly on the configuration files.
		- Restarted the mysql servers/performed a rolling restart after a configuration change.
	- ClusterControl will trigger a job to fetch the latest modification from each MySQL, HAProxy and Garbd node.

* **Change Parameter**
	- The selected parameter will be changed or created in the specified group option. ClusterControl will attempt to dynamically set the configuration value if the parameter is valid. Then, the change can be persisted in the configuration file.
	
	
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

There are a number of configuration variables configurable dynamically by ClusterControl. These variables are represented with a capital letter enclosed by at sign ‘@’, for example ``@DATADIR@``. The following shows the list of variables supported by ClusterControl for MongoDB-based clusters:

============================ ==============
Variable                     Description
============================ ==============
``@DATADIR@``                Default is ``/var/lib/mongodb``. Value specified during cluster deployment takes precendence.
``@MONGODB_PORT@``           Default is 27017, 27018, 27019 (depending on the cluster type). Value specified during cluster deployment takes precendence.
``@LOGDIR@``                 Automatically configured based on vendor.
``@HOST@``                   Automatically configured based on hostname/IP address.
``@SMALLFILES@``             Automatically configured based on disk space.
``@PIDFILEPATH@``            Automatically configured based on MongoDB data directory.
``@REPLICASET_NAME@``        Default is "my_mongodb_N". Value specified during cluster deployment takes precendence.
============================ ==============

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


Developer Studio
````````````````

Provides functionality to create Advisors, Auto Tuners, or “mini Programs” right within your web browser based on `ClusterControl DSL (Domain Specific Language) <../../dsl.html>`_. The DSL syntax is based on JavaScript, with extensions to provide access to ClusterControl’s internal data structures and functions. The DSL allows you to execute SQL statements, run shell commands/programs across all your cluster hosts, and retrieve results to be processed for advisors/alerts or any other actions. Developer Studio is a development environment to quickly create, edit, compile, run, test, debug and schedule your JavaScript programs.

Advisors in ClusterControl are powerful constructs; they provide specific advice on how to address issues in areas such as performance, security, log management, configuration, storage space, etc. They can be anything from simple configuration advice, warning on thresholds or more complex rules for predictions, or even cluster-wide automation tasks based on the state of your servers or databases. 

ClusterControl comes with a set of basic advisors that include rules and alerts on security settings, system checks (NUMA, Disk, CPU), queries, innodb, connections, performance schema, Galera configuration, NDB memory usage, and so on. The advisors are open source under an MIT license, and available on `GitHub <https://github.com/severalnines/s9s-advisor-bundle>`_. Through the Developer Studio, it is easy to import new advisors as a JS bundle, or export your own for others to try out.

* **New**
	- Name - Specify the file name including folders if you need. E.g. "shared/helpers/cmon.js" will create all appropriate folders if they don't exist yet.
	- File content:
		- Empty file - Create a new empty file.
		- Galera Template - Create a new file containing skeleton code for Galera monitoring.
		- Generic MySQL Template - Create a new file containing skeleton code for generic MySQL monitoring.

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
	- Remove the script.

* **Compile**
	- Compiles the script.

* **Compile and run**
	- Compile and run the script. The output appears under *Message*, *Graph* or *Raw response* tab down below.
	- The arrow next to the “Compile and Run” button allows us to change settings for a script and, for example, pass some arguments to the ``main()`` function.

* **Schedule Advisor**
	- Schedules the script as an advisor.

We have covered this in details `in this blog post <http://www.severalnines.com/blog/introducing-clustercontrol-developer-studio-creating-your-own-advisors>`_. For full documentation on ClusterControl Domain Specific Language, see `ClusterControl DSL <../../dsl.html>`_ section.
