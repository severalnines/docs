Manage
-------

Hosts
``````

Lists of hosts being managed by ClusterControl for the specific cluster. This includes:

* ClusterControl node
* MySQL nodes (Galera, replication and standalone)
* MySQL slave nodes (Galera and replication)
* garbd nodes (Galera)
* HAproxy nodes (Galera and MySQL Cluster)
* Keepalived nodes
* MySQL API nodes (MySQL Cluster)
* Management nodes (MySQL Cluster)
* Data nodes (MySQL Cluster)

To remove a host, just select the host and click on the *Remove* button. 

.. Attention:: We strongly recommend user to avoid removing nodes from this page if it still hold a role inside ClusterControl.

Configurations
``````````````

Manage the configuration files of your database nodes. From here you can edit and/or detect whether your database configuration files are in sync and do not diverge. Any changes will not take effect until the database server/process is restarted.

* **Edit/View**
	- Edit and view your database configuration files. Any changes will not take effect until the database server/processes is restarted.

* **Restart**
	- This button will only appear under *Action* when ClusterControl detects configuration changes. Click to restart the database service for selected node.

* **Reimport Configuration**
	- Re-import configuration if you have:
		- Performed local configuration changes directly on the configuration files
		- Restarted the mysql servers/performed a rolling restart after a configuration change

* **Create New Template**
	- Create a new MySQL configuration template file. This template can be used when adding a new node.

Load Balancer
``````````````

Deploys load balancers (HAProxy), virtual IP (Keepalived) for MySQL-based clusters (NDB, Galera). For Galera Cluster, it is also possible to add Galera arbitrator daemon (Garbd) through this interface. You can monitor the status of the job under *ClusterControl > Logs > Jobs*.


HAproxy
.......

Installs and configures an :term:`HAProxy` instance on a selected node. ClusterControl will automatically install and configure HAproxy, install ``mysqlcheck`` script (to report the MySQL healthiness) on each of database nodes as part of xinetd service and start the HAproxy service. Once the installation is complete, MySQL will listen on *Listen Port* (3307 by default) on the configured node.

This feature is indempotent, you can execute it as many times as you want and it will always reinstall everything as configured.

.. seealso:: `MySQL Load Balancing with HAProxy - Tutorial <http://www.severalnines.com/resources/clustercontrol-mysql-haproxy-load-balancing-tutorial>`_.

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
+++++++++++++++++
	
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
++++++++++++++++++++++++++++++++++++++

* **Include**
	- Select MySQL servers in your cluster that will be included in the load balancing set.

* **Role**
	- Supported roles:
		- Active - The server is actively used in load balancing.
		- Backup - The server is only used in load balancing when all other non-backup servers are unavailable.

* **Remove**
	- Remove the selected HAProxy node.

Keepalived
..........

:term:`Keepalived` requires two HAProxy nodes in order to provide virtual IP address failover. By default, this IP will be assigned to Haproxy1 instance. If the node goes down, the IP will be automatically failover to Haproxy2.

* **Haproxy1**
	- Select the primary HAProxy node.
	
* **Haproxy2**
	- Select the secondary HAProxy node.

* **Virtual IP**
	- Assigns a virtual IP address. The IP address should not exist in any node in the cluster.

* **Network Interface** 
	- Specify a network interface to bind the virtual IP address.

* **Install Keepalived**
	- Starts installation of Keepalived.

Garbd
.....

Galera arbitrator daemon (:term:`garbd`) can be installed to avoid network partitioning/split-brain scenarios.

* **Host**
	- A new host or select a host from the list. That host cannot be an existing Galera node.

* **Install Garbd**
	- Starts the installation of garbd.

* **Remove**
	- Remove the selected garbd node. This will:
		1. Stop garbd service on that node.
		2. Remove the process monitoring and node from ClusterControl.

.. Note:: Removing garbd from ClusterControl does not uninstall the existing garbd packages.

Spreadsheet (BETA)
``````````````````

Provides data predection engine in the rows and columns of a grid and can be manipulated and used in calculations. The spreadsheet subsystem can be controlled and used by sending JSON messages to the C++ backend and receiving the JSON reply messages when the given operation is finished. You can send JSON requests trough the CMON RPC interface.


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

Schema and Users
````````````````

ClusterControl provides a simple interface to manage database schemas. It is possible to create databases, upload dump files, manage users and privileges. All of the changes are automatically synced to all database nodes in the cluster.

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
	- Click to create a database.

Privileges
..........

Provides list of users and privileges created from the ClusterControl UI. Users and privileges will be synced over to database nodes. To create a new user click on *Create User* button and specify values of the following fields:

* **User**
	- MySQL username.

* **Host**
	- Host that the user can connect from. You can specify a wildcard, hostname or IP address.

* **Password**
	- The password for corresponding user and host.

.. Note:: You can only see users and privileges that is created through ClusterControl in the list. It does not import any existing MySQL users and privileges.

To assign privilege to a user, click *Assign Privileges to User* button. Check the appropriate privileges with respective database and user host value.

* **Privileges**
	- Select privileges for the user.

* **Database**
	- Specify the database on which to apply the privileges.

* **User Host**
	- Specify the host from which the user will connect.

Software Packages
``````````````````
Allows users to manage packages, upload new versions to ClusterControl’s repository, and select which package to use for deployments. In order to use this feature, set ``post_max_size`` and ``upload_max_filesize`` in php.ini to 256M or more. Make sure you restart Apache to apply the PHP changes. Location of :term:`php.ini` may vary depending on your operating system, infrastructure type and PHP settings.

.. Note:: This feature is intended for packages installed without using package repository. If the MySQL server is installed through package repository and you want to upgrade your MySQL servers, please skip this and see `Upgrades`_ section.

* **Package Name**
	- Assign a name for the new package.

* **Create**
	- Create the package.

* **Upload**
	- Upload files to an existing package.

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
	- If the MySQL server is installed through package repository, clicking on this will trigger an upgrade job through the respective package manager.

* **Rolling Restart**
	- Performs a rolling node restart. This stops each node one at a time, waits for it to restart with the new version, before moving to the next node. The cluster is upgraded while it is online and available.

* **Stop/Start**
	- If an online upgrade using rolling restart is not supported, e.g., if it is a major version upgrade with incompatible changes, you will need to perform an offline stop/start. This will let ClusterControl stop the cluster, perform the upgrade and then restart the cluster with the new version.

For a step-by-step walkthrough of how to perform database software upgrades, please review `this blog post <http://www.severalnines.com/blog/patch-updates-and-new-version-upgrades-your-database-clusters>`_.

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
