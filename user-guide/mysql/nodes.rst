Nodes
-----

Provides detailed information for each node in the cluster. On the left hand column, you can find a list of all nodes that are members of the cluster including ClusterControl node. If you added slaves, :term:`HAProxy`, :term:`Keepalived`, :term:`MaxScale`, :term:`ProxySQL` or :term:`Garbd` to your cluster through ClusterControl, these will also be listed.


Nodes Monitoring
++++++++++++++++

The node on the list will appear in red color to indicate it is unhealthy. The tabs show performance and resource usage for a specific node. There are also database specific tabs depending on the type of database running on the host.

Database node status indicator:

=========== ===========
Status      Description
=========== ===========
OK          Indicates the node is working fine.
WARNING     Indicates the node is degraded and not performing as expected.
MAINTENANCE Indicates the node is under maintenance.
PROBLEMATIC Indicates the node is down or unreachable.
=========== ===========

Controller Node
````````````````

* **Overview**
	- Provides a summary of host information and statistic histogram including CPU, disk, network and memory usage.

* **Top**
	- Provides a snapshot view of processes running on the host, similar to :term:`top` command in Linux.

Database Nodes
``````````````

* **Overview**
	- Provides a summary of host information and statistic histogram including CPU, disk, network and memory usage.

* **Top**
	- Provides a snapshot view of processes running on the host, similar to :term:`top` command in Linux.
	
* **Logs**
	- Database related logs. For MySQL standalone, replication and Galera, it shows the MySQL error log and xtrabackup related log. For MySQL Cluster, it shows all logs relative to the role e.g MySQL error logs, cluster logs and data node logs.

* **DB Performance**
	- Overview of chosen database performance counters shown in graph.
	
* **DB Status**
	- MySQL status for this node, similar to ``SHOW STATUS`` command.

* **DB Variables**
	- MySQL global variables for this node, similar to ``SHOW GLOBAL VARIABLES`` command.
	
HAProxy Nodes
``````````````

Provides detailed view of HAProxy stats, similar to HAProxy stats page. If HAProxy is deployed using ClusterControl, ClusterControl will automatically create HAProxy stats page on port 9600. You can access the page directly using the value of *Admin User* and *Admin Password* specified during the deployment at *ClusterControl > Manage > Load Balancer > Install HAProxy > Show Advanced Settings*.

* **Stats URL**
	- If you are using HAProxy 1.6 and newer, use another URL: :samp:`http://{HAproxy_host}:{HAproxy_admin_port}/haproxy?stats;csv/`

* **Update**
	- Updates the HAProxy stats URL.
	
* **Enabled**
	- Tick on the checkbox to enable or disable the backend server from the load balancing set. This is useful during backend servers maintenance.

ProxySQL Nodes
``````````````

Provides detailed view of ProxySQL stats. ClusterControl connects to the ProxySQL admin interface to retrieve the stats and visualize them here.

Monitor
'''''''

* **ProxySQL Host Groups**
	- List of hostgroups created under this service.
	- It also provides the status of hosts in all defined hostgroups. It shows metrics related to hostgroups - used connections, free connections, errors, number of queries executed, amount of data sent and received and latency in microseconds.
	
* **ProxySQL Stats**
	- Graphs related to ProxySQL metrics - active transactions, data sent and received, memory utilization, number of connections and many more. This gives you insight in how ProxySQL operates and helps to catch any potential issues with the proxy layer.

Top Queries
'''''''''''

List of queries digested by the ProxySQL instance.

Rules
'''''

List out all query rules created under this ProxySQL instance.

* **Add New Rule**
	- Details at ProxySQL MySQL query rules `wiki site <https://github.com/sysown/proxysql/wiki/MySQL-Query-Rules>`_.

* **Edit**
	- Edit an existing query rule.

* **Delete**
	- Delete an existing query rule.

Servers
'''''''

List out all backend servers created under this ProxySQL instance.

* **Add Server**
	- Host Details: Specify hostname or IP address with MySQL port of the backend server. The server must be provisioned by ClusterControl server.
	- Hostgroup Id: Assign a hostgroup identifier number.
	- Weight: Server weight when balancing.
	- Max Replication Lag: Specify how many seconds ProxySQL should tolerate a lagging slave as healthy.
	- Max Connection: Specify maximum number of connections allowed to access this backend server.
	- Max Latency(ms): Specify maximum latency in microseconds 
	- Use SSL: Use SSL to the backend server. Details at `ProxySQL SSL documentation <https://github.com/sysown/proxysql/wiki/SSL-configuration>`_.
	- Use Compression: Use compression to the backend server.

* **Host Groups**
	- List of hostgroups created under this service. 
	- Click on 'Edit' to edit the server details like hostgroup id, weight, max replication lag and so on.
	- Click on 'Remove' to delete the selected server.

Users
'''''

List out all users created under this ProxySQL.

* **Add New User**
	- Create a new user or add an existing user created on the backend MySQL server.

* **Edit**
	- Edit the selected user.

* **Drop User**
	- Drop the selected user.

Variables
'''''''''

List all ProxySQL variables for this instance. You can filter the variables using the lookup field.

Node Actions
++++++++++++

SSH Console
````````````

Opens a web-based SSH terminal in a new browser window that allows to execute shell commands on the server directly from a browser as the configured ``os_user``. This feature only supported with Apache 2.4+ with ClusterControl SSH component is installed and service ``cmon-ssh`` is started. Details at `ClusterControl SSH <../../components.html#clustercontrol-ssh>`_.

Schedule Maintenance Mode
``````````````````````````

Puts individual nodes into maintenance mode which prevents ClusterControl to raise alarms and notifications during the maintenance period. When toggling ON, you can set the maintenance period for a pre-defined time or schedule it accordingly. Specify the reason for auditing purpose. ClusterControl will not degrade the node, hence the node's state remains as what it is unless you perform any maintenance onto it. 

Alarms and notifications for this node will be activated back once the maintenance period is exceeded, or you explicitly toggling it OFF.

Reboot Host
````````````

Initiates a system reboot of the selected host. Once initiated, ClusterControl will monitor the reboot progress every 5 seconds for 10 minutes (600 seconds) before declaring the reboot operation is failed.

Restart Node
````````````

Restarts the active monitored process of the selected host. For example, if the node's role is HAProxy, ClusterControl will restart HAProxy process. This is not a system reboot. Only available if the service is started. 

You can configure the graceful shutdown timeout (default is 1800 seconds) in the "Confirm Shutdown" dialog. ClusterControl will give up waiting for a node to gracefully terminate. If the node is still running after the timeout you may send the SIGKILL signal to force the node down by toggling on 'Force stop (SIGKILL) node after the graceful shutdown timeout has been reached' option.

The node will be shutdown and enter maintenance mode.

Stop Node
``````````

Stops the monitored process of the selected host. For example, if the node's role is HAProxy, ClusterControl will restart HAProxy process. This is not a system shut down. Only available if the service is started. 

You can configure the graceful shutdown timeout (default is 1800 seconds) in the "Confirm Shutdown" dialog. ClusterControl will give up waiting for a node to gracefully terminate. If the node is still running after the timeout you may send the SIGKILL signal to force the node down by toggling on 'Force stop (SIGKILL) node after the graceful shutdown timeout has been reached' option.

The node will be shutdown and enter maintenance mode.

Unregister Node
````````````````

Removes the database node from the database cluster and/or ClusterControl monitoring. You can choose one of the these three options:

* *Keep the service running* - Node will be unregistered from ClusterControl but the service will be kept running. This node will remain part of the database cluster.
* *Stop service and keep files untouched* - Node will be unregistered from ClusterControl and the service will be stopped. Data files and configuration files will be left intact on the server. The node will be down, but would be part of the database cluster if started.
* *Stop and uninstall service (all configuration files will be deleted)* - Node will be unregistered from ClusterControl and the service will be stopped. Data files and configuration files will be deleted on the server. The monitored service will be disabled to prevent accidental restarts.

Cluster-Specific Node Actions
+++++++++++++++++++++++++++++

Some of the node management jobs are cluster-specific, as described in the next sections.

.. Note:: You can monitor the job's progress at *ClusterControl > Logs > Jobs*.

Galera Cluster
``````````````

These are specific options available for Galera nodes:

* **Resync Node**
	- Removes all files in the datadir and forces a full resync (SST) of the node. This is necessary sometimes if the galera node is trying Node Recovery multiple times and there is e.g., a filesystem issue. Wait for its completion before starting another node with Initial Start.

* **Bootstrap Cluster**
	- Launches the bootstrap cluster window. ClusterControl will stop all running nodes before bootstrapping the cluster from the selected Galera node.

* **Rebuild Replication Slave**
	- Rebuilds replication slave on this node from another master. This is only relevant if you have setup a replication slave for the cluster and you want to resync the data. It uses Percona Xtrabackup to stage the replication data.

.. caution:: *Rebuilding Replication Slave* will wipe out the selected node's MySQL datadir.

* **Start Node**
	- This option is only available if the node is down. It starts the database instance on this node. If you tick 'Perform an initial start?', it will remove all files in the MySQL datadir and force a full resync (SST), which is necessary sometimes if the Galera node fails to reach a synced state after multiple node recovery attempts and there is a filesystem issue.
	
* **Make Primary**
	- This option is only available if the node is down. It makes sense to use this if the Galera node is reported as non-Primary component from the *Overview* page. ClusterControl will attempt to promote the node from non-Primary state to :term:`Primary component`.
	
* **Enable Binary Logging**
	- Updates the related configurations on this host to enable binary logging. A replication slave can then be added to the node, or it may be possible to use the binary log for point-in-time recovery (PITR). See `Point-in-Time Recovery <backup.html#point-in-time-recovery>`_ for details. A server restart is needed to finalize the configuration update.
	
MySQL Group Replication
````````````````````````

* **Rebuild Node**
	- Rebuilds the node by streaming backup from a master node using Percona Xtrabackup. ClusterControl will automatically start the Group Replication once the rebuild job succeeds.
	
.. caution:: *Rebuild Node* will wipe out the node's MySQL datadir.

MySQL Cluster
``````````````

These are specific options available for MySQL cluster nodes:

* **Shutdown Node**
	- Stops the database instance on this node. This is not a system shut down.
	
* **Restart Node**
	- Stops and starts the database instance on this node. This is not a system reboot.

* **Reboot Host**
	- Initiates a system reboot on this host.
	
* **Start Node**
	- This option is only available if the node is down. It starts the database instance on this node.

MySQL Replication
``````````````````

These are specific options available for MySQL replication nodes:

* **Disable Read Only**
  - Disable read-only by setting up ``SET GLOBAL read_only = OFF``. This option is only available if read-only is on.

* **Enable Read Only**
  - Enable read-only by setting up ``SET GLOBAL read_only = ON``. This option is only available if read-only is off.

* **Change Replication Master**
	- Exclusive for slave nodes. This option will tell ClusterControl to change the replication master to the other available master. All slaves will then be configured to replicate from the new master.

* **Rebuild Replication Slave**
	- Exclusive for slave nodes. Rebuilds replication slave on this node from another master. It can use an existing backup (PITR compatible) or use Percona Xtrabackup to stage the replication data from a master.
	
.. caution:: *Rebuilding Replication Slave* will wipe out the selected node's MySQL datadir.

* **Start Slave**
	- Exclusive for slave node and only if the slave is stopped. It starts the slave thread.

* **Stop Slave**
	- Exclusive for slave node and only if the slave is started. Stops the slave IO and SQL threads.
    
* **Promote Slave**
	- Exclusive for slave node. Promotes the selected slave to become the new master. If the master is currently functioning correctly, then stop application queries prior to promoting another slave to safe guard from data loss. Connections on the current running master will be killed after a 10 second grace period.

MySQL Standalone
``````````````````

These are specific options available for MySQL standalone nodes:

* **Enable Binary Logging**
  - Updates the related configurations on the node to enable binary logging. A replication slave can then be added to the node, or it may be possible to use the binary log for point-in-time recovery (PITR). See `Point-in-Time Recovery <backup.html#point-in-time-recovery>`_ for details. A server restart is needed to finalize the configuration update.

* **Disable Read Only**
  - Disables read-only by setting up ``SET GLOBAL read_only = OFF``. This option is only available if read-only is on.

* **Enable Read Only**
  - Enables read-only by setting up ``SET GLOBAL read_only = ON``. This option is only available if read-only is off.
