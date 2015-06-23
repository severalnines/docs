Overview
--------

Provides summary of all database nodes in the cluster.

Actions
```````

Provides shortcuts to main cluster functionality. Each of database clusters has its own set of functionality as described below:

Galera Cluster
''''''''''''''

* **Add Node**
	- See `Add Node <#add-node>`_ section.

* **Add Load Balancer**
	- See `Add Load Balancer <manage.html#load-balancer>`_ section.

* **Add Replication Slave**
	- Deploys a replication slave attached to this cluster. Choose one of the Galera node to be a master. See `Add Replication Slave`_.

* **Clone Cluster** 
	- See `Clone`_ section.

* **Find Most Advanced Node**
	- Finds which is the most advanced node in the cluster. This is very useful to determine which node to be bootstrapped if the cluster doesn't have any primary component or when cluster automatic recovery is disabled.

* **Bootstrap Cluster**
	- Launches the bootstrap cluster window. Similar to *ClusterControl > Actions > Bootstrap Cluster*. ClusterControl will stop all running nodes before bootstrapping the cluster from the selected Galera node.

* **Stop Cluster**
	- Stop all nodes in the cluster.

* **Delete Cluster**
	- Unregister a database cluster from the ClusterControl UI. This action will remove the selected CMONAPI URL and token for corresponding cluster and will NOT uninstall the actual database cluster.
	- You can still re-register your cluster to ClusterControl at a later stage.

MySQL standalone/replication
''''''''''''''''''''''''''''

* **Add Node**
	- See `Add Node`_ section.

* **Delete Cluster**
	- Unregister a database cluster from the ClusterControl UI. This action will remove the selected CMONAPI URL and token for corresponding cluster and will NOT uninstall the actual database cluster.
	- You can still re-register your cluster to ClusterControl at a later stage.


MySQL Cluster
'''''''''''''

* **Add Node**
	- See `Add Node`_ section.

* **Add Load Balancer**
	- See `Add Load Balancer <manage.html#load-balancer>`_ section.

* **Delete Cluster**
	- Unregister a database cluster from the ClusterControl UI. This action will remove the selected CMONAPI URL and token for corresponding cluster and will NOT uninstall the actual database cluster.
	- You can still re-register your cluster to ClusterControl at a later stage.

Add Node
''''''''

Adds a new or existing database node into the cluster. You can scale out your cluster by adding mode database nodes. The new node will automatically join and synchronize with the rest of the cluster. 

Create and add a new DB node
............................

If you specify a new hostname or IP address, make sure that the node is accessible from ClusterControl node and following conditions are true:

* ClusterControl host is able to connect to the target host via passwordless SSH.
* The target host must use the same operating system distribution with the ClusterControl host.

Starting from version 1.2.6, this is only available for Galera Cluster, MySQL Replication (adding slave) and MySQL Cluster.

* **Hostname**
	- IP address or :term:`FQDN` of the target node. If you already have the host added under *ClusterControl > Manage > Hosts*, you can just choose the host from the dropdown menu.

* **Configuration**
	- Choose a MySQL configuration template for the new node. The configuration file should be created at *ClusterControl > Manage > Configurations > Template Configuration Files*.
	
* **Install Software**
	- If you already have the database server installed on the target host but not yet configured, you can tell ClusterControl to skip the database installation part by choosing 'No'.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled.

Add an existing DB node
.......................

Use this function if you have added a DB node manually to your cluster and want it to be detected/managed by ClusterControl. ClusterControl will then detect the new DB node as being part of the cluster and starts to manage and monitor it as with the rest of the cluster nodes. Useful if a node has been created outside of ClusterControl e.g, through Puppet, Chef or Ansible.

* **Hostname**
	- IP address or :term:`FQDN` of the target node. If you already have the host added under *ClusterControl > Manage > Hosts*, you can just choose the host from the dropdown menu.

* **Port**
	- MySQL port. Default is 3306.


Add Replication Slave
'''''''''''''''''''''

MySQL replication slave requires at least a master with GTID enabled on the Galera nodes. However, we would recommend users to configure all Galera nodes as master for better failover. GTID is required as it is used to do master failover (MariaDB's  GTID is not supported at the moment). If you are running on MySQL 5.5, you might need to upgrade to MySQL 5.6

The following must be true for the masters:

* At least one master among the Galera nodes
* MySQL GTID must be enabled
* ``log_slave_updates`` must be enabled
* Master’s MySQL port is accessible by ClusterControl and slaves

For the slave, you would need a separate host or VM, with or without MySQL installed. If you do not have a MySQL installed, and choose ClusterControl to install the MySQL on the slave, ClusterControl will perform the necessary actions to prepare the slave, for example, configure root password (based on ``monitored_mysql_root_password``), create slave user (based on ``repl_user``, ``repl_password``), configure MySQL, start the server and also start replication. The MySQL package used will be based on the Galera vendor used, for example, if you are running Percona XtraDB Cluster, ClusterControl will prepare the slave using Percona Server. Prior to the deployment, you must perform following actions beforehand:

* The slave node must be accessible using passwordless SSH from the ClusterControl server
* MySQL port (default 3306) and netcat port 9999 on the slave are open for connections.
* You must configure the following options in the ClusterControl configuration file for the respective cluster ID under ``/etc/cmon.cnf`` or ``/etc/cmon.d/cmon_<cluster ID>.cnf``:

.. code-block:: bash

	repl_user=<the replication user>
	repl_password=<password for replication user>
	monitored_mysql_root_password=<the mysql root password of all nodes including slave>

* The slave configuration template file must be configured beforehand, and must have at least the following variables defined in the MySQL configuration template:

.. code-block:: bash

	server_id
	basedir
	datadir

To prepare the MySQL configuration file for the slave, go to *ClusterControl > Manage > Configurations > Template Configuration files > edit my.cnf.slave*. Later, specify this template file when adding a slave.

We have covered an example deployment in `this blog post <http://www.severalnines.com/blog/deploy-asynchronous-slave-galera-mysql-easy-way>`_.

Clone
''''''

Exclusive for Galera Clusters. This feature allows you to create, in one click, an exact copy of your Galera Cluster onto a new set of hosts. The most common use case for cloning a deployment is for setting up a staging deployment for further development and test. Cloning is a ‘hot’ procedure and does not affect the operations of the source cluster. 

A clone will be created of this cluster. The follow procedure applies:

* Create a new Cluster consisting of one node
* Stage the new Cluster with SST (it is now cloned)
* Nodes will be added to the Cloned Cluster until *Cloned Cluster Size* is reached.
* Query Monitor settings and settings for Cluster Recovery and Node Recovery options are not cloned
* The ``my.cnf`` file may not be identical on the Cloned Cluster

* **Cloned Cluster Name**
	- The cloned cluster name.

* **Cloned Cluster Size**
	- The number of database node of the cloned cluster.

* **Disable Firewall On Cloned Nodes?**
	- Check the box to disable firewall on cloned nodes (recommended).

* **Disable SELinux/AppArmor on Cloned Nodes?**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) on cloned nodes.

* **DB Node (1-9)**
	- The database node IP address or hostname. The enable fields is depending on the Cloned Cluster Size.


Cluster Load
````````````

The Cluster Load graph provides overview of aggregated load on your database cluster. To jump into individual database load, click on ‘Show Servers’.

* **Dash Settings**
	- Customize the Cluster Load dashboard. See `Custom Dashboard`_ section.

* **Show Servers**
	- Show real-time individual node database load.

* **Show Queries**
	- Show real-time queries across all nodes.

* **Sync Graphs**
	- Sync all graph (cluster load and server load) when selecting a range.

* **Refresh Rate**
	- The number of seconds all values should be updated under Cluster Load.

* **Connections**
	- The number of aggregated connections across all nodes.

* **Selects**
	- The number of aggregated SELECT queries across all nodes.

* **Inserts**
	- The number of aggregated INSERT queries across all nodes.

* **Updates**
	- The number of aggregated UPDATE queries across all nodes.

* **Delete**
	- The number of aggregated DELETE queries across all nodes.

* **Queries**
	- The total of all queries running across all nodes.

Custom Dashboard
````````````````

Customize your dashboard in the `Overview`_ page by selecting which metrics and graphs to display. For Galera nodes, 3 graphs are configured by default:

====================== ===========
Dashboard Name         Description
====================== ===========
Cluster Load           Shows aggregated load on your database cluster.
Galera - Flow Control  Shows the replication performance.
InnoDB - Disk IO       Shows IO read/write stats for InnoDB.
Galera - Innodb/Flow   Shows InnoDB IO stats alongside Galera replication performance.
Handler                Shows MySQL handler status.
Query Performance      Shows the number of "slow performing" queries such as table scans and joins without indexes.
====================== ===========

The created custom dashboards will appear as tabs beside *Dash Settings*.

* **Dashboard Name**
	- Give a name to the dashboard.

* **Metric**
	- Select an available metric from the list.

* **Scale**
	- Choose between linear or logarithmic graph scale.

* **Selected as Default Graph**
	- Choose Yes if you want to set the graph as default when viewing the Overview page.

.. Note:: You can rearrange dashboard order by drag and drop above.

Server Load
````````````

Drill down into metrics for individual servers. Click on *Show CPU, Net and Disk* to view monitoring data on CPU, network and disk for the corresponding host.

* **Show CPU, Net and Disk**
	- Drill down to each of the selected node’s CPU, network and disk load.

Cluster-wide Queries
``````````````````````

Provides aggregated view of all queries running across all database nodes in the cluster. This page is auto-refreshed every 30 seconds. You can change the refresh rate by clicking on the arrow beside the greenRefresh icon. Click on any SELECT query to see the execution plan.

* **Filter by Server**
	- Filter the query list based on database node.

* **Email Query**
	- Email the selected query to recipients listed in *ClusterControl > Settings > General Settings > Email Notification*.

* **Time**
	- Timestamp on last query sampling.

* **Query**
	- The parameterized query.

* **Count**
	- How many times the query occurred.

* **Max Query Time**
	- The maximum amount of time the query executed.

* **Max Lock Time**
	- The maximum amount of time the query spent waiting to acquire the lock it needs to run.

Hosts/Nodes Statistics
``````````````````````

This provides a summary of host and replication-related stats for all nodes. These values are refreshed every *Refresh rate* values defined at the top of the page. 

Each database cluster has it’s own set of statistics as explained below:

Galera Cluster
..............

Galera Nodes Grid
+++++++++++++++++

* **Host**
	- Database node hostname or IP address

* **Status**
	- This variable shows internal Galera node state. See `wsrep_local_state_comment <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-local-state-comment>`_. Possible values are:
		- Joining (requesting/receiving State Transfer) - node is joining the cluster
		- Donor/Desynced - node is the donor to the node joining the cluster
		- Joined - node has joined the cluster
		- Synced - node is synced with the cluster
	- Status of the cluster component. See `wsrep_cluster_status <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-cluster-status>`_. Possible values are:
		- Primary
		- Non-Primary
		- Disconnected

* **WSREP Cluster Size**
	- Current number of nodes in the cluster. See `wsrep_cluster_size <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-cluster-size>`_.

* **WSREP Ready**
	- This variable shows whether the node is ready to accept queries. If status is OFF almost all the queries will fail with ``ERROR 1047 (08S01) Unknown Command`` error (unless wsrep_on variable is set to 0). See `wsrep_ready <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-ready>`_.

* **Local Queue (Send/Receive)**
	- Average length of the send/receive queue since the last status query. When the cluster experiences network throughput issues or replication throttling this value will be greater than 0. See `wsrep_local_send_queue_avg <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-local-send-queue-avg>`_ and `wsrep_local_recv_queue_avg <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-local-recv-queue-avg>`_.

* **Flow Control Paused/Sent**
	- Time since the last status query that replication was paused due to flow control. See `wsrep_flow_control_paused <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-flow-control-paused>`_.
	- Number of wsrep_flow_control_paused events sent since the last status query. See `wsrep_flow_control_sent <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-flow-control-sent>`_.

* **Cert Deps Distance**
	- Average distance between highest and lowest sequence number that can be possibly applied in parallel. See `wsrep_cert_deps_distance <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-cert-deps-distance>`_.

* **Segment ID**
	- WAN segment identifier number. See `gmcast.segment <http://galeracluster.com/documentation-webpages/galeraparameters.html#gmcast-segment>`_.

* **Last Committed**
	- Sequence number of the last committed transaction. See `wsrep_last_committed <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-last-committed>`_.

* **Server Version**
	- MySQL server version. 

* **Uptime**
	- MySQL service uptime.

* **Last Updated**
	- The last time ClusterControl fetch for node's status.
	
* **Refresh**
	- Fetch the latest update.

Master Nodes Grid
+++++++++++++++++

This grid appears if you configured Galera node to produce binary log with a unique ``server_id`` value.

* **Host**
	- The MySQL master hostname or IP address.
	
* **Server ID**
	- MySQL server ID.

* **File**
	- Current binary log file.

* **Position**
	- Current binary log position.

* **Binlog_Do_Db**
	- Value of ``binlog_do_db`` option.

* **Binlog_Ignore_Db**
	- Value of ``binlog_ignore_db`` option.
	
* **Executed Gtid Set**
	- Shows the set of GTIDs for transactions that have been executed on the master.

* **Refresh**
	- Fetch the latest update.

Slave Nodes Grid
++++++++++++++++

This grid appears if you have a replication slave attached to the Galera cluster.

* **Host**
	- The MySQL slave hostname or IP address.

* **Server ID**
	- MySQL server ID.

* **Role**
	- Replication role. For slaves, it can be 'slave' or 'multi', where the slave also produces binary log.
	
* **Status**
	- The state of the SQL thread. The value is identical to the State value of the SQL thread as displayed by ``SHOW PROCESSLIST``.

* **Master Host**
	- The master host that the slave is connected to.

* **Lag**
	- How many seconds this slave behind the master.

* **Master Log File**
	- The name of the master binary log file from which the I/O thread is currently reading.

* **Read Master Log Pos**
	- The position in the current master binary log file up to which the I/O thread has read.

* **Exec Master Log Pos**
	- The position in the current master binary log file to which the SQL thread has read and executed, marking the start of the next transaction or event to be processed.

* **Retrieved Gtid Set**
	- Shows the set of GTIDs for transactions that have been received by this slave.

* **Executed Gtid Set**
	- Shows the set of GTIDs for transactions that have been executed on the master.

* **Refresh**
	- Fetch the latest update.


MySQL single instance or replication
....................................

Standalone Nodes Grid
+++++++++++++++++++++

* **Host**
	- Database node hostname or IP address

* **Connections**
	- How many MySQL threads connected.

* **Queries**
	- The number of queries running on this node per second.

* **Selects**
	- The number of SELECT queries on this node per second.

* **Inserts**
	- The number of SELECT queries on this node per second.

* **Updates**
	- The number of SELECT queries on this node per second.

Master Nodes Grid
+++++++++++++++++

This grid appears if you configured MySQL node to produce binary log with a unique ``server_id`` value.

* **Host**
	- The MySQL master hostname or IP address.
	
* **Server ID**
	- MySQL server ID.

* **Role**
	- Replication role.
	
* **Status**
	- The state of the SQL thread. The value is identical to the State value of the SQL thread as displayed by ``SHOW PROCESSLIST``.
	
* **Executed Gtid Set**
	- Shows the set of GTIDs for transactions that have been executed on the master.
	
* **Binlog**
	- Current binary log file.

* **Position**
	- Current binary log position.

* **Binlog do db**
	- Value of ``binlog_do_db`` option.

* **Binlog ignore db**
	- Value of ``binlog_ignore_db`` option.

Slave Nodes Grid
+++++++++++++++++

This grid appears if you have slaves replicating from a master.

* **Host**
	- The MySQL slave hostname or IP address.

* **Server ID**
	- MySQL server ID.

* **Role**
	- Replication role. For slaves, it can be 'slave' or 'multi', where the slave also produces binary log.
	
* **Status**
	- The state of the SQL thread. The value is identical to the State value of the SQL thread as displayed by ``SHOW PROCESSLIST``.

* **Master Host**
	- The master host that the slave is connected to.

* **Lag**
	- How many seconds this slave behind the master.

* **Master Log File**
	- The name of the master binary log file from which the I/O thread is currently reading.

* **Read Master Log Pos**
	- The position in the current master binary log file up to which the I/O thread has read.

* **Exec Master Log Pos**
	- The position in the current master binary log file to which the SQL thread has read and executed, marking the start of the next transaction or event to be processed.

* **Retrieved Gtid Set**
	- Shows the set of GTIDs for transactions that have been received by this slave.

* **Executed Gtid Set**
	- Shows the set of GTIDs for transactions that have been executed on the master.

* **Refresh**
	- Fetch the latest update.

MySQL Cluster
.............

Management Nodes Grid
+++++++++++++++++++++

* **Instance**
	- Management node hostname or IP address

* **Status**
	- The MySQL Cluster management daemon status.

* **Node ID**
	- MySQL Cluster node identifier number.

* **Version**
	- NDB version.

* **Uptime**
	- MySQL Cluster management service uptime.

* **Last Updated**
	- The last time ClusterControl fetch for node's status.

* **Refresh**
	- Fetch the latest update.

SQL Nodes Grid
++++++++++++++

* **Instance**
	- SQL node hostname or IP address.

* **SQL Status**
	- The MySQL Cluster SQL/API daemon status.

* **Hosts Stats**
	- Ping - ping round trip from ClusterControl host to each host in miliseconds.
	- CPU Util - total of CPU utilization in percentage.
	- Uptime - System host uptime.

* **Server Stats**
	- Queries - Number of queries running on the node.
	- Connections - Number of connected thread on the node.
	- Uptime - MySQL API service uptime.

* **Version**
	- MySQL Cluster and NDB versions.

* **Last Error**
	- Last error when ClusterControl talks SQL to the mysql server. "Failed to connect", "connection refused etc.."

* **Last Updated**
	- The last time ClusterControl fetch for node's status
	
* **Refresh**
	- Fetch the latest update.

Data Nodes Grid
+++++++++++++++

* **Instance**
	- Data node hostname or IP address.

* **Status**
	- The data node status.

* **Hosts Stats**
	- Ping - ping round trip from ClusterControl host to each host in miliseconds.
	- CPU Util - total of CPU utilization in percentage.
	- Uptime - System host uptime.

* **Node ID**
	- MySQL Cluster node identifier number.

* **Last Updated**
	- The last time ClusterControl fetch for node's status
	
* **Refresh**
	- Fetch the latest update.

Hosts
`````

Shows collected system statistics in a table as below:

* **Ping**
	- ping round trip from ClusterControl host to each host in milliseconds.

* **CPU util/steal**
	- total of CPU utilization in percentage.

* **Loadavg (1/5/15)**
	- load value captured for 1, 5 and 15 minutes average.

* **Net (tx/s / rx/s)**
	- Amount of data transmitted and received by the host.

* **Disk read/sec**
	- Amount of disk read of ``monitored_mountpoint``.

* **Disk writes/sec**
	- Amount of disk write of ``monitored_mountpoints``.

* **Uptime**
	- Host uptime.

* **Last Updated**
	- The last time ClusterControl fetch for host's status.
