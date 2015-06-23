.. _components:

Components
==========

ClusterControl consists of three components:

+----------------------------------+---------------------------+-----------------------------------------------------------------------------------+
| Component                        | Package naming            | Role                                                                              |
+==================================+===========================+===================================================================================+
| ClusterControl controller (cmon) | clustercontrol-controller | The brain of ClusterControl. A backend service performing automation, management, |
|                                  |                           | monitoring and scheduling tasks. All the collected data will be stored directly   |
|                                  |                           | inside CMON database                                                              |
+----------------------------------+---------------------------+-----------------------------------------------------------------------------------+
| ClusterControl REST API          | clustercontrol-cmonapi    | Interprets request and response data between ClusterControl UI and CMON database  |
+----------------------------------+---------------------------+-----------------------------------------------------------------------------------+
| ClusterControl UI                | clustercontrol            | A modern web user interface to visualize and manage the cluster. It interacts with| 
|                                  |                           | CMON controller via remote procedure call (RPC) or REST API interface             |
+----------------------------------+---------------------------+-----------------------------------------------------------------------------------+

ClusterControl Controller (CMON)
--------------------------------

ClusterControl Controller (CMON) is the core backend process that performs all automation and management procedures. It is usually installed as ``/usr/sbin/cmon``. It uses a collection of helper scripts in ``/usr/bin`` directory (prefixed with s9s\_) to complete specific tasks.

CMON controller package is available at `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_. Redhat-based systems should download and install the RPM package while Debian-based systems should download and extract the DEB package. The package name is formatted as:

* RPM package (Redhat-based systems): ``cmon-controller-[version]-[build number]-[architecture].rpm``
* DEB package (Debian-based systems): ``cmon-controller-[version]-[build number]-[architecture].deb``

To check the installed CMON controller version, simply invoke -v option:

.. code-block:: bash

	$ cmon -v
	cmon, version 1.2.6.233
	Enabled features: mongodb, libssh, mysql
	Build git-hash: 18d7bfcce920f2bf779751193c05bff9172250c5

A configuration file ``/etc/cmon.cnf`` is needed to initially setup the CMON Controller. It is possible to have several configuration files each for multiple clusters as described in the next section.

Configuration File
``````````````````

Starting from version 1.2.4, a single CMON Controller process is able to monitor one or more clusters. Each of the cluster requires one exclusive configuration file residing in the ``/etc/cmon.d/`` directory. For instance, the default CMON configuration file is located at ``/etc/cmon.cnf``, and commonly used to manage the first cluster (cluster_id=1). For the second cluster, it is possible to have another configuration file saved as ``/etc/cmon.d/cmon_2.cnf`` with cluster_id=2.

An example of file hierarchy is as follows:

+----------------------------+------------------------+--------------+-----------------------------+
| Example cluster            | Configuration file     | Cluster      | Log file location           |
|                            |                        | identifier   |                             |
+============================+========================+==============+=============================+
| Cluster #1 (Galera)        | /etc/cmon.cnf          | cluster_id=1 | logfile=/var/log/cmon.log   |
+----------------------------+------------------------+--------------+-----------------------------+
| Cluster #2 (PXC)           | /etc/cmon.d/cmon_2.cnf | cluster_id=2 | logfile=/var/log/cmon_2.log |
+----------------------------+------------------------+--------------+-----------------------------+
| Cluster #3 (MySQL Cluster) | /etc/cmon.d/cmon_3.cnf | cluster_id=3 | logfile=/var/log/cmon_3.log |
+----------------------------+------------------------+--------------+-----------------------------+
| Cluster #4 (cluster type)  | /etc/cmon.d/cmon_N.cnf | cluster_id=N | logfile=/var/log/cmon_N.log |
+----------------------------+------------------------+--------------+-----------------------------+
 
The CMON Controller will import the configuration options defined in each configuration file into the CMON database and is then used to manage cluster based on the ``cluster_id value``, representing a specific cluster.

Configuration Options
`````````````````````

All of the options and values as described below must not contain any whitespace between them. Any changes to the CMON configuration file requires a CMON service restart before they are applied.

The configuration options can be divided into 4 types, with * as indicator:

- \* - mandatory for all cluster type
- \*\* - mandatory for Galera/MySQL replication/MySQL single instance
- \*\*\* - mandatory for MySQL Cluster
- \*\*\*\* - mandatory for MongoDB/TokuMX clusters

Supported configuration options for the CMON Controller configuration file:

General
'''''''

``cluster_id=<integer>``

* \* Cluster identifier. This will be used by CMON to indicate which cluster to provision. It must be unique, i.e two clusters can not share the same ID.	
* Example: ``cluster_id=1``

``name=<string>``

* \* Cluster name.
* Example: ``name=cluster_1``

``type=<string>``

* \* Cluster type. Supported values are galera, mysql_single, mongodb, postgresql_single, replication. 
* Example: ``type=galera``

CMON
''''

``mode=<string>``

* \* CMON role. Supported values are controller, dual, agent, hostonly.
* Example: ``mode=controller``

``agentless=<boolean integer>``

* \* CMON controller mode (deprecated). Agents are no longer supported. Supported values are 0 (agentful) or 1 (agentless - default). 
* Example: ``agentless=1``

``logfile=<path to log file>``

* \* CMON log file location. This is where CMON logs its activity. The file will be automatically generated if it doesn't exist. CMON will write to syslog by default. 
* Example: ``logfile=/var/log/cmon.log``

``pidfile=<path to PID directory>``

* \* CMON process identifier file directory. It's recommended not to change the default value.	
* Example: ``pidfile=/var/run``

``enable_cluster_autorecovery=<boolean integer>``

* If undefined, CMON will perform automatic recovery if it detects cluster failure. Supported values are 1 (disable cluster recovery) or 0 (enable cluster recovery).	

* Example: ``enable_cluster_autorecovery=0``

``enable_node_autorecovery=<boolean integer>``

* If undefined, CMON will perform automatic recovery if it detects node failure. Supported values are 1 (disable node recovery) or 0 (enable node recovery).
* Example: ``enable_node_autorecovery=0``

``enable_autorecovery=<boolean integer>``

* If undefined, CMON will perform automatic recovery if it detects node or cluster failure. Supported values are 0 (disable cluster and node recovery) or 1 (enable cluster and node recovery). This setting will internally set enable_node_autorecovery and enable_cluster_autorecovery to the specified value.
* Example: ``enable_autorecovery=0``

Operating system
''''''''''''''''

``os=<string>``

* \* Operating system runs across the cluster, including ClusterControl host. Supported values are 'redhat' for Redhat-based distributions (CentOS/Fedora/Amazon Linux/Oracle Linux) or 'debian' for Debian-based distributions (Debian/Ubuntu)	
* Example: ``os=redhat``

``os_user=<string>``

* \* System user that will be used by CMON to perform automation tasks like cluster recovery, backups and upgrades. This user must be able to perform super-user activities.
* Example: ``os_user=root``

``osuser=<string>``

* Alias to ``os_user``.

``sshuser=<string>``

* Alias to ``os_user``.

``sudo="echo '<sudo password>' | sudo -S 2>/dev/null"``

* If sudo user uses password, specify the sudo command (with sudo password) here.
* Example: ``sudo="echo 'My5ud0' | sudo -S 2>/dev/null"``

``hostname=<string>``

* Hostname or IP address of the CMON host.
* Example: ``hostname=192.168.0.10``

``wwwroot=<path to CMONAPI and ClusterControl UI>``

* \* Path to CMONAPI and ClusterControl UI. If not set, it defaults to '/var/www/html' for Redhat-based distributions or '/var/www' for Debian-based distributions.
* Example: ``wwwroot=/var/www/html``

SSH
'''

``ssh_identify=<path to SSH key private key or key pair>``

* The SSH key or key pair file that will be used by CMON to connect managed nodes (including ClusterControl node) passwordlessly. If undefined, CMON will use the home directory of ``os_user`` and look for ``.ssh/id_rsa`` file.	
* Example: ``ssh_identity=/root/.ssh/id_rsa``

``ssh_port=<integer>``

* The SSH port used by CMON to connect to managed nodes. If undefined, CMON will use port 22.	
* Example: ``ssh_port=22``

``ssh_options=<string>``

* The SSH options used by CMON to connect to managed nodes. Details on SSH manual page.	
* Example: ``ssh_options='-nqtt'``

CMON database
'''''''''''''

``mysql_hostname=<string>``

* \* The MySQL hostname or IP address where CMON database resides. Using IP address is recommended.	
* Example: ``mysql_hostname=192.168.0.10``

``mysql_password=<string>``

* \* The MySQL password for user cmon to connect to CMON database. Alphanumeric values only.
* Example: ``mysql_password=cMonP4ss``

``mysql_port=<integer>``

* \* The MySQL port used by CMON to connecto to CMON database.	
* Example: ``mysql_port=3306``

``mysql_basedir=<MySQL base directory location>``

* \* The MySQL base directory used by CMON to find MySQL client related binaries.	
* Example: ``mysql_basedir=/usr``

``mysql_bindir=<MySQL binary directory location>``

* \* The MySQL binary directory used by CMON to find MySQL client related binaries.	
* Example: ``mysql_bindir=/usr/bin``

Host monitoring
'''''''''''''''

``monitored_mountpoints=<list of paths to be monitored>``

* \* The MySQL/MongoDB/TokuMX/PostgreSQL data directory used by database nodes for disk performance in comma separated list.	
* Example: ``monitored_mountpoints=/var/lib/mysql,/mnt/data/mysql``

``monitored_nics=<list of NICs to be monitored>``

* List of network interface card (NIC) to be monitored for network performance in comma separated list.	
* Example: ``monitored_nics=eth1,eth2``

MySQL managed nodes
'''''''''''''''''''

``mysql_server_addresses=<string>``

* \*\*/\*\*\* Comma separated list of target MYSQL IP addresses. For MySQL Cluster, this should be the list of MySQL API node IP addresses.	
* Example: ``mysql_server_addresses=192.168.0.11,192.168.0.12,192.168.0.13``

``datanode_addresses=<string>``

* \*\*\* Exclusive for MySQL Cluster. Comma separated list of data node IP addresses.
* Example: ``datanode_addresses=192.168.0.41,192.168.0.42``

``mgmnode_addresses=<string>``

* \*\*\* Exclusive for MySQL Cluster. Comma separated list of management node IP addresses.
* Example: ``mgmnode_addresses=192.168.0.51,192.168.0.52``

``ndb_connectstring=<string>``

* \*\*\* Exclusive for MySQL Cluster. NDB connection string for the cluster.
* Example: ``ndb_connectstring=192.168.0.51:1186,192.168.0.52:1186``

``ndb_binary=<string>``

* \*\*\* Exclusive for MySQL Cluster. NDB binary for data node. Supported values are ndbd or ndbmtd.
* Example: ``ndb_binary=ndbmtd``

MongoDB/TokuMX managed hosts
''''''''''''''''''''''''''''

``mongodb_server_addresses=<string>``

* \*\*\*\* Exclusive for MongoDB/TokuMX. Comma separated list of MongoDB/TokuMX shard or replica IP addresses with port.
* Example: ``mongodb_server_addresses=192.168.0.11:27017,192.168.0.12:27017,192.168.0.13:27017``

``mongoarbiter_server_addresses=<string>``

* \*\*\*\* Exclusive for MongoDB/TokuMX. Comma separated list of MongoDB/TokuMX arbiter IP addresses with port.	
* Example: `mongoarbiter_server_addresses=192.168.0.11:27019,192.168.0.12:27019,192.168.0.13:27019`

``mongocfg_server_addresses=<string>``

* \*\*\*\* Exclusive for MongoDB/TokuMX. Comma separated list of MongoDB/TokuMX config server IP addresses with port.	
* Example: ``mongocfg_server_addresses=192.168.0.11:27019,192.168.0.12:27019,192.168.0.13:27019``

``mongos_server_addresses=<string>``

* \*\*\*\* Exclusive for MongoDB/TokuMX. Comma separated list of MongoDB/TokuMX mongos IP addresses with port.
* Example: ``mongos_server_addresses=192.168.0.11:27017,192.168.0.12:27017,192.168.0.13:27017``

``mongodb_basedir=<location MongoDB base directory>``

* \*\*\*\* Exclusive for MongoDB/TokuMX. The MongoDB/TokuMX base directory used by CMON to find mongodb client related binaries.	
* Example: ``mongodb_basedir=/usr``

Statistic collections
'''''''''''''''''''''

``db_stats_collection_interval=<integer>``

* \* Database statistic collections interval in seconds performed by CMON. The lowest value is 1. Default is 30 seconds.
* Example: ``db_stats_collection_interval=30``

``host_stats_collection_interval=<integer>``

*  \* Host statistic collections interval in seconds performed by CMON. The lowest value is 1. Default is 30 seconds.
* Example: ``host_stats_collection_interval=30``

``db_schema_stats_collection_interval=<integer>``

* How often database growth and table checks are performed in seconds. This translates to information_schema queries. 0 means disable.
* Example: ``db_schema_stats_collection_interval=10800``

``db_long_query_time_alarm=<integer>``

* If a query takes longer than ``db_long_query_time_alarm`` to execute, an alarm will be raised containing detailed information about blocked and long running transactions. Default is 10 seconds.
* Example: ``db_long_query_time_alarm=5``

``enable_mysql_timemachine=<boolean integer>``

* This determine whether ClusterControl should enable MySQL time machine status and variable collections. The status time machine allows you to select status variable for a time range and compare the values at the start and end of that range from ClusterControl UI. Default is 0, meaning it is disabled.
* Example: ``enable_mysql_timemachine=1``

Encryption
''''''''''

``cmondb_ssl_key=<file path>``

* Path to SSL key, for SSL encryption between CMON process and the CMON database.	
* Example: ``cmondb_ssl_key=/etc/ssl/mysql/client-key.pem``

``cmondb_ssl_cert=<file path>``

* Path to SSL certificate, for SSL encryption between CMON process and the CMON database.
* Example: ``cmondb_ssl_cert=/etc/ssl/mysql/client-cert.pem``

``cmondb_ssl_ca=<file path>``

* Path to SSL CA, for SSL encryption between CMON process and the CMON database.
* Example: ``cmondb_ssl_ca=/etc/ssl/mysql/ca-cert.pem``

``cluster_ssl_key=<file path>``

* Path to SSL key, for SSL encryption between CMON process and managed MySQL Servers.
* Example: ``cluster_ssl_key=/etc/ssl/mysql/client-key.pem``

``cluster_ssl_cert=<file path>``

* Path to SSL cert, for SSL encryption between CMON process and managed MySQL Servers.
* Example: ``cluster_ssl_cert=/etc/ssl/mysql/client-cert.pem``

``cluster_ssl_ca=<file path>``

* Path to SSL CA, for SSL encrption between CMON and managed MySQL Servers.	
* Example: ``cluster_ssl_ca=/etc/ssl/mysql/ca-cert.pem``

``cluster_certs_store=<directory path>``

* Path to storage location of SSL related files. This is required when you want to add new node in an encrypted Galera cluster.	
* Example: ``cluster_certs_store=/etc/ssl/galera/cluster_1``


Agentless
`````````

Starting from version 1.2.5, ClusterControl introduces an agentless mode of operation. There is now no need to install agents on the managed nodes. User only need to install the CMON controller package on the ClusterControl host, and make sure that passwordless SSH and the CMON database user GRANTs are properly set up on each of the managed hosts.

The agentless mode is the default and recommended type of setup. Starting from version 1.2.9, an agentful setup is no longer supported.

CMON database
`````````````

The CMON Controller requires a MySQL database running on ``mysql_hostname`` as defined in CMON configuration file. The database name and user is ‘cmon’ and is immutable.

The CMON database is the persistent store for all monitoring data collected from the managed nodes, as well as all ClusterControl meta data (e.g. what jobs there are in the queue, backup schedules, backup statuses, etc.). ClusterControl CMONAPI contains logic to query the CMON DB, e.g. for cluster statistics that is presented in the ClusterControl UI.

The CMON database dump files are shipped with the CMON Controller package and can be found under ``/usr/share/cmon`` once it installed. When performing a manual upgrade from an older version, it is compulsory to apply the SQL modification files relative to the upgrade. For example, when upgrading from version 1.2.0 to version 1.2.5, apply all SQL modification files between those versions in sequential order:

1. cmon_db_mods-1.2.0-1.2.1.sql
2. cmon_db_mods-1.2.3-1.2.4.sql
3. cmon_db_mods-1.2.4-1.2.5.sql

Note that there is no 1.2.1 to 1.2.2 SQL modification file. That means there is no changes on the CMON database structure between those versions. The database upgrade procedure will not remove any of the existing data inside the CMON database. You can just use simple MySQL import command as follow:

.. code-block:: bash

	mysql -f -ucmon -p[cmon_password] -h[mysql_hostname] -P[mysql_port] cmon < /usr/share/cmon/cmon_db.sql
	mysql -f -ucmon -p[cmon_password] -h[mysql_hostname] -P[mysql_port] cmon < /usr/share/cmon/cmon_data.sql

.. Note:: Replace the variables in square brackets with respective values defined in CMON configuration file.

MySQL user 'cmon' needs to have proper access to CMON DB by performing following grant:

Grant all privileges to 'cmon' at ``hostname`` value (as defined in CMON configuration file) on ClusterControl host: 

.. code-block:: mysql

	GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'[hostname]' IDENTIFIED BY '[mysql_password]' WITH GRANT OPTION;

Grant all privileges for 'cmon' at 127.0.0.1 on ClusterControl host:

.. code-block:: mysql

	GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'127.0.0.1' IDENTIFIED BY '[mysql_password]' WITH GRANT OPTION;

For each managed database server, on the managed database server, grant all privileges to cmon at controller's ``hostname`` value (as defined in CMON configuration file) on each of the managed database host:

.. code-block:: mysql

	GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'[hostname]' IDENTIFIED BY '[mysql_password]' WITH GRANT OPTION;

Don't forget to run ``FLUSH PRIVILEGES`` on each of the above statement so the grant will be kept after restart. If users deploy using the deployment package generated from the Severalnines Cluster Configurator and installer script, this should be configured correctly.

Database Client
```````````````

For MySQL-based clusters, CMON Controller requires MySQL client to connect to CMON database. This package usually comes by default when installing MySQL server required by CMON database.

For MongoDB/TokuMX cluster, the CMON Controller requires to have both MySQL and MongoDB client packages installed and correctly defined in CMON configuration file on ``mysql_basedir`` and ``mongodb_basedir`` option.

For PostgreSQL, the CMON controller doesn't require any PostgreSQL clients installed on the node. All PostgreSQL commands will be executed locally on the managed PostgreSQL node via SSH.

If users deploy using the deployment package generated from the Severalnines Cluster Configurator, this should be configured automatically.

ClusterControl REST API (CMONAPI)
---------------------------------

The CMONAPI is a RESTful interface, and exposes all ClusterControl functionality as well as monitoring data stored in the CMON DB. Each CMONAPI connects to one CMON DB instance. Several instances of the ClusterControl UI can connect to one CMONAPI as long as they utilize the correct CMONAPI token and URL. The CMON token is automatically generated during installation and is stored inside ``config/bootstrap.php``.

By default, the CMONAPI is running on Apache and located under ``/var/www/html/cmonapi`` (Redhat/CentOS/Ubuntu >14.04) or ``/var/www/cmonapi`` (Debian/Ubuntu <14.04). The value is relative to ``wwwroot`` value defined in CMON configuration file. The web server must support rule-based rewrite engine and able to follow symlinks.

The CMONAPI page can be accessed through following URL:

**http or https://<ClusterControl IP address or hostname\>/cmonapi**

Both ClusterControl CMONAPI and UI must be running on the same version to avoid misinterpretation of request and response data. For instance, ClusterControl UI version 1.2.5 needs to connect to the CMONAPI version 1.2.5.

.. Attention:: We are gradually in the process of migrating all functionalities in REST API to RPC interface. Kindly expect the REST API to be obselete in the near future.

ClusterControl UI
-----------------

ClusterControl UI provides a modern web user interface to visualize the cluster and perform tasks like backup scheduling, configuration changes, adding nodes, rolling upgrades, etc. It requires a MySQL database called 'dcps', to store cluster information, users, roles and settings. It interacts with CMON controller via remote procedure call (RPC) or REST API interface.

You can install the ClusterControl UI independently on another server by using a helper script, ``install-cc-ui.sh`` available from the `Severalnines download page <http://www.severalnines.com/downloads/cmon/>`_. To install the UI:

.. code-block:: bash

	wget http://www.severalnines.com/downloads/cmon/install-cc-ui.sh 
	chmod u+x install-cc-ui.sh 
	sudo ./install-cc-ui.sh
	
.. Note:: Omit 'sudo' if you are running as root.

The ClusterControl UI can connect to multiple CMON Controller servers (if they have installed the CMONAPI) and provides a centralized view of the entire database infrastructure. Users just need to register the CMONAPI token and URL for a specific cluster on the Cluster Registrations page.

The ClusterControl UI will load the cluster in the database cluster list, similar to the screenshot below:

.. image:: img/docs_cc_ui.png
   :align: center

Similar to the CMONAPI, the ClusterControl UI is running on Apache and located under ``/var/www/html/clustercontrol`` (Redhat/CentOS/Ubuntu >14.04) or ``/var/www/clustercontrol`` (Debian/Ubuntu <14.04). The web server must support rule-based rewrite engine and must be able to follow symlinks. 

ClusterControl UI page can be accessed through following URL: 

**http or https://<ClusterControl IP address or hostname\>/clustercontrol**

Please refer to User Guide for MySQL for more details on the functionality available through the ClusterControl UI.