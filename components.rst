.. _components:
.. include:: <isotech.txt>

Components
==========

ClusterControl consists of 6 components:

+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| Component                          | Package naming               | Role                                                                               |
+====================================+==============================+====================================================================================+
| ClusterControl controller (cmon)   | clustercontrol-controller    | The brain of ClusterControl. A backend service performing automation, management,  |
|                                    |                              | monitoring and scheduling tasks. All the collected data will be stored directly    |
|                                    |                              | inside CMON database.                                                              |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl REST API [#f1]_     | clustercontrol-cmonapi       | Interprets request and response data between ClusterControl UI and CMON database.  |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl UI                  | clustercontrol               | A modern web user interface to visualize and manage the cluster. It interacts with | 
|                                    |                              | CMON controller via remote procedure call (RPC) or REST API interface.             |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl SSH                 | clustercontrol-ssh           | Optional package introduced in ClusterControl version 1.4.2 for ClusterControl's   |
|                                    |                              | web SSH console. Only works with Apache 2.4+.                                      |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl Notifications       | clustercontrol-notifications | Optional package introduced in ClusterControl version 1.4.2 to provide an          |
|                                    |                              | interface for notification services and integration with third party tools.        |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl CLI                 | s9s-tools                    | Open-source command line tool to manage and monitor clusters provisioned by        |
|                                    |                              | ClusterControl.                                                                    |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+

ClusterControl Controller (CMON)
--------------------------------

ClusterControl Controller (CMON) is the core backend process that performs all automation and management procedures. It is usually installed as ``/usr/sbin/cmon``. It comes with a collection of helper scripts in ``/usr/bin`` directory (prefixed with s9s\_) to complete specific tasks. However, some of the scripts have been deprecated due to the corresponding tasks are now being handled by the CMON core process.

CMON controller package is available at `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_. Redhat-based systems should download and install the RPM package while Debian-based systems should download and extract the DEB package. The package name is formatted as:

* RPM package (Redhat-based systems): ``clustercontrol-controller-[version]-[build number]-[architecture].rpm``
* DEB package (Debian-based systems): ``clustercontrol-controller-[version]-[build number]-[architecture].deb``

A configuration file ``/etc/cmon.cnf`` is required to initially setup the CMON Controller. It is possible to have several configuration files each for multiple clusters as described in the `Configuration File`_ section.

Command Line Arguments
``````````````````````

By default if you just run ``cmon`` (without any arguments), cmon defaults to run in the background. ClusterControl Controller (cmon) supports several command line options as shown below:

``-h, --help``

* Print the help.

``--help-config``

* Print the manual for configuration parameters. See `Configuration Options`_ for details.

``-v, --version``

* Prints out the version number and build info.

``--logfile=[filepath]``

The path of the log file to be used.

``-d, --nodaemon``

* Run in foreground. Ctrl + C to exit.

``-r, --directory=[directory]``

* Running directory.

``-p, --rpc-port=[integer]``

* Listen on RPC port. Default is 9500.

``-t, --rpc-tls=<bool>``

* Enable TLS on RPC port. Default is false.

``-b, --bind-addr='ip1,ip2..'``

* Bind Remote Procedure Call (RPC) to IP addresses (default is 127.0.0.1,::1). By default cmon binds to ‘127.0.0.1’ and ‘::1’. If another bind-address is needed, then it is possible to define the bind addresses in the file ``/etc/default/cmon``. The CMON init script translates those RPC\_* ones into command line options.

* Example

.. code-block:: bash

	RPC_PORT=9500
	RPC_BIND_ADDRESSES="10.10.10.13,192.168.33.1"

``-u, --upgrade-schema``

* Try to upgrade the CMON schema (Supported from CMON version 1.2.12 and later).

``-U, --cmondb-user=USERNAME``

* Sets the user name to access the CMON database.

``-P, --cmondb-password=PASSWORD``

* Uses the password to access teh CMON database.

``-H, --cmondb-host=HOSTNAME``

* Access the CMON database on the given host.

``-D, --cmondb-name=DATABASE``

* Sets the CMON database name.

``-e, --events-client=URL``

* Additional RPC URL where backend sends events.


Configuration File
``````````````````

A single CMON Controller process is able to monitor one or more clusters. Each of the cluster requires one exclusive configuration file residing in the ``/etc/cmon.d/`` directory. For instance, the default CMON configuration file is located at ``/etc/cmon.cnf``, and commonly used to store the default (minimal) configuration for CMON process to run. 

Example of the CMON main configuration file located at ``/etc/cmon.cnf``:

.. code-block:: bash

	mysql_port=3306
	mysql_hostname=127.0.0.1
	mysql_password=cm0nP4ss
	mysql_basedir=/usr
	hostname=10.0.0.196
	logfile=/var/log/cmon.log
	rpc_key=390faeffb8166277a4f25336a69efa50915635a7


For the first cluster (cluster_id=1), the configuration options should be stored inside ``/etc/cmon.d/cmon_1.cnf``. For the second cluster, it would be ``/etc/cmon.d/cmon_2.cnf`` with ``cluster_id=2`` respectively, and so on. The following shows example content of CMON cluster's configuration file located at ``/etc/cmon.d/cmon_1.cnf``:

.. code-block:: bash
	
	cluster_id=1
	cmon_user=cmon
	created_by_job=1
	db_stats_collection_interval=30
	enable_query_monitor=1
	galera_vendor=codership
	galera_version=3.x
	group_owner=1
	host_stats_collection_interval=60
	hostname=10.0.0.196
	logfile=/var/log/cmon_1.log
	mode=controller
	monitored_mountpoints=/var/lib/mysql/
	monitored_mysql_port=3306
	monitored_mysql_root_password=7XU@Wy4nqL9
	mysql_bindir=/usr/bin/
	mysql_hostname=127.0.0.1
	mysql_password=cm0nP4ss
	mysql_port=3306
	mysql_server_addresses=10.0.0.99:3306,10.0.0.253:3306,10.0.0.181:3306
	mysql_version=5.6
	name='Galera Cluster'
	os=redhat
	osuser=root
	owner=1
	pidfile=/var/run
	basedir=/usr
	repl_password=9hHRgQLSsZz3Vd4a
	repl_user=rpl_user
	rpc_key=3V0RaV6dE8KSyClE
	ssh_identity=/root/ashrafawskey.pem
	ssh_port=22
	type=galera
	vendor=codership

An example of CMON configuration file hierarchy is as follows:

+----------------------------+------------------------+--------------+-----------------------------+
| Example cluster            | Configuration file     | Cluster      | Log file location           |
|                            |                        | identifier   |                             |
+============================+========================+==============+=============================+
| Default configuration      | /etc/cmon.cnf          | N/A          | logfile=/var/log/cmon.log   |
+----------------------------+------------------------+--------------+-----------------------------+
| Cluster #1 (Galera)        | /etc/cmon.d/cmon_1.cnf | cluster_id=1 | logfile=/var/log/cmon_1.log |
+----------------------------+------------------------+--------------+-----------------------------+
| Cluster #2 (MySQL Cluster) | /etc/cmon.d/cmon_2.cnf | cluster_id=2 | logfile=/var/log/cmon_2.log |
+----------------------------+------------------------+--------------+-----------------------------+
| Cluster #N (cluster type)  | /etc/cmon.d/cmon_N.cnf | cluster_id=N | logfile=/var/log/cmon_N.log |
+----------------------------+------------------------+--------------+-----------------------------+
 
.. Note:: It's highly recommendeded to separate CMON logging for each cluster to its own log file. In the above example, we can see that ``cluster_id`` and ``logfile`` are two imporant configuration options to distinguish the cluster.

The CMON Controller will import the configuration options defined in each configuration file into the CMON database during process starts up. Once loaded, CMON then use all the loaded information to manage clusters based on the ``cluster_id`` value.

Configuration Options
`````````````````````

All of the options and values as described below must not contain any whitespace between them. Any changes to the CMON configuration file requires a CMON service restart before they are applied. The s

The configuration options can be divided into a number of types:

1. General
2. CMON
3. Operating system
4. SSH
5. Nodes (MySQL, MongoDB, PostgreSQL)
6. Monitoring
7. Management
8. Security & Encryption

Following is the list of common configuration options inside CMON Controller configuration file. You can also see them by using ``--help-config`` parameter in the terminal:

.. code-block:: bash

	$ cmon --help-config


General
'''''''

``cluster_id=<integer>``

* Cluster identifier. This will be used by CMON to indicate which cluster to provision. It must be unique, two clusters can not share the same ID.	
* Example: ``cluster_id=1``

``name=<string>``

* Cluster name. The cluster name configured under *ClusterControl > DB cluster > Settings > General Settings > Cluster Name* precedes this.
* Example: ``name=cluster_1``

``cluster_name=<string>``

* Alias to ``name``.

``type=<string>``

* Cluster type. Supported values are galera, mysql_single, mysqlcluster, mongodb, postgresql_single, replication, group_replication.
* Example: ``type=galera``

``cluster_type``

* Alias to ``type``.

``created_by_job=<integer>``

* The ID of the job created this cluster. This is usually automatically generated by ClusterControl.
* Example: ``created_by_job=13``

CMON
'''''

``mode=<string>``

* CMON role. Supported values are controller, dual, agent, hostonly.
* Example: ``mode=controller``

``agentless=<boolean integer>``

* CMON controller mode (deprecated). Agents are no longer supported. 0 for agentful or 1 for agentless (default). 
* Example: ``agentless=1``

``logfile=<path to log file>``

* CMON log file location. This is where CMON logs its activity. The file will be automatically generated if it doesn't exist. CMON will write to syslog by default. 
* Example: ``logfile=/var/log/cmon.log``

``pidfile=<path to PID directory>``

* CMON process identifier file directory. Keep the default value is recommended.	
* Example: ``pidfile=/var/run``

``mysql_hostname=<string>``

* The MySQL hostname or IP address where CMON database resides. Using IP address is recommended.	
* Example: ``mysql_hostname=192.168.0.10``

``mysql_password=<string>``

* The MySQL password for user cmon to connect to CMON database. Alphanumeric values only.
* Example: ``mysql_password=cMonP4ss``

``mysql_port=<integer>``

* The MySQL port used by CMON to connecto to CMON database.	
* Example: ``mysql_port=3306``


Operating system
''''''''''''''''

``os=<string>``

* Operating system runs across the cluster, including ClusterControl host. 'redhat' for Redhat-based distributions (CentOS/Red Hat Enterprise Linux/Oracle Linux) or 'debian' for Debian-based distributions (Debian/Ubuntu).
* Example: ``os=redhat``

``osuser=<string>``

* Operating system user that will be used by CMON to perform automation tasks like cluster recovery, backups and upgrades. This user must be able to perform super-user activities. Using root is recommended.
* Example: ``os_user=root``

``os_user=<string>``

* Alias to ``osuser``.

``sshuser=<string>``

* Alias to ``osuser``.

``sudo="echo '<sudo password>' | sudo -S 2>/dev/null"``

* The command used to obtain superuser permissions. If sudo user requires password, specify the sudo command with sudo password here. The sudo command must be trimmed by redirecting stderr to somewhere else. Therefore, it is compulsary to have ``-S 2>/dev/null`` appended in the sudo command.
* Example: ``sudo="echo 'My5ud0' | sudo -S 2>/dev/null"``

``sudo_opts=<command>``

* Alias to ``sudo``.

``hostname=<string>``

* Hostname or IP address of the ClusterControl Controller (cmon) host.
* Example: ``hostname=192.168.0.10``

``wwwroot=<path to CMONAPI and ClusterControl UI>``

* Path to CMONAPI and ClusterControl UI. If not set, it defaults to '/var/www/html' for Redhat-based distributions or '/var/www' for Debian-based distributions.
* Example: ``wwwroot=/var/www/html``

``vendor=<string>``

* Database vendor name. ClusterControl needs to know this in order to distinguish the vendor's relevant naming convention especially for package name, daemon name, deployment steps, recovery procedures and lots more. Supported value at the moment is percona, codership, mariadb, mongodb, oracle.
* Example: ``vendor=codership``


SSH
'''

``ssh_identify=<path to SSH key or key pair>``

* The SSH key or key pair file that will be used by CMON to connect managed nodes (including ClusterControl node) passwordlessly. If undefined, CMON will use the home directory of ``os_user`` and look for ``.ssh/id_rsa`` file.	
* Example: ``ssh_identity=/root/.ssh/id_rsa``

``ssh_keypath=<path to SSH key or key pair>``

* Alias to ``ssh_identify``.

``identity_file=<path to SSH key or key pair>``

* Alias to ``ssh_identify``.

``ssh_port=<integer>``

* The SSH port used by CMON to connect to managed nodes. If undefined, CMON will use port 22.	
* Example: ``ssh_port=22``

``ssh_options=<string>``

* The SSH options used by CMON to connect to managed nodes. Details on SSH manual page.	
* Example: ``ssh_options='-nqtt'``

``ssh_acquire_tty=<boolean integer>``

* Setting for libssh: should it acquire a remote tty. Default is 1 (true).
* Example: ``ssh_acquire_tty=1``

``ssh_password=<string>``

* The SSH password used for connection to nodes.
* Example: ``ssh_password=P4ssw0rd123``

``ssh_timeout=<integer>``

* Network timeout value in seconds for SSH connections. Default is 30.
* Example: ``ssh_timeout=30``

``libssh_timeout=<integer>``

* Alias to ``ssh_timeout``
     
``libssh_loglevel=<integer>``

* Setting for libssh logging verbosity to stdout. Accepted values are 0 (NONE), 1 (WARN), 2 (INFO), 3 (DEBUG), 4 (TRACE).
* Example: ``libssh_loglevel=2``

Monitoring
'''''''''''

``monitored_mountpoints=<list of paths to be monitored>``

* The MySQL/MongoDB/TokuMX/PostgreSQL data directory used by database nodes for disk performance in comma separated list.	
* Example: ``monitored_mountpoints=/var/lib/mysql,/mnt/data/mysql``

``monitored_nics=<list of NICs to be monitored>``

* List of network interface card (NIC) to be monitored for network performance in comma separated list.	
* Example: ``monitored_nics=eth1,eth2``

``db_stats_collection_interval=<integer>``

* Database statistic collections interval in seconds. The lowest value is 1. Default is 30 seconds.
* Example: ``db_stats_collection_interval=30``

``host_stats_collection_interval=<integer>``

* Host statistic collections interval in seconds. The lowest value is 1. Default is 30 seconds.
* Example: ``host_stats_collection_interval=30``

``lb_stats_collection_interval=<integer>``

* Load balancer stats collection interval. Default is 30.
* Example: ``lb_stats_collection_interval=30``

``db_schema_stats_collection_interval=<integer>``

* How often database growth and table checks are performed in seconds. This translates to information_schema queries. Default is 10800 seconds (3 hours). 0 means disabled.
* Example: ``db_schema_stats_collection_interval=10800``

``db_log_collection_interval=<integer>``

* Database log files collection interval. Default is 600.
* Example: ``db_log_collection_interval=600``

``db_long_query_time_alarm=<integer>``

* If a query takes longer than ``db_long_query_time_alarm`` to execute, an alarm will be raised containing detailed information about blocked and long running transactions. Default is 10 seconds.
* Example: ``db_long_query_time_alarm=5``

``db_schema_max_objects=<integer>``

* Maximum number of database objects that ClusterControl will pull from monitored database nodes.
* Example: ``db_schema_max_objects=500``

``db_hourly_stats_collection_interval=<integer>``

* Database statistic collections interval in seconds. Default is 5.
* Example: ``db_hourly_stats_collection_interval=5``
		 
``enable_mysql_timemachine=<boolean integer>``

* This determine whether ClusterControl should enable MySQL time machine status and variable collections. The status time machine allows you to select status variable for a time range and compare the values at the start and end of that range from ClusterControl UI. Default is 0, meaning it is disabled.
* Example: ``enable_mysql_timemachine=1``

``swap_warning=<integer>``

* Warning alarm threshold for swap usage. Default is 5. Also configurable at *ClusterControl > {cluster_id} > Settings > Thresholds*.
* Example: ``swap_warning=20``

``swap_critical=<integer>``

* Critical alarm threshold for swap usage. Default is 20. Also configurable at *ClusterControl > {cluster_id} > Settings > Thresholds*.
* Example: ``swap_critical=40``

``swap_inout_period=<integer>``

* The interval for swap I/O alarms in seconds. Default is 600 (10 minutes).
* Example: ``swap_inout_period=120``

``swap_inout_warning=<integer>``

* The number of pages swapped I/O for warning. Default is 10240. To determine the page size for the host, use ``getconf PAGESIZE``.
* Example: ``swap_inout_warning=51200``

``swap_inout_critical=<integer>``

* The number of pages swapped I/O for critical. Default is 102400. To determine the page size for the host, use ``getconf PAGESIZE``.
* Example: ``swap_inout_critical=102400``

Management
'''''''''''

``enable_cluster_autorecovery=<boolean integer>``

* If undefined, CMON defaults to 0 (false) and will NOT perform automatic recovery if it detects cluster failure. Supported value is 1 (cluster recovery is enabled) or 0 (cluster recovery is disabled).
* Example: ``enable_cluster_autorecovery=1``

``enable_node_autorecovery=<boolean integer>``

* If undefined, CMON default to 0 (false) and will NOT perform automatic recovery if it detects node failure. Supported value is 1 (node recovery is enabled) or 0 (node recovery is disabled).
* Example: ``enable_node_autorecovery=1``

``enable_autorecovery=<boolean integer>``

* If undefined, CMON defaults to 0 (false) and will NOT perform automatic recovery if it detects node or cluster failure. Supported value is 0 (cluster and node recovery are disabled) or 1 (cluster and node recovery are enabled). This setting will internally set ``enable_node_autorecovery`` and ``enable_cluster_autorecovery`` to the specified value.
* Example: ``enable_autorecovery=1``

``netcat_port=<integer>``

* The netcat port used to stream backups. Default is 9999.
* Example: ``netcat_port=9999``

Nodes (MySQL)
''''''''''''''

``mysql_server_addresses=<string>``

* Comma separated list of MySQL hostnames or IP addresses (with or without port is supported). For MySQL Cluster, this should be the list of MySQL API node IP addresses.	
* Example: ``mysql_server_addresses=192.168.0.11:3306,192.168.0.12:3306,192.168.0.13:3306``

``datanode_addresses=<string>``

* Exclusive for MySQL Cluster. Comma separated list of data node hostnames or IP addresses.
* Example: ``datanode_addresses=192.168.0.41,192.168.0.42``

``mgmnode_addresses=<string>``

* Exclusive for MySQL Cluster. Comma separated list of management node hostnames or IP addresses.
* Example: ``mgmnode_addresses=192.168.0.51,192.168.0.52``

``ndb_connectstring=<string>``

* Exclusive for MySQL Cluster. NDB connection string for the cluster.
* Example: ``ndb_connectstring=192.168.0.51:1186,192.168.0.52:1186``

``ndb_binary=<string>``

* Exclusive for MySQL Cluster. NDB binary for data node. Supported values are ndbd or ndbmtd.
* Example: ``ndb_binary=ndbmtd``

``db_configdir=<string>``

* Exclusive for MySQL Cluster. Directory where configuration files (my.cnf/config.ini) of the cluster is stored.
* Example: ``db_configdir=/etc/mysql``

``monitored_mysql_port=<integer>``

* MySQL port for the managed cluster. ClusterControl all DB nodes are running on the same MySQL port.
* Example: ``monitored_mysql_port=3306``

``monitored_mysql_root_password=<string>``

* MySQL root password for the managed cluster. ClusterControl assumes all DB nodes are using the same root password. This is required when you want to scale your cluster by adding a new DB node or replication slave.
* Example: ``monitored_mysql_root_password=r00tPassword``

``mysql_basedir=<MySQL base directory location>``

* The MySQL base directory used by CMON to find MySQL client related binaries.	
* Example: ``mysql_basedir=/usr``

``mysql_bindir=<MySQL binary directory location>``

* The MySQL binary directory used by CMON to find MySQL client related binaries.	
* Example: ``mysql_bindir=/usr/bin``

``repl_user=<string>``

* The MySQL replication user.
* Example: ``repl_user=repluser``

``repl_password=<string>``

* Password for ``repl_user``.
* Example: ``repl_password=ZG04Z2Jnk0MUWAZK``

``auto_manage_readonly=<boolean integer>``

* Enable/Disable automatic management of the MySQL server ``read_only`` variable. Default is 1 (true), which means ClusterControl will set the ``read_only=ON`` if the MySQL replication role is slave.
* Example: ``auto_manage_readonly=0``

``galera_port=<integer>``

* The galera port to be used. Default is 4567.
* Example: ``galera_port=5555``

``replication_failover_whitelist=<string>``

* Comma separated list of MySQL slaves which should be used as potential master candidates. If no server on the whitelist is available (up/connected) the failover will fail. If this variable is set, only those hosts will be considered. This parameter takes precedence over ``replication_failover_blacklist``.
* Example: ``replication_failover_whitelist=192.168.1.11,192.168.1.12``

``replication_failover_blacklist=<string>``

* Comma separated list of MySQL slaves which will never be considered a master candidate. You can use it to list slaves that are used for backups or analytical queries. If the hardware varies between slaves, you may want to put here the slaves which use slower hardware. ``replication_failover_whitelist`` takes precedence over this parameter if it is set.
* Example: ``replication_failover_blacklist=192.168.1.101,192.168.1.102``

``replication_skip_apply_missing_txs=<boolean integer>``

* Default is 0. Skip the check process for additional missing transactions before promoting a slave to a master and just use the most advanced slave. Such process may result in a serious problem though - if an errant transaction is found, replication may be broken.
* Example: ``replication_skip_apply_missing_txs=1``

``replication_stop_on_error=<boolean integer>``

* Default is 1. ClusterControl will perform the MySQL master switch only once and will be aborted immediately if the switchover fails, unless the controller is restarted or you specify this variable to 0.
* Example: ``replication_stop_on_error=0``

``replication_failover_wait_to_apply_timeout=<integer>``

* Candidate waits up to this many seconds to apply outstanding relay log (retrieved_gtids) before failing over. Default is -1, which means ClusterControl will wait indefinitely for it to apply all missing transactions from its relay logs. This is safe, but, if for some reason, the most up-to-date slave is lagging badly, failover may takes hours to complete. If set to 0, failover happens immediately, no matter if the master candidate is lagging or not. Default -1 seconds (wait forever). Value higher than 0 means ClusterControl will wait for the specified seconds before failover happens.
* Example: ``replication_failover_wait_to_apply_timeout=0``

``replication_stop_on_error=<boolean integer>``

* Failover/switchover procedures will fail if errors are encountered that may cause data loss. Enabled by default. 0 means disable, default is 1 (true).
* Example: ``replication_stop_on_error=0``

``replication_auto_rebuild_slave=<boolean integer>``

* If the SQL THREAD is stopped and error code is non-zero then the slave will be automatically rebuilt. 1 means enable, default is 0 (false).
* Example: ``replication_auto_rebuild_slave=1``

``replication_onfail_failover_script=<path to the script on ClusterControl node>``

* This script executes as soon as it has been discovered that a failover is needed. If the script returns non-zero it will force the failover to abort. If the script is defined but not found, the failover will be aborted. Four arguments are supplied to the script: arg1='all servers'  arg2='oldmaster' arg3='candidate', arg4='slaves of oldmaster' and passed like this: ``scriptname arg1 arg2 arg3 arg4``.
* The script must be accessible on the controller and executable.
* Example: ``replication_onfail_failover_script=/usr/local/bin/failover_script.sh``

``replication_pre_failover_script=<path to the script on ClusterControl node>``

* This script executes before the failover happens, but after a candidate has been elected and it is possible to continue the failover process. If the script returns non-zero it will force the failover to abort. If the script is defined but not found, the failover will be aborted. The script must be accessible on the controller and executable.
* Example: ``replication_pre_failover_script=/usr/local/bin/pre_failover_script.sh``

``replication_post_failover_script=<path to the script on ClusterControl node>``

* This script executes after the failover happened. If the script returns non-zero a Warning will be written in the job log. The script must be accessible on the controller and executable.
* Example: ``replication_post_failover_script=/usr/local/bin/post_failover_script.sh``

``replication_post_unsuccessful_failover_script=<path to the script on ClusterControl node>``

* This script is executed after the failover attempt failed. If the script returns non-zero a Warning will be written in the job log. The script must be accessible on the controller and executable.
* Example: ``replication_post_unsuccessful_failover_script=post_fail_failover_script.sh``

``replication_failed_reslave_failover_script=<path to the script on ClusterControl node>``

* This script is executed after that a new master has been promoted and if the reslaving of the slaves to the new master fails. If the script returns non-zero a Warning will be written in the job log. The script must be accessible on the controller and executable.
* Example: ``replication_failed_reslave_failover_script=/usr/local/bin/fail_reslave_failover_script.sh``

``replication_pre_switchover_script=<path to the script on ClusterControl node>``

* This script executes before the switchover happens. If the script returns non-zero it will force the switchover to fail. If the script is defined but not found, the switchover will be aborted. The script must be accessible on the controller and executable.
* Example: ``replication_pre_switchover_script=/usr/local/bin/pre_switchover_failover_script.sh``

``replication_post_switchover_script=<path to the script on ClusterControl node>``

* This script executes after the switchover happened. If the script returns non-zero a Warning will be written in the job log. The script must be accessible on the controller and executable.
* Example: ``replication_post_switchover_script=/usr/local/bin/post_switchover_failover_script.sh``

``replication_check_external_bf_failover=<boolean integer>``

* Before attempting a failover, perform extended checks by checking the slave status to detect if the master is truly down, and also check if ProxySQL (if installed) can still see the master. If the master is detected to be functioning, then no failover will be performed. Default is 0 (false) meaning the checks are disabled. 1 means enable.
* Example: ``replication_check_external_bf_failover=0``

``replication_check_binlog_filtration_bf_failover=<boolean integer>``

* Before attempting a failover, verify filration (binlog_do/ignore_db) and replication_* are identically configured on the candidate and the slaves. Default is 0 (false) meaning the checks are disabled. 1 means enable.
* Example: ``replication_check_binlog_filtration_bf_failover=1``

``schema_change_detection_address=<string>``

* This option must be defined to use "Operational Report for Schema Change". Creating a report of thousands of database objects (schemas, tables etc) will take some time (about 5-10 minutes) depending on the hardware. It's recommended to configure a specific host to run this job for example on a replication slave or an asynchronous slave connected to e.g a Galera or Group Replication Cluster. For NDB this option should be set to a MySQL server used for admin purposes.
* Example: ``schema_change_detection_address=192.168.111.53``

``schema_change_detection_pause_time_ms=<integer>``

* Throttle the detection process by pausing every this value, in miliseconds. For example, if defined as 3000, ClusterControl will pause the operation for every 3 seconds.
* Example: ``schema_change_detection_pause_time_ms=3000``

``schema_change_detection_databases=<string>``

* Comma separated string of database names and also supports wildcards. For example 'DB%', will evaluate all database starting with "DB".
* Example: ``schema_change_detection_databases=mydb%,shops_db,mymonitoring``

Nodes (MongoDB)
'''''''''''''''

``mongodb_server_addresses=<string>``

* Comma separated list of MongoDB/TokuMX shard or replica IP addresses with port.
* Example: ``mongodb_server_addresses=192.168.0.11:27017,192.168.0.12:27017,192.168.0.13:27017``

``mongoarbiter_server_addresses=<string>``

* Comma separated list of MongoDB/TokuMX arbiter IP addresses with port.	
* Example: `mongoarbiter_server_addresses=192.168.0.11:27019,192.168.0.12:27019,192.168.0.13:27019`

``mongocfg_server_addresses=<string>``

* Comma separated list of MongoDB/TokuMX config server IP addresses with port.	
* Example: ``mongocfg_server_addresses=192.168.0.11:27019,192.168.0.12:27019,192.168.0.13:27019``

``mongos_server_addresses=<string>``

* Comma separated list of MongoDB/TokuMX mongos IP addresses with port.
* Example: ``mongos_server_addresses=192.168.0.11:27017,192.168.0.12:27017,192.168.0.13:27017``

``mongodb_basedir=<location MongoDB base directory>``

* The MongoDB base directory used by CMON to find mongodb client related binaries.	
* Example: ``mongodb_basedir=/usr``

``mongodb_user=<string>``

* MongoDB admin/root username.
* Example: ``mongodb_user=root``

``mongodb_password=myadminpassword``

* Password for ``mongodb_user``.
* Example: ``mongodb_password=kPo123^^#*``

``mongodb_authdb=<string>``

* The database containing user information to use for authentication. Default is admin.
* Example: ``mongodb_authdb=admin``

``mongodb_cluster_key=<path>``
 
* The cluster's nodes authenticating to each other using this key.
* Example: ``mongodb_cluster_key=/etc/repl.key``

Nodes (PostgreSQL)
''''''''''''''''''

``postgresql_server_addresses=<string>``

* The PostgreSQL node instances.
* Example: ``postgresql_server_addresses=192.168.10.100``

``postgre_server_addresses=<string>``

* Alias to ``postgresql_server_addresses``.

``postgresql_user=<string>``

* The PostgreSQL admin user name. Default is postgres.
* Example: ``postgresql_user=postgres``

``postgre_user=<string>``

* Alias to ``postgresql_user``.

``postgresql_password=<string>``

* The password used for PostgreSQL user.
* Example: ``postgresql_password=p4ssw0rd123``

``postgre_password=<string>``

* Alias to ``postgresql_password``.


Encryption and Security
''''''''''''''''''''''''

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

``rpc_key=<string>``

* Unique secret token for authentication. To interact with individual cluster via CMON RPC interface (port 9500), one must use this key or else you would get 'HTTP/1.1 403 Access denied'.
* `ClusterControl UI`_ needs this key stored as RPC API Token to communicate with CMON RPC interface. Each cluster should be configured with different ``rpc_key`` value. This value is automatically generated when new cluster/server is created or added into ClusterControl.
* Example: ``rpc_key=VJZKhr5CvEGI32dP``

Agentless
`````````

Starting from version 1.2.5, ClusterControl introduces an agentless mode of operation. There is no need to install agents on the managed nodes. Users only need to install the CMON controller package on the ClusterControl host, and make sure that passwordless SSH and the CMON database user GRANTs are properly set up on each of the managed hosts.

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

	mysql -f -ucmon -p{cmon_password} -h{mysql_hostname} -P{mysql_port} cmon < /usr/share/cmon/cmon_db.sql
	mysql -f -ucmon -p{cmon_password} -h{mysql_hostname} -P{mysql_port} cmon < /usr/share/cmon/cmon_data.sql

MySQL user 'cmon' needs to have proper access to CMON DB by performing following grant:

Grant all privileges to 'cmon' at ``hostname`` value (as defined in CMON configuration file) on ClusterControl host: 

.. code-block:: mysql

	GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'{hostname}' IDENTIFIED BY '{mysql_password}' WITH GRANT OPTION;

Grant all privileges for 'cmon' at 127.0.0.1 on ClusterControl host:

.. code-block:: mysql

	GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'127.0.0.1' IDENTIFIED BY '{mysql_password}' WITH GRANT OPTION;

For each managed database server, on the managed database server, grant all privileges to cmon at controller's ``hostname`` value (as defined in CMON configuration file) on each of the managed database host:

.. code-block:: mysql

	GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'{hostname}' IDENTIFIED BY '{mysql_password}' WITH GRANT OPTION;

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

You can generate the CMONAPI token manually by using following command:

.. code-block:: bash

	python -c 'import uuid; print uuid.uuid4()' | sha1sum | cut -f1 -d' '

By default, the CMONAPI is running on Apache and located under ``/var/www/html/cmonapi`` (Redhat/CentOS/Ubuntu >14.04) or ``/var/www/cmonapi`` (Debian/Ubuntu <14.04). The value is relative to ``wwwroot`` value defined in CMON configuration file. The web server must support rule-based rewrite engine and able to follow symlinks.

The CMONAPI page can be accessed through following URL:

:samp:`https://{ClusterControl IP address or hostname}/cmonapi`

Both ClusterControl CMONAPI and UI must be running on the same version to avoid misinterpretation of request and response data. For instance, ClusterControl UI version 1.2.6 needs to connect to the CMONAPI version 1.2.6.

.. Attention:: We are gradually in the process of migrating all functionalities in REST API to RPC interface. Kindly expect the REST API to be obselete in the near future.

ClusterControl UI
-----------------

ClusterControl UI provides a modern web user interface to visualize the cluster and perform tasks like backup scheduling, configuration changes, adding nodes, rolling upgrades, etc. It requires a MySQL database called 'dcps', to store cluster information, users, roles and settings. It interacts with CMON controller via remote procedure call (RPC) or REST API interface.

You can install the ClusterControl UI independently on another server by running following command:

.. code-block:: bash

	yum install clustercontrol # RHEL/CentOS
	sudo apt-get install clustercontrol # Debian/Ubuntu
	
.. Note:: Omit 'sudo' if you are running as root.

The ClusterControl UI can connect to multiple CMON Controller servers (if they have installed the CMONAPI) and provides a centralized view of the entire database infrastructure. Users just need to register the CMONAPI token and URL for a specific cluster on the Cluster Registrations page.

The ClusterControl UI will load the cluster in the database cluster list, similar to the screenshot below:

.. image:: img/docs_cc_ui.png
   :align: center

Similar to the CMONAPI, the ClusterControl UI is running on Apache and located under ``/var/www/html/clustercontrol`` (Redhat/CentOS/Ubuntu >14.04) or ``/var/www/clustercontrol`` (Debian <8/Ubuntu <14.04). The web server must support rule-based rewrite engine and must be able to follow symlinks. 

ClusterControl UI page can be accessed through following URL: 

:samp:`https://[ClusterControl IP address or hostname]/clustercontrol`

Please refer to `User Guide <user-guide/index.html>`_ for more details on the functionality available in the ClusterControl UI.

ClusterControl SSH
-------------------

This optional package is introduced in ClusterControl v1.4.2. This module provides the ability to connect to SSH console to any of your cluster hosts directly via ClusterControl UI. This can be very useful if you need to quickly log into a database server and access the command line. The package installs a binary called ``cmon-ssh`` located under ``/usr/sbin`` directory and by default listens to port 9511 on the ClusterControl node. It interacts directly with the target host via SSH protocol using the credential (``os_user`` and ``ssh_identity``) configured when deploying or importing the cluster into ClusterControl.

The SSH module need to be enabled in order to use the feature. If the package is installed directly via package manager, the required steps are configured automatically. The steps are:

1) Enable the SSH module inside ``clustercontrol/bootstrap.php`` file:

.. code-block:: php

	define('SSH_ENABLED', true);

2) Set up the RewriteRule inside Apache configuration file (above the ``<Directory/>`` definitions):

.. code-block:: bash

	# ClusterControl SSH
	RewriteEngine On
	RewriteRule ^/clustercontrol/ssh/term$ /clustercontrol/ssh/term/ [R=301]
	RewriteRule ^/clustercontrol/ssh/term/ws/(.*)$ ws://127.0.0.1:9511/ws/$1 [P,L]
	RewriteRule ^/clustercontrol/ssh/term/(.*)$ http://127.0.0.1:9511/$1 [P]

3) Enable the following Apache modules:

.. code-block:: bash

	a2enmod proxy proxy_http proxy_wstunnel

Communication is based on HTTPS, so it is possible to access your servers from behind a firewall that restricts Internet access to only port 443. Access to WebSSH is configurable by the ClusterControl admin through the GUI.

.. Warning:: ClusterControl does not provide extra layers of authentication and authorization when accessing the cluster from web-based SSH terminal. User who has access to the cluster in the ClusterControl UI may capable of accessing the terminal as a privileged user. Use `Access Control <user-guide/index.html#access-control>`_ to limit them.

ClusterControl Notifications
----------------------------

This optional package is introduced in ClusterControl v1.4.2, deprecating the previous version of ClusterControl NodeJS package (which served the same purpose). Alarms and events can now easily be sent to incident management services like PagerDuty, VictorOps and OpsGenie. You can also run any command available in the `ClusterControl CLI`_ from your CCBot-enabled chat services like Slack and Telegram. Additionally, it provides a generic webhook if you want to integrate with other services to act on status changes in your clusters. The direct connections with these popular incident communication services allow you to customize how you are alerted from ClusterControl when something goes wrong with your database environments.

The package installs a binary called ``cmon-events`` located under ``/usr/sbin`` directory and by default listens to port 9510 on the ClusterControl node.

Additional resources on setting up integration with third-party tools are listed below:
	* Blogs:
		* `Introducing the ClusterControl Alerting Integrations <https://severalnines.com/blog/introducing-clustercontrol-alerting-integrations>`_
		* `Video: ClusterControl ChatOps Integrations - Product Demonstration <https://severalnines.com/blog/video-clustercontrol-chatops-integrations-product-demonstration>`_
	* Documentation:
		* `Integrations <user-guide/index.html#integrations>`_


ClusterControl CLI
------------------

Also known as **s9s-tools**, this optional package is introduced in ClusterControl version 1.4.1, which contains a binary called `s9s`. It is a command line tool to interact, control and manage database clusters using the ClusterControl Database Platform. Starting from version 1.4.1, the installer script will automatically install this package on the ClusterControl node. You can also install it on another computer or workstation to manage the database cluster remotely. All communication is encrypted and secure through SSH. The s9s command line project is open source and is located on `GitHub <https://github.com/severalnines/s9s-tools>`_.

ClusterControl CLI opens a new door for cluster automation where you can easily integrate it with existing deployment automation tools like Ansible, Puppet, Chef or Salt. You can deploy, manage and monitor your cluster without having to use the ClusterControl UI. The following list shows what features are available at the moment:

* Deploy and manage database clusters:
	- MySQL
	- PostgreSQL
	- MongoDB to be added soon.
* Basic monitoring features:
	- Status of nodes and clusters.
	- Cluster properties can be extracted.
	- Gives detailed enough information about your clusters.
* Management features:
	- Create clusters (Can’t add existing cluster).
	- Stop or start clusters.
	- Add or remove nodes.
	- Restart nodes in the cluster.
	- Create database users (CREATE USER, GRANT privileges to user).
	- Create load balancers (HAProxy and ProxySQL are supported)
	- Create and restore backups.
	- Maintenance mode.
	- Configuration changes of db nodes.

The command line tool is invoked by executing a binary called ``s9s``. The commands are basically JSON messages being sent over to the ClusterControl Controller (CMON) RPC interface. Communication between the s9s (the command line tool) and the cmon process (ClusterControl Controller) is encrypted using TLS and requires the port 9501 to be opened on controller and the client host.

Installation
`````````````

Package Manager (yum/apt)
'''''''''''''''''''''''''

The package list is available at `s9s-tools repository page <http://repo.severalnines.com/s9s-tools/>`_. 

RHEL/CentOS
...........

The repository files for each distribution can be downloaded directly from here:

* CentOS 6: http://repo.severalnines.com/s9s-tools/CentOS_6/s9s-tools.repo
* CentOS 7: http://repo.severalnines.com/s9s-tools/CentOS_7/s9s-tools.repo
* RHEL 6: http://repo.severalnines.com/s9s-tools/RHEL_6/s9s-tools.repo
* RHEL 7: http://repo.severalnines.com/s9s-tools/RHEL_7/s9s-tools.repo

Installation steps are straight-forward:

.. code-block:: bash
	
	# CentOS 7
	$ cd /etc/yum.repos.d
	$ wget http://repo.severalnines.com/s9s-tools/CentOS_7/s9s-tools.repo
	$ yum install s9s-tools
	$ s9s --help

Ubuntu DEB repository (via Launchpad)
.....................................

s9s-tools package is also hosted on `Launchpad <https://launchpad.net/~severalnines/+archive/ubuntu/s9s-tools>`_. You can use the PPA to your system's Software Sources:

.. code-block:: bash

	$ sudo add-apt-repository ppa:severalnines/s9s-tools
	$ sudo apt-get update
	$ sudo apt-get install s9s-tools
	$ s9s --help

Debian (and Ubuntu) DEB repositories
.....................................

The repository files for each distribution can be downloaded directly from here:

* Debian 7 (Wheezy): http://repo.severalnines.com/s9s-tools/wheezy/
* Debian 8 (Jessie): http://repo.severalnines.com/s9s-tools/jessie/
* Ubuntu 12.04 (Precise): http://repo.severalnines.com/s9s-tools/precise/
* Ubuntu 14.04 (Trusty): http://repo.severalnines.com/s9s-tools/trusty/
* Ubuntu 16.04 (Xenial): http://repo.severalnines.com/s9s-tools/xenial/
* Ubuntu 16.10 (Yakkety): http://repo.severalnines.com/s9s-tools/yakkety/
* Ubuntu 17.04 (Zesty): http://repo.severalnines.com/s9s-tools/zesty/

To install, one would do:

.. code-block:: bash

	# Available distros: wheezy, jessie, precise, trusty, xenial, yakkety, zesty
	$ DISTRO=jessie
	$ wget -qO - http://repo.severalnines.com/s9s-tools/${DISTRO}/Release.key | sudo apt-key add -
	$ echo "deb http://repo.severalnines.com/s9s-tools/${DISTRO}/ ./" | sudo tee /etc/apt/sources.list.d/s9s-tools.list
	$ sudo apt-get update
	$ sudo apt-get install s9s-tools
	$ s9s --help


Build From Source
''''''''''''''''''

To build from source may require additional packages and tools to be installed:

1. Get the source code from Github:

.. code-block:: bash

	$ git clone https://github.com/severalnines/s9s-tools.git

2. Navigate to the source code directory:

.. code-block:: bash

	$ cd s9s-tools

3. You may need to install packages such as C/C++ compiler, autotools, openssl-devel etc:

.. code-block:: bash

	# RHEL/CentOS
	$ yum groupinstall "Development Tools"
	$ yum install automake git openssl-devel
	
	# Ubuntu/Debian
	$ sudo apt-get install build-essential automake git libssl-dev byacc flex

4. Compile the source code:

.. code-block:: bash

	$ ./autogen.sh
	$ ./configure
	$ make
	$ make install
	$ s9s --help

It is possible to build the s9s command line client on Linux and Mac OS/X.

Configuration
``````````````

The first thing that must be done is to create a user that is allowed to connect to and use the controller. Communication between the s9s command line client and the controller (the cmon process) is encrypted using TLS on port 9501. A public and private RSA key pair associated with a username is used to encrypt the communication. The s9s command line client is responsible to setup the user and the required private and public key.

The command line client can be located on the same server as the controller (localhost communication) or on a remote server. The configuration differs depending on the location - localhost or remote access, and both cases are covered below.

Localhost Access
'''''''''''''''''

SSH into the controller and then let us create a user called 'dba' that is allowed to access the controller. This will create the first user. 

.. Important:: All users have the rights to perform all operations on the managed clusters. There is no access control or roles implemented at the moment. However, the user must be an authenticated user to be able to access the controller.

.. code-block:: bash

	$ s9s user --create --generate-key --controller="https://localhost:9501" --cmon-user=dba
	Grant user 'dba' succeeded.

.. Note:: Short options exist.

If this is the first time you use the s9s client, then a new directory has been created in ``$HOME/.s9s/`` storing the private/public key and a configuration file.

Let us see what has been created:

.. code-block:: bash

	$ ls $HOME/.s9s/
	dba.key  dba.pub  s9s.conf

Viewing the configuration file we will see:

.. code-block:: bash

	[global]
	cmon_user              = dba
	# controller_host_name = localhost
	# controller_port      = 9500
	# rpc_tls              = false

Now we need to set the ``controller_host_name`` and ``controller_port``, and the ``rpc_tls`` so the file looks:

.. code-block:: bash

	[global]
	cmon_user            = dba
	controller_host_name = localhost
	controller_port      = 9501
	rpc_tls              = true
	# controller = "https://localhost:9501"

In order to verify it is working you can list the available clusters:

.. code-block:: bash

	$ s9s cluster --list
	cluster_1 cluster_2 cluster_3

If the authentication fails you will see messages like:

.. code-block:: bash

	Authentication failed: User 'dba' is not found.

The above means that the user has not been created. Or if there is a problem connecting to cmon:

.. code-block:: bash

	Authentication failed: Connect to localhost:9501 failed: "{{ Failure message }}".

In this case, double check the ``~/.s9s/s9s.conf`` file and check that cmon is started with TLS:

.. code-block:: bash

	$ sudo grep -i tls /var/log/cmon.log
	2016-11-28 15:00:31 : (INFO) Server started at tls://127.0.0.1:9501

And also:

.. code-block:: bash

	$ sudo netstat -atp | grep 9501
	tcp        0      0 localhost:9501          *:*                     LISTEN      22096/cmon

To view the users, and list which is the currently used user (marked with an "A" - a short form of "Authenticated"):

.. code-block:: bash

	$ s9s user --list --long
	A ID UNAME      GNAME  EMAIL REALNAME
	-  1 system     admins -     System User
	-  2 nobody     nobody -     -
	A  3 dba        users  -     -
	-  4 remote_dba users  -     -

The 'nobody' user is a legacy user. No one should ever see a job issued by the user 'nobody'. The 'system' user is the ClusterControl server itself creating internal jobs (e.g internal cron jobs).

Remote Access
''''''''''''''

The steps to setup the s9s command line client for remote access is similar as for localhost, except:

* The s9s command line client must exist on the remote server
* The controller (cmon) must be accepting TLS connections from the remote server.
* The remote server can connect to the controller with key-based authentication (no password). This is required only during the user creation private/public key setup.

1. Setup the bind address from cmon process as follow:

.. code-block:: bash

	$ vi /etc/init.d/cmon

Locate the line:

.. code-block:: bash

	RPC_BIND_ADDRESSES=""

And change to:

.. code-block:: bash

	RPC_BIND_ADDRESSES="127.0.0.1,10.0.1.12"

Here we assume the public IP address of the controller is 10.0.1.12. 

.. Attention:: Naturally, you should lock down this IP with firewall rules only allowing access from the remote servers you wish to access the controller from.

2. Restart the controller and check the log:

.. code-block:: bash

	$ service cmon restart # sysvinit
	$ systemctl restart cmon # systemd

3. Verify the ClusterControl Controller is listening to the configured IP address on port 9501:

.. code-block:: bash

	$ cat /var/log/cmon.log | grep -i tls
	2016-11-29 12:34:04 : (INFO) Server started at tls://127.0.0.1:9501
	2016-11-29 12:34:04 : (INFO) Server started at tls://10.0.1.12:9501

4. On the remote server/computer, we have to enable key-based authentication and create a user called 'remote_dba'. Create a system user called 'remote_dba':

.. code-block:: bash

	$ useradd -m remote_dba

5. As the current user (root or sudoer for example) in the remote server, setup a passwordless SSH to the ClusterControl host. Generate one SSH key if you don't have one:

.. code-block:: bash

	$ ssh-keygen -t rsa # press 'Enter' for all prompts

Copy the SSH public key to the ClusterControl Controller host, for example 10.0.1.12:

.. code-block:: bash

	$ ssh-copy-id root@10.0.1.12

6. Create the s9s client user:

.. code-block:: bash

	$ s9s user --generate-key --create --cmon-user=remote_dba --controller="https://10.0.1.12:9501"
	Warning: Permanently added '10.0.1.12' (ECDSA) to the list of known hosts.
	Connection to 10.0.1.12 closed.
	Grant user 'remote_dba' succeeded.

7. Ensure the config file located at ``~/.s9s/s9s.conf`` looks like this (take note the IP of the controller may be different):

.. code-block:: bash

	[global]
	cmon_user            = remote_dba
	controller_host_name = 10.0.1.12
	controller_port      = 9501
	rpc_tls              = true

8. Finally, test the connection:

.. code-block:: bash

	$ s9s cluster --list
	cluster_1 cluster_2 cluster_3

Usage 
``````

The command line client installs manual pages and can be viewed by entering the command:

.. code-block:: bash

	$ man s9s-{command group}

For example:

.. code-block:: bash

	$ man s9s
	$ man s9s-cluster
	$ man s9s-user
	$ man s9s-job
	$ man s9s-backup
	$ man s9s-node
	$ man s9s-maintenance
	$ man s9s-process
	$ man s9s-conf

The general synopsis to execute commands using s9s is:

.. code-block:: bash

	s9s {command group} {options}

s9s cluster
'''''''''''

Create, list and manipulate clusters.

**Usage**

.. code-block:: bash

	s9s cluster {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ list                 List the clusters.
|minus|\ |minus|\ stat                 Print the details of a cluster.
|minus|\ |minus|\ create               Create and install a new cluster.
|minus|\ |minus|\ ping                 Check the connection to the controller.
|minus|\ |minus|\ rolling-restart      Restart the nodes without stopping the cluster.
|minus|\ |minus|\ add-node             Add a new node to the cluster.
|minus|\ |minus|\ remove-node          Remove a node from the cluster.
|minus|\ |minus|\ drop                 Drop cluster from the controller.
|minus|\ |minus|\ stop                 Stop the cluster.
|minus|\ |minus|\ start                Start the cluster.
|minus|\ |minus|\ create-account       Create a user account on the cluster.
|minus|\ |minus|\ delete-account       Delete a user account on the cluster.
|minus|\ |minus|\ create-database      Create a database on the cluster.
====================================== ===========

**Options**

=============================================== ===========
Name, shorthand                                 Description
=============================================== ===========
|minus|\ |minus|\ cluster-id=ID                 The ID of the cluster to manipulate.
|minus|\ |minus|\ cluster-name=NAME             Name of the cluster to manipulate or create.
|minus|\ |minus|\ nodes=NODE_LIST               List of nodes to work with.
|minus|\ |minus|\ vendor=VENDOR                 The name of the software vendor.
|minus|\ |minus|\ provider-version=VER          The version of the software.
|minus|\ |minus|\ os-user=USERNAME              The name of the user for the SSH commands.
|minus|\ |minus|\ cluster-type=TYPE             The type of the cluster to install.
|minus|\ |minus|\ db-admin=USERNAME             The database admin user name.
|minus|\ |minus|\ db-admin-passwd=PASSWD        The pasword for the database admin.
|minus|\ |minus|\ account=NAME[:PASSWD][@HOST]  Account to be created on the cluster.
|minus|\ |minus|\ with-database                 Create a database for the user too.
|minus|\ |minus|\ db-name=NAME                  The name of the database.
|minus|\ |minus|\ opt-group=NAME                The option group for configuration.
|minus|\ |minus|\ opt-name=NAME                 The name of the configuration item.
|minus|\ |minus|\ opt-value=VALUE               The value for the configuration item.
|minus|\ |minus|\ wait                          Wait until the job ends.
|minus|\ |minus|\ long, -l                      Print the detailed list.
=============================================== ===========

**Examples**

Create a three-node Percona XtraDB Cluster 5.7 cluster, with OS user vagrant:

.. code-block:: bash

	$ s9s cluster --create --cluster-type=galera --nodes="10.10.10.10;10.10.10.11;10.10.10.12"  --vendor=percona --provider-version=5.7 --db-admin-passwd='pa$$word' --os-user=vagrant --cluster-name='galeracluster01'

List all clusters with more details:

.. code-block:: bash

	$ s9s cluster --list --long

Delete a cluster with cluster ID 1:

.. code-block:: bash

	$ s9s cluster --delete --cluster-id=1
	
Add a new database node on Cluster ID 1:

.. code-block:: bash

	$ s9s cluster --add-node --nodes=10.10.10.14 --cluster-id=1 --wait

Create an HAproxy load balancer, 192.168.55.198 on cluster ID 1:

.. code-block:: bash

	s9s cluster --add-node --cluster-id=1 --nodes="haproxy://192.168.55.198" --wait

Remove a database node from cluster ID 1 as a background job:

.. code-block:: bash

	$ s9s cluster --remove-node --nodes=10.10.10.13 --cluster-id=1

s9s node
'''''''''''

View and handle nodes.

**Usage**

.. code-block:: bash

	s9s node {command} {options}

**Command**

====================================== ===================
Name, shorthand                        Description
====================================== ===================
|minus|\ |minus|\ list                 List the jobs found on the controller.
|minus|\ |minus|\ stat                 Print detailed node information.
|minus|\ |minus|\ set                  Change the properties of a node.
|minus|\ |minus|\ list-config          Print the configuration for a node.
|minus|\ |minus|\ change-config        Change the configuration for a node.
|minus|\ |minus|\ pull-config          Copy configuration files from a node.
|minus|\ |minus|\ push-config          Copy configuration files to a node.
====================================== ===================

**Options**

========================================== ===========
Name, shorthand                            Description
========================================== ===========
|minus|\ |minus|\ cluster-id=ID            The ID of the cluster in which the node is.
|minus|\ |minus|\ cluster-name=NAME        Name of the cluster to list.
|minus|\ |minus|\ nodes=NODE_LIST          The nodes to list or manipulate.
|minus|\ |minus|\ properties=ASSIGNMENTS   Names and values of the properties to change.
|minus|\ |minus|\ opt-group=GROUP          The configuration option group.
|minus|\ |minus|\ opt-name=NAME            The name of the configuration option.
|minus|\ |minus|\ opt-value=VALUE          The value of the configuration option.
|minus|\ |minus|\ output-dir=DIR           The directory where the files are created.
|minus|\ |minus|\ force                    Force to execute dangerous operations.
========================================== ===========

**Examples**

List all nodes:

.. code-block:: bash

	$ s9s node --list --long
	ST  VERSION                  CID CLUSTER        HOST       PORT COMMENT
	go- 10.1.22-MariaDB-1~xenial   1 MariaDB Galera 10.0.0.185 3306 Up and running
	co- 1.4.1.1856                 1 MariaDB Galera 10.0.0.205 9500 Up and running
	go- 10.1.22-MariaDB-1~xenial   1 MariaDB Galera 10.0.0.209 3306 Up and running
	go- 10.1.22-MariaDB-1~xenial   1 MariaDB Galera 10.0.0.82  3306 Up and running
	Total: 4
	
Print the configuration for a node:

.. code-block:: bash

	$ s9s node --list-config --nodes=10.0.0.3
	...
	mysqldump   max_allowed_packet                     512M
	mysqldump   user                                   backupuser
	mysqldump   password                               nWC6NSm7PnnF8zQ9
	xtrabackup  user                                   backupuser
	xtrabackup  password                               nWC6NSm7PnnF8zQ9
	MYSQLD_SAFE pid-file                               /var/lib/mysql/mysql.pid
	MYSQLD_SAFE basedir                                /usr/
	Total: 71

Push a configuration option inside my.cnf (max_connections=500) on node 10.0.0.3:

.. code-block:: bash

	$ s9s node --change-config --nodes=10.0.0.3 --opt-group=mysqld --opt-name=max_connections --opt-value=500

s9s backup
'''''''''''

View and create database backups. Three backup methods are supported:
	* mysqldump
	* xtrabackup (full)
	* xtrabackup (incremental)

The s9s client also needs to know:
	* The cluster id of the cluster to backup.
	* The node to backup.
	* The databases that should be included.
	
By default, the backups will be stored on the controller node. If you wish to store the backup on the data node, you can set the flag ``--on-node``.
	
.. Note:: If you are using Percona Xtrabackup, an incremental backup requires that there is already a full backup made of the same databases (all or individually specified). Otherwise, the incremental backup will be upgraded to a full backup.

**Usage**

.. code-block:: bash

	s9s backup {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ list                 List the backups.
|minus|\ |minus|\ create               Create a new backup.
|minus|\ |minus|\ restore              Restore an existing backup.
|minus|\ |minus|\ delete               Delete a previously created backup.
====================================== ===========

**Options**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ cluster-id=ID        The ID of the cluster.
|minus|\ |minus|\ backup-id=ID         The ID of the backup.
|minus|\ |minus|\ nodes=NODELIST       The list of nodes involved in the backup.
|minus|\ |minus|\ databases=LIST       Comma separated list of databases to archive.
|minus|\ |minus|\ backup-method=METHOD Defines the backup program to be used.
|minus|\ |minus|\ backup-directory=DIR The directory where the backup is placed.
|minus|\ |minus|\ parallellism=N       Number of threads used while creating backup.
|minus|\ |minus|\ no-compression       Do not compress the archive file.
|minus|\ |minus|\ use-pigz             Use the pigz program to compress archive.
|minus|\ |minus|\ on-node              Store the created backup file on the node itself.
|minus|\ |minus|\ full-path            Print the full path of the files.
====================================== ===========

**Examples**

Assume we have a data node on 10.10.10.20 (port 3306) on cluster id 2, that we want to backup all databases:

.. code-block:: bash

	$ s9s backup --create --backup-method=mysqldump --cluster-id=2 --nodes=10.10.10.20:3306 --backup-directory=/storage/backups

List all backups for cluster ID 2:

.. code-block:: bash

	$ s9s backup --list --cluster-id=2 --long --human-readable

.. Note:: Omit the ``--cluster-id=2`` and to see the backup records for all your cluster.

Restore backup ID 3 on cluster ID 2:

.. code-block:: bash
	
	$ s9s backup --restore --cluster-id=2 --backup-id=3 --wait

s9s job
'''''''''''

View jobs.

**Usage**

.. code-block:: bash

	s9s job {command} {options}

**Command**

====================================== ===================
Name, shorthand                        Description
====================================== ===================
|minus|\ |minus|\ list                 List the jobs.
|minus|\ |minus|\ log                  Inspect the job messages.
|minus|\ |minus|\ wait                 Attach to a job and see the progress and status.
====================================== ===================

**Options**

========================================== ===========
Name, shorthand                            Description
========================================== ===========
|minus|\ |minus|\ job-id=ID                The ID of the job.
|minus|\ |minus|\ date-format=FORMAT       The format of the dates printed.
|minus|\ |minus|\ from=DATE&TIME           The start of the interval to be printed.
|minus|\ |minus|\ until=DATE&TIME          The end of the interval to be printed.
|minus|\ |minus|\ schedule=DATE&TIME       Run the job at the specified time.
========================================== ===========

**Examples**


List jobs:

.. code-block:: bash

	$ s9s job --list
	10235 RUNNING  dba     2017-01-09 10:10:17   2% Create Galera Cluster
	10233 FAILED   dba     2017-01-09 10:09:41   0% Create Galera Cluster

The s9s client will send a job that will be executed in the background by cmon. It will printout the job ID, for example:
"Job with ID 57 registered."

It is then possible to attach to the job to find out the progress:

.. code-block:: bash

	$ s9s job --wait --job-id=57

View job log messages of job ID 10235:

.. code-block:: bash

	$ s9s job --log  --job-id=10235

s9s maint
'''''''''''

View and manipulate maintenance periods.

**Usage**

.. code-block:: bash

	s9s maint {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ list                 List the maintenance period.
|minus|\ |minus|\ create               Create a new maintenance period.
|minus|\ |minus|\ delete               Delete a maintenance period.
====================================== ===========

**Options**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ cluster-id=ID        The cluster for cluster maintenance.
|minus|\ |minus|\ nodes=NODELIST       The nodes for the node maintenances.
|minus|\ |minus|\ full-uuid            Print the full UUID.
|minus|\ |minus|\ start=DATE&TIME      The start of the maintenance period.
|minus|\ |minus|\ end=DATE&TIME        The end of the maintenance period.
|minus|\ |minus|\ reason=STRING        The reason for the maintenance.
|minus|\ |minus|\ date-format=FORMAT   The format of the dates printed.
|minus|\ |minus|\ uuid=UUID            The UUID to identify the maintenance period.
====================================== ===========

**Examples**

List out all node under maintenance period:

.. code-block:: bash

	$ s9s maint --list --long
	ST UUID    OWNER           GROUP START    END      HOST/CLUSTER REASON
	-h 70346c3 admin@email.com       07:42:18 08:42:18 10.0.0.209   Upgrading RAM
	Total: 1

Delete a maintenance period for UUID 70346c3:

.. code-block:: bash

	$ s9s maint --delete --uuid=70346c3

s9s process
'''''''''''

View processes running on nodes.

**Usage**

.. code-block:: bash

	s9s process {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ list                 List the processes.
|minus|\ |minus|\ top                  Continuously print top processes.
====================================== ===========

**Options**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ cluster-id=ID        The ID of the cluster to show.
|minus|\ |minus|\ update-freq=SECS     The screen update frequency.
====================================== ===========

**Examples**

Continuously print aggregated view of processes (similar to ``top`` output) of all nodes for cluster ID 1:

.. code-block:: bash

	$ s9s process --top --cluster-id=1

List aggregated view of processes (similar to ``ps`` output) of all nodes for cluster ID 1:

.. code-block:: bash

	$ s9s process --list --cluster-id=1

s9s user
'''''''''''

Manage users.

**Usage**

.. code-block:: bash

	s9s user {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ create               Create a user.
|minus|\ |minus|\ whoami               List the current user only.
|minus|\ |minus|\ list                 List users.
====================================== ===========

**Options**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ cmon-user, -u        Username on the CMON system.
|minus|\ |minus|\ generate-key, -g     Generate an RSA keypair for the user.
|minus|\ |minus|\ group                The primary group of the new user.
|minus|\ |minus|\ create-group         Create the group if it does not exist.
|minus|\ |minus|\ first-name           The first name of the user.
|minus|\ |minus|\ last-name            The last name of the user.
|minus|\ |minus|\ title                The prefix title of the user.
|minus|\ |minus|\ email-address        The email address of the user.
====================================== ===========

**Examples**

Create a remote s9s user and generate a SSH key for the user:

.. code-block:: bash

	$ s9s user --create --generate-key --cmon-user=dba --controller="https://10.0.1.12:9501"

List out all users:

.. code-block:: bash

	$ s9s user --list --long
	A ID UNAME      GNAME  EMAIL REALNAME
	-  1 system     admins -     System User
	-  2 nobody     nobody -     -
	A  3 dba        users  -     -
	-  4 remote_dba users  -     -

s9s script
'''''''''''

Manage and execute Advisor scripts available in the `Developer Studio <user-guide/mysql/manage.html#developer-studio>`_.

**Usage**

.. code-block:: bash

	s9s script {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ tree                 Print the scripts available on the controller.
|minus|\ |minus|\ execute              Execute a script.
====================================== ===========

**Options**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ cluster-id=ID        The cluster for cluster maintenance.
====================================== ===========

**Examples**

Print the scripts available for cluster ID 1:

.. code-block:: bash

	$ s9s script --tree --cluster-id=1


Limitations
````````````

Currently the s9s command line tool has a user management module that is not yet fully integrated with ClusterControl UI and ClusterControl Controller. For example, there is no RBAC (Role-Based Access Control) for a user (see Setup and Configuration how to create a user). This means that any user created to be used with the s9s command line tool has a full access to all clusters managed by the ClusterControl server.

Users created by the command line client will be shown in Job Messages, but it is not possible to use this user to authenticate and login from the UI. Thus, the users created from the command line client are all super admins.

Reporting Issues
````````````````

If you would encounter issues, have questions, or have any features you would like to see included, please create an issue on https://github.com/severalnines/s9s-tools/issues .

.. rubric:: Footnotes

.. [#f1]

    We are gradually in the process of migrating all functionalities in REST API to RPC interface. Kindly expect the REST API to be obselete in the near future.
