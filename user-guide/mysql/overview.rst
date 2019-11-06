.. _MySQL - Overview:

Overview
--------

Provides summary of all database nodes in the cluster. This page is accessible only if there is a cluster deployed by ClusterControl via :ref:`Deploy Database Cluster` or imported into ClusterControl via :ref:`Import Existing Server Cluster`.

.. _MySQL - Overview - Actions:

Actions
+++++++

Provides shortcuts to the main cluster functionality. Each database cluster has its own set of functionality as described below:

Galera Cluster
``````````````

* **Add Load Balancer**
	- See :ref:`MySQL - Manage - Load Balancer`.

* **Add Node**
	- See `Add Node`_.

* **Add Replication Slave**
	- Deploys a replication slave attached to this cluster. Choose one of the Galera node to be a master. See `Add Replication Slave`_.

* **Change RPC API Token**
	- Serves as the authentication string by ClusterControl UI to connect to CMON RPC interface. Each cluster has its own unique token.
	
.. Note:: You can retrieve the RPC API Token value directly from the respective CMON configuration file, ``/etc/cmon.d/cmon_{clusterID}.cnf``.

* **Clone Cluster** 
	- See `Clone Cluster`_.

* **Create Slave Cluster**
	- Create a new cluster that replicates from this cluster. See `Cluster-Cluster Replication`_.

* **Enable ReadOnly**
	- Enable cluster-wide read-only. ClusterControl will set ``read_only=ON`` on all database nodes in the cluster. This is very useful in cluster-to-cluster replication setup. See `Cluster-Cluster Replication`_.

* **Find Most Advanced Node**
	- Finds which is the most advanced node in the cluster. This is very useful to determine which node to be bootstrapped if the cluster doesn't have any primary component or when ClusterControl automatic recovery is disabled.

* **Bootstrap Cluster**
	- Launches the bootstrap cluster window. ClusterControl will stop all running nodes before bootstrapping the cluster from the selected Galera node.

* **Delete Cluster**
	- This action will remove the corresponding cluster from ClusterControl supervision and will NOT uninstall the actual database cluster.
	- If you want to re-add the cluster, you have to use :ref:`Import Existing Server Cluster`.

* **Deregister Cluster from UI**
	- Unregister a database cluster from the ClusterControl UI. 
	- You can still re-register your cluster to ClusterControl at a later stage by using :ref:`UserGuide - Global Settings - Cluster Registrations`.
	
* **Remove Node**
	- Remove a managed node from the cluster.

* **Stop Cluster**
	- Stop all nodes in the cluster.

MySQL Replication
``````````````````

* **Add Node**
	- See `Add Node`_ section.
	
* **Add Load Balancer**
	- See :ref:`MySQL - Manage - Load Balancer`.

* **Change RPC API Token**
	- Serves as the authentication string by ClusterControl UI to connect to CMON RPC interface. Each cluster has its own unique token.

* **Find Most Advanced Node**
    - Finds which is the most advanced node in the replication setup. This is very useful to determine which slaves has the most updated data before being promoted to a new master.

* **Delete Cluster**
	- This action will remove the corresponding cluster from ClusterControl supervision and will NOT uninstall the actual database cluster.
	- If you want to re-add the cluster, you have to use :ref:`Import Existing Server Cluster`.

* **Deregister Cluster from UI**
	- Unregister a database cluster from the ClusterControl UI. 
	- You can still re-register your cluster to ClusterControl at a later stage by using :ref:`UserGuide - Global Settings - Cluster Registrations`.
	
* **Remove Node**
	- Remove a managed node from the cluster.

MySQL Standalone
````````````````

* **Add Node**
	- See `Add Node`_ section.
	
* **Add Load Balancer**
	- See :ref:`MySQL - Manage - Load Balancer`.

* **Change RPC API Token**
	- Serves as the authentication string by ClusterControl UI to connect to CMON RPC interface. Each cluster has its own unique token.

* **Delete Cluster**
	- This action will remove the corresponding cluster from ClusterControl supervision and will NOT uninstall the actual database cluster.
	- If you want to re-add the cluster, you have to use :ref:`Import Existing Server Cluster`.

* **Deregister Cluster from UI**
	- Unregister a database cluster from the ClusterControl UI. 
	- You can still re-register your cluster to ClusterControl at a later stage by using :ref:`UserGuide - Global Settings - Cluster Registrations`.

MySQL Group Replication
````````````````````````

* **Add Replication Slave**
	- Deploys a replication slave attached to this cluster. Choose one of the Group Replication node to be a master. See `Add Replication Slave`_.

* **Change RPC API Token**
	- Serves as the authentication string by ClusterControl UI to connect to CMON RPC interface. Each cluster has its own unique token.

* **Bootstrap Cluster**
	- Launches the bootstrap cluster window. Similar to *ClusterControl > Actions > Bootstrap Cluster*. ClusterControl will stop all running nodes before bootstrapping the cluster from the selected Galera node.

* **Stop Cluster**
	- Stop all nodes in the cluster.

* **Delete Cluster**
	- This action will remove the corresponding cluster from ClusterControl supervision and will NOT uninstall the actual database cluster.
	- If you want to re-add the cluster, you have to use :ref:`Import Existing Server Cluster`.

* **Deregister Cluster from UI**
	- Unregister a database cluster from the ClusterControl UI. 
	- You can still re-register your cluster to ClusterControl at a later stage by using :ref:`UserGuide - Global Settings - Cluster Registrations`.
	
* **Remove Node**
	- Remove a managed node from the cluster.

MySQL Cluster
``````````````

* **Add SQL Node**
	- Add MySQL Cluster SQL node. See `Add Node`_ section.

* **Add Load Balancer**
	- See :ref:`MySQL - Manage - Load Balancer`.
	
* **Change RPC API Token**
	- Serves as the authentication string by ClusterControl UI to connect to CMON RPC interface. Each cluster has its own unique token.

* **Delete Cluster**
	- This action will remove the corresponding cluster from ClusterControl supervision and will NOT uninstall the actual database cluster.
	- If you want to re-add the cluster, you have to use :ref:`Import Existing Server Cluster`.

* **Deregister Cluster from UI**
	- Unregister a database cluster from the ClusterControl UI. 
	- You can still re-register your cluster to ClusterControl at a later stage by using :ref:`UserGuide - Global Settings - Cluster Registrations`.

Add Node
````````

Adds a new or existing database node into the cluster. You can scale out your cluster by adding mode database nodes. The new node will automatically join and synchronize with the rest of the cluster. 

Create and add a new DB node
''''''''''''''''''''''''''''

If you specify a new hostname or IP address, make sure that the node is accessible from ClusterControl node via passwordless SSH.

This is only available for Galera Cluster, MySQL Replication (adding slave) and MySQL Cluster.

* **Hostname**
	- IP address or :term:`FQDN` of the target node. If you already have the host added under *ClusterControl > Manage > Hosts*, you can just choose the host from the dropdown menu.

* **Configuration**
	- Choose a MySQL configuration template for the new node.
	
* **Install Software**
	- If you already have the database server installed on the target host but not yet configured, you can tell ClusterControl to skip the database installation part by choosing 'No'.

* **Disable Firewall**
	- Yes - Firewall will be disabled (recommended).
	- No - ClusterControl will not disabling any enabled firewall rules.

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled.

* **Include in Loadbalancer set (if exist)**
	- The node will be added into the load balancing set if you have HAProxy or MaxScale deployed with ClusterControl.
	
* **Do you want to delay the slave?**
	- Yes - Sets up a delayed slave.
	- No - Sets up a standard slave.
	
* **Delay slave with**
	- This option will appear only if you select Yes. Specify the value in seconds.

Add an existing DB node
'''''''''''''''''''''''

Use this feature if you have added a DB node manually to your cluster and want it to be detected and managed by ClusterControl. ClusterControl will then detect the new DB node as being part of the cluster and starts to manage and monitor it as with the rest of the cluster nodes. Useful if a node has been created outside of ClusterControl e.g, through Puppet, Chef or Ansible.

* **Hostname**
	- IP address or :term:`FQDN` of the target node.

* **Port**
	- MySQL port. Default is 3306.


Add Replication Slave
``````````````````````

MySQL replication slave requires at least a master with GTID enabled on the Galera nodes. However, we would recommend users to configure all Galera nodes as master for better failover. GTID is required as it is used to do master failover (MariaDB's  GTID is not supported at the moment). If you are running on MySQL 5.5, you might need to upgrade to MySQL 5.6.

The following must be true for the masters:

* At least one master among the Galera nodes.
* MySQL GTID must be enabled.
* ``log_slave_updates`` must be enabled.
* Master’s MySQL port is accessible by ClusterControl and slaves.

To configure a Galera node as master, go to *ClusterControl > Nodes > choose the mysql server > Enable Binary Logging*. In the "Enable Binary Logging" dialog, set the binary logs expiration, set "Enable GTID" to yes and "auto-restart node" to yes, then click Proceed.

Or, you can also achieve the same thing manually by appending the following lines into the corresponding ``my.cnf``. Do not forget to restart the MySQL server to load the changes:

.. code-block:: bash

	server_id=<must be unique across all mysql servers participating in replication>
	binlog_format=ROW
	log_slave_updates=1
	log_bin=binlog
	gtid_mode=ON
	enforce_gtid_consistency=1

For the slave, you would need a separate host or VM, with or without MySQL installed. If you do not have a MySQL installed, and choose ClusterControl to install the MySQL on the slave, ClusterControl will perform the necessary actions to prepare the slave, for example, configure root password (based on ``monitored_mysql_root_password``), create slave user, configure MySQL, start the server and also start the replication. The MySQL package used will be based on the Galera vendor used, for example, if you are running Percona XtraDB Cluster, ClusterControl will prepare the slave using Percona Server. Prior to the deployment, you must perform following actions:

* The slave node must be accessible using passwordless SSH from the ClusterControl server
* MySQL port (default 3306) and :term:`netcat` port 9999 on the slave are open for connections.
* You must configure the following options in the ClusterControl configuration file for the respective cluster ID under ``/etc/cmon.cnf`` or ``/etc/cmon.d/cmon_<cluster ID>.cnf``:

.. code-block:: bash

	monitored_mysql_root_password=<the mysql root password of all nodes including slave>


We have covered an example deployment in this blog post, `Deploy an asynchronous slave to Galera Cluster for MySQL - The Easy Way <http://www.severalnines.com/blog/deploy-asynchronous-slave-galera-mysql-easy-way>`_.

Add New Replication Slave
''''''''''''''''''''''''''

The slave will be setup from a streamed XtraBackup from the master to the slave. 

* **Master Server**
	- Select a master server. Only Galera nodes that generate binary log are listed here.

* **Slave Server**
	- Specify the IP address or FQDN of the slave node. This node must be accessible from ClusterControl node via passwordless SSH beforehand.

* **Netcat port**
	- Choose a port to stream Xtrabackup. Default port is 9999. This port must be reachable by the selected Master Server.

* **Do you want to delay the slave?**
	- Yes - Sets up a delayed slave.
	- No - Sets up a standard slave.
	
* **Delay slave with**
	- This option will appear only if you select Yes. Specify the value in seconds.

* **Do you want to install the Slave server**
	- Yes - Install MySQL Server packages. It will based on the repository and vendor for Galera node. For example, if you are running on Percona XtraDB Cluster, ClusterControl will setup a standalone Percona XtraDB Cluster node as the slave.

* **Disable firewall**
	- Check the box to disable firewall (recommended).

* **Disable SELinux/AppArmor**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled (recommended).

.. Note:: Existing MySQL server packages will be uninstalled.


Add Existing Replication Slave
''''''''''''''''''''''''''''''

Add an existing replication slave into ClusterControl. Use this feature if you have added a replication slave manually to your cluster and want it to be detected/managed by ClusterControl. ClusterControl will then detect the new database node as being part of the cluster and starts to manage and monitor it as with the rest of the cluster nodes. Useful if a node has been configured outside of ClusterControl e.g, through Puppet, Chef or Ansible.

* **Hostname**
	- Specify the slave IP address or FQDN.

* **Port**
	- MySQL port. Default is 3306. This port must be reachable by ClusterControl.


Clone Cluster
``````````````

Exclusive for Galera Cluster. This feature allows you to create, in one click, an exact copy of your Galera Cluster onto a new set of hosts. The most common use case for cloning a deployment is for setting up a staging deployment for further development and test. Cloning is a ‘hot’ procedure and does not affect the operations of the source cluster. 

A clone will be created of this cluster. The following procedure applies:

* Create a new Cluster consisting of one node.
* Stage the new Cluster with SST (it is now cloned).
* Nodes will be added to the Cloned Cluster until *Cloned Cluster Size* is reached.
* Query Monitor settings and settings for Cluster Recovery and Node Recovery options are not cloned.
* The ``my.cnf`` file may not be identical on the Cloned Cluster.

* **Cloned Cluster Name**
	- The cloned cluster name.

* **Cloned Cluster Size**
	- The number of database node of the cloned cluster.

* **Disable Firewall On Cloned Nodes?**
	- Check the box to disable firewall on cloned nodes (recommended).

* **Disable SELinux/AppArmor on Cloned Nodes?**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) on cloned nodes.

* **DB Node (1-9)**
	- The database node IP address or hostname. The enable fields is depending on the Cloned Cluster Size.
	
Cluster-Cluster Replication
````````````````````````````

Exclusive for Galera Cluster. This feature allows you to create a new cluster that will be replicating from this cluster. One primary use case is for disaster recovery by having a hot standby site/cluster which can take over when the main site/cluster has failed. Clusters can be rebuilt with an existing backup or by streaming from a master on the source cluster.

For MySQL-based clusters, ClusterControl will configure asynchronous MySQL replication from a master cluster to a slave cluster.

* **Cluster Provisioning Data**
	- Choose one method to provision the slave's cluster data:
		- *Streaming from the master*: Stream the data from a master using hot backup tools e.g, Percona Xtrabackup and MariaDB Backup.
		- *Stage cluster from backup*: Choose an existing full backup from the dropdown list. If none is listed, take full backup of one of the nodes in your cluster which have binary logging enabled.

* **Replication Master**
	- A node of the source cluster to replicate from. For MySQL, the chosen node must have binary logs enabled. To do this, go to *Nodes > pick the corresponding node > Node Actions >  Enable Binary Logging*.

Once the above options have been selected, the cluster deployment wizard will appear similar to deploying a new cluster. See :ref:`Deploy Database Cluster`.

A slave cluster will appear in the database cluster list after deployment finishes. You will notice the slave cluster entry is a bit indented in the list, with a pointed arrow coming from the source cluster, indicating the cluster-cluster replication is now active.

.. Note:: We highly recommend users to enable cluster-wide read-only on the slave cluster. Disable read-only only when promoting the slave cluster as the new master cluster.


.. _MySQL - Overview - Cluster Load:

Cluster Load
++++++++++++

The Cluster Load graph provides overview of aggregated load on your database cluster. To jump into individual database load, click on ‘Show Servers’.

* **Dash Settings**
	- Customize the Cluster Load dashboard. See :ref:`MySQL - Overview - Custom Dashboard` section.

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
	- The total of all queries running across all nodes. The total number of queries is including statements like SET, BEGIN, COMMIT, etc. These statements are frequently executed by ORMs or during creation of a connection (for instance "SET NAMES UTF8") and thus create a lot of "Queries" even though they are not any queries that read or write to the database. Therefore a sum of selects, updates, deletes and inserts will not the same as the value of "Queries".

.. _MySQL - Overview - Custom Dashboard:

Custom Dashboard
+++++++++++++++++

Customize the dashboard in the :ref:`MySQL - Overview` page by selecting which metrics and graphs to display. For Galera nodes, 6 graphs are configured by default:

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

The created custom dashboards will appear as tabs right before *Dash Settings*.

* **Dashboard Name**
	- Give a name to the dashboard.

* **Metric**
	- Select an available metric from the list.

* **Scale**
	- Choose between linear or logarithmic graph scale.

* **Selected as Default Graph**
	- Choose Yes if you want to set the graph as default when viewing the Overview page.

.. Note:: You can re-arrange dashboard order by drag and drop.

.. _MySQL - Overview - Server Load:

Server Load
+++++++++++

Drill down into metrics for individual servers. Click on *Show CPU, Net and Disk* to view monitoring data on CPU, network and disk for the corresponding host.

* **Show CPU, Net and Disk**
	- Drill down to each of the selected node’s CPU, network and disk load.

.. _MySQL - Overview - Cluster-wide Queries:

Cluster-wide Queries
+++++++++++++++++++++

Provides aggregated view of all queries running across all database nodes in the cluster. This page is auto-refreshed every 30 seconds. You can change the refresh rate by clicking on the arrow beside the greenRefresh icon. Click on any SELECT query to see the execution plan.

* **Filter by Server**
	- Filter the query list based on database node.

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

.. _MySQL - Overview - Database Nodes Stats:

Database Stats
++++++++++++++

This provides a summary of database and replication-related metrics for all nodes. These values are refreshed every *Refresh rate* values defined at the top of the page. 

Each database cluster has it’s own set of metrics as explained below:

Galera Cluster
``````````````

Galera Nodes Grid
''''''''''''''''''

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

* **WSREP Cluster Size/Ready**
	- Current number of nodes in the cluster. See `wsrep_cluster_size <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-cluster-size>`_.
	- This Ready variable shows whether the node is ready to accept queries. If status is OFF almost all the queries will fail with ``ERROR 1047 (08S01) Unknown Command`` error (unless wsrep_on variable is set to 0). See `wsrep_ready <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-ready>`_.

* **Local Send Queue (now/avg)**
	- Current and average length of the local send queue since the last status query. When the cluster experiences network throughput issues or replication throttling this value will be greater than 0. See `wsrep_local_send_queue_avg <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-local-send-queue-avg>`_ and `wsrep_local_recv_queue_avg <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-local-recv-queue-avg>`_.

* **Local Receive Queue (now/avg)**
	- Current and average length of the local receive queue since the last status query. When the cluster experiences network throughput issues or replication throttling this value will be greater than 0. See `wsrep_local_send_queue_avg <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-local-send-queue-avg>`_ and `wsrep_local_recv_queue_avg <http://galeracluster.com/documentation-webpages/galerastatusvariables.html#wsrep-local-recv-queue-avg>`_.

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
''''''''''''''''''

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
''''''''''''''''''

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
	
MySQL Group Replication
````````````````````````

Master Nodes Grid
''''''''''''''''''

This grid appears if you configured MySQL node to produce binary log with a unique ``server_id`` value.

* **Host**
	- The MySQL master hostname or IP address.
	
* **Read Only**
	- Read-only status. Click on the button to change the state. It may take 10 seconds before the change is visible in the UI.

* **Server ID**
	- MySQL server ID.
	
* **Status**
	- The state of the SQL thread.
	
* **Member Status**
	- MySQL group replication member status.

* **Worker Status**
	- MySQL group replication worker status.

* **File**
	- Current binary log file.

* **Position**
	- Current binary log position.

* **Executed Gtid Set**
	- Shows the set of GTIDs for transactions that have been executed on the master.

* **Refresh**
	- Fetch the latest update.

MySQL Replication or Single Instance
``````````````````````````````````````

Standalone Nodes Grid
''''''''''''''''''''''

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
''''''''''''''''''

This grid appears if you configured MySQL node to produce binary log with a unique ``server_id`` value.

* **Host**
	- The MySQL master hostname or IP address.
	
* **Read Only**
	- Read-only status. Click on the button to change the state. It may take 10 seconds before the change is visible in the UI.

* **Server ID**
	- MySQL server ID.
	
* **Status**
	- The state of the SQL thread.

* **Executed Gtid Set**
	- Shows the set of GTIDs for transactions that have been executed on the master.
	
* **Binlog**
	- Current binary log file.

* **Position**
	- Current binary log position.
	
* **Executed Gtid Set**
	- Shows the set of GTIDs for transactions that have been executed on the master.

* **Binlog_Do_Db**
	- Value of ``binlog_do_db`` option.

* **Binlog_Ignore_Db**
	- Value of ``binlog_ignore_db`` option.

Slave Nodes Grid
''''''''''''''''''

This grid appears if you have slaves replicating from a master.

* **Host**
	- The MySQL slave hostname or IP address.

* **Read Only**
	- Read-only status.

* **Server ID**
	- MySQL server ID.
	
* **Status**
	- The state of the SQL thread. The value is identical to the State value of the SQL thread as displayed by ``SHOW SLAVE STATUS``.

* **Master Host**
	- The master host that the slave is connected to.

* **Lag**
	- How many seconds this slave is behind the master.

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

MySQL Cluster
``````````````

Management Nodes Grid
''''''''''''''''''''''

* **Instance**
	- Management node hostname or IP address

* **Node ID**
	- MySQL Cluster node identifier number.

* **Version**
	- NDB version.

* **Last Updated**
	- The last time ClusterControl fetch for node's status.

* **Refresh**
	- Fetch the latest update.

SQL Nodes Grid
''''''''''''''''''

* **Host**
	- SQL node hostname or IP address.

* **Connections**
	- The number of aggregated connections across all nodes.

* **Queries**
	- The total of queries running on the node. The total number of queries is including statements like SET, BEGIN, COMMIT, etc. These statements are frequently executed by ORMs or during creation of a connection (for instance "SET NAMES UTF8") and thus create a lot of "Queries" even though they are not any queries that read or write to the database. Therefore a sum of selects, updates, deletes and inserts will not the same as the value of "Queries".

* **Selects**
	- The number of current SELECT queries on the node.

* **Inserts**
	- The number of current INSERT queries on the node.

* **Updates**
	- The number of current UPDATE queries on the node.

* **Delete**
	- The number of current DELETE queries on the node.

* **Server Version**
	- MySQL server version.

* **Uptime**
	- MySQL service uptime.

* **Last Updated**
	- The last time ClusterControl fetch for node's status.
	
* **Refresh**
	- Fetch the latest update.

Data Nodes Grid
''''''''''''''''''

* **Instance**
	- Data node hostname or IP address.
	
* **Node ID**
	- MySQL Cluster node identifier number.

* **Index Memory Used**
	- Index usage in percentage.

* **Data Memory Used**
	- Data usage in percentage.

* **LongMemoryBuffer Used**
	- LongMessageBuffer usage in percentage. This is an internal buffer used for passing messages within individual nodes and between nodes.

* **RedoBuffer Used**
	- RedoBuffer usage in percentage. RedoBuffer sets the size of the buffer in which the REDO log is written.

* **RedoLog Used**
	- RedoLog usage in percentage.
	
* **Uptime**
	- MySQL NDB service uptime.

* **Last Updated**
	- The last time ClusterControl fetch for node's status
	
* **Refresh**
	- Fetch the latest update.

.. _MySQL - Overview - Hosts Stats:

Hosts Stats
+++++++++++

Shows collected host metrics in a grid as below:

* **Ping(us)**
	- Ping round trip time (RTT) from ClusterControl host in microseconds.

* **CPU Util/Steal**
	- Total of CPU utilization in percentage.

* **Loadavg 1/5/15**
	- Load value captured for 1, 5 and 15 minutes average.

* **Net (tx/s / rx/s)**
	- Amount of data transmitted and received by the host.

* **Disk Read/sec**
	- Amount of disk read of ``monitored_mountpoint``.

* **Disk Writes/sec**
	- Amount of disk write of ``monitored_mountpoints``.

* **Uptime**
	- Host uptime.

* **Last Updated**
	- The last time ClusterControl fetch for host's status.
