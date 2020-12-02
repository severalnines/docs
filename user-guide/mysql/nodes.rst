.. _MySQL - Nodes:

Nodes
-----

Provides detailed information for each node in the cluster. On the left hand column, you can find a list of all nodes that are members of the cluster including ClusterControl node. If you added slaves, :term:`HAProxy`, :term:`Keepalived`, :term:`MaxScale`, :term:`ProxySQL` or :term:`Garbd` to your cluster through ClusterControl, these will also be listed.

.. _MySQL - Nodes - Node Monitoring:

Node Monitoring
++++++++++++++++

.. Note:: To learn more about how ClusterControl monitors the hosts, see :ref:`Components - ClusterControl Controller - Monitoring Operations`.

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

* **Clear Queries**
  - Resets the top query list. This is equal to run ``SELECT * FROM stats_mysql_query_digest_reset LIMIT 1;`` inside ProxySQL admin interface.

For each query, there are menu if you roll over on the row:

* **Create Rule**
	- Create a query rule for the selected query into ProxySQL. This will open a pop-up dialog for you to fine tune the query rule before saving it into ProxySQL. By default, Clustercontrol will auto-fill two text fields - *Match Pattern* and *Match Digest*. However, you can only choose to save only one of the field into ProxySQL.

* **Cache Query**
	- Cache the selected query into ProxySQL. This will open a pop-up dialog for you to fine tune the query rule before saving it into ProxySQL.

* **Full Digests**
	- Show full digest statement.

Rules
'''''

List out all query rules created under this ProxySQL instance.

* **Add New Rule**
	- Creates a new query rule. Details at ProxySQL `MySQL query rules <https://github.com/sysown/proxysql/wiki/Main-(runtime)#mysql_query_rules>`_.

* **Edit**
	- Edit an existing query rule. This will expand a dialog for you to fine tune the query rule before updating it into ProxySQL.

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
	- Use SSL: Use SSL to the backend server. Details at `ProxySQL SSL Support <https://github.com/sysown/proxysql/wiki/SSL-Support>`_.
	- Use Compression: Use compression to the backend server.

* **Host Groups**
	- List of hostgroups created under this service. 
	- Click on 'Edit' to edit the server details like hostgroup id, weight, max replication lag and so on.
	- Click on 'Remove' to delete the selected server.

* **ProxySQL Cluster**
	- List of ProxySQL nodes belong to this ProxySQL cluster. 
	- Only available if you enabled ProxySQL native clustering.

Users
'''''

List out all users created under this ProxySQL.

* **Import Users**
	- Opens the import wizard. ClusterControl will list out MySQL users retrieved from the database cluster. Check the boxes that you would like to import and click *Next*. In the next stage, you have to specify the default hostgroup for the selected users before instructing ClusterControl to start importing those users.

* **Add New User**
	- Creates a new user for the ProxySQL instance as well as the backend MySQL server. 

* **Edit**
	- Edit the selected user. This will expand a dialog for you to fine tune the user details before updating it into ProxySQL.

* **Drop User**
	- Drop the selected user.

Variables
'''''''''

Lists out all ProxySQL variables for this instance. You can filter the variables using the lookup field. Details at `ProxySQL Global Variables <https://github.com/sysown/proxysql/wiki/Global-variables>`_.

Scheduler Scripts
'''''''''''''''''

Lists out scheduler script, commonly being configured if you are running ProxySQL on top of Galera Cluster. Scheduler is a cron-like implementation integrated inside ProxySQL with millisecond granularity. Details at `ProxySQL Scheduler <https://github.com/sysown/proxysql/wiki/Scheduler>`_.

Node Performance
''''''''''''''''

Provides a summary of host information and statistic histogram including CPU, disk, network and memory usage.

Process List
''''''''''''''''

Lists out ProxySQL process list, similar to the output of ``select * from stats_mysql_processlist``. This can be useful for troubleshooting process and make sure the query is routed properly to the respective hostgroup in real time.

Prometheus
``````````

Provides detailed view of ProxySQL stats. ClusterControl connects to the ProxySQL admin interface to retrieve the stats and visualize them here.

Exporters
'''''''''

Lists out exporter jobs per host. A green exporter means the exporter is working correctly.

Settings
'''''''''

Shows the Prometheus settings.


.. _MySQL - Nodes - Node Actions:

Node Actions
++++++++++++

SSH Console
````````````

Opens a web-based SSH terminal in a new browser window that allows to execute shell commands on the server directly from a browser as the configured ``os_user``. This feature only supported with Apache 2.4+ with a running ``cmon-ssh`` daemon. For more details, on this component, see :ref:`Components - ClusterControl SSH`.

Schedule Maintenance Mode
``````````````````````````

Puts individual nodes into maintenance mode which prevents ClusterControl to raise alarms and notifications during the maintenance period. When toggling ON, you can set the maintenance period for a pre-defined time or schedule it accordingly. Specify the reason for auditing purpose. ClusterControl will not degrade the node, hence the node's state remains as what it is unless you perform any maintenance onto it. 

Alarms and notifications for this node will be activated back once the maintenance period is exceeded, or you explicitly toggling it OFF.

.. Attention::  If node autorecovery is enabled, ClusterControl will always recover a node regardless of the maintenance mode status. Don't forget to disable node autorecovery to avoid ClusterControl interfering your maintenance tasks.

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

.. _MySQL - Nodes - Cluster-Specific Node Actions:

Cluster-specific Node Actions
+++++++++++++++++++++++++++++

Some of the node management feature set are built for a particular cluster, as described in the next sections.

Galera Cluster
``````````````

These are specific options available for Galera nodes:

* **Resync Node**
	- Removes all files in the datadir on this node and forces a full resync any existing full backup or SST from a donor. The former is recommended to bring the joiner node to the closest point and gain the probability of IST which is a non-blocking operation. This is necessary sometimes if the Galera node is trying to recover multiple times and there is for example a filesystem issue. Wait for its completion before starting another node with *Initial Start*.

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
	- Updates the related configurations on this host to enable binary logging. A replication slave can then be added to the node, or it may be possible to use the binary log for point-in-time recovery (PITR). See :ref:`MySQL - Backup - Restore Backup - Point-in-Time Recovery` for details. A server restart is needed to finalize the configuration update.

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

* **Disable Readonly**
  - Disable read-only by setting up ``SET GLOBAL read_only = OFF``. This option is only available if read-only is on.

* **Enable Readonly**
  - Enable read-only by setting up ``SET GLOBAL read_only = ON``. This option is only available if read-only is off.

* **Promote Slave**
	- Exclusive for slave node. Promotes the selected slave to become the new master. If the master is currently functioning correctly, then stop application queries prior to promoting another slave to safe guard from data loss. Connections on the current running master will be killed after a 10 second grace period.
	
* **Start Slave**
	- Exclusive for slave node and only if the slave is stopped. It starts the slave thread.

* **Stop Slave**
	- Exclusive for slave node and only if the slave is started. Stops the slave IO and SQL threads.

* **Rebuild Replication Slave**
	- Exclusive for slave nodes. Rebuilds replication slave on this node from another master. It can use an existing backup (PITR compatible) or use Percona Xtrabackup to stage the replication data from a master.
	
.. caution:: *Rebuilding Replication Slave* will wipe out the selected node's MySQL datadir.

* **Change Replication Master**
	- Exclusive for slave nodes. This option will tell ClusterControl to change the replication master to the other available master. All slaves will then be configured to replicate from the new master.

* **Reset Slave**
	- Exclusive for slave node. Make the slave forget its replication position in the master's binary log. This is similar to running a ``RESET SLAVE`` command on the slave. The slave must be stopped in order for this feature to work.

* **Reset Slave All**
	- Exclusive for slave node. Make the slave forget its replication position in the master's binary log, as well as any replication connection parameters such as master host, master port, master user, or master password. This is similar to running a ``RESET SLAVE ALL`` command on the slave. The slave must be stopped in order for this feature to work.

MySQL Standalone
``````````````````

These are specific options available for MySQL standalone nodes:

* **Enable Binary Logging**
  - Updates the related configurations on the node to enable binary logging. A replication slave can then be added to the node, or it may be possible to use the binary log for point-in-time recovery (PITR). See :ref:`MySQL - Backup - Restore Backup - Point-in-Time Recovery` for details. A server restart is needed to finalize the configuration update.

* **Disable Read Only**
  - Disables read-only by setting up ``SET GLOBAL read_only = OFF``. This option is only available if read-only is on.

* **Enable Read Only**
  - Enables read-only by setting up ``SET GLOBAL read_only = ON``. This option is only available if read-only is off.

ProxySQL
`````````

The following are specific options available for ProxySQL nodes:

* **Sync Instances**
	- Synchronizes a ProxySQL configuration with other instances to keep them identical. You can perform syncing operation (export & import), export (backup) or import (restore) of ProxySQL configurations. 
	- For export (backup), the configuration data will be exported into several SQL dump files where applicable. The following configuration data will be exported: 
		- Query Rules
		- Host Groups/Servers
		- Users and corresponding MySQL users
		- Global Variables
		- Scheduler
		- proxysql.cnf
	- For import (restore), the existing configuration  will be overwritten.

Failover, Switchover, Topology Changes and Recovery
+++++++++++++++++++++++++++++++++++++++++++++++++++

ClusterControl performs failover, switchover and recovery procedures are based on the cluster topology that user has set up. Since MySQL can be running in hybrid replication mode e.g, a three-node Galera Cluster with two asynchronous replication slaves attached to it.

Galera Cluster Recovery
```````````````````````

Node Recovery
'''''''''''''

In Galera Cluster, all nodes are equal - each node holds the same role and same dataset. Therefore, there is no failover within the cluster if a node fails. Only the application side requires failover, to skip the inoperational nodes while the cluster is partitioned. Therefore, it's highly recommended to place load balancers on top of a Galera Cluster to:

* Unify the multiple database endpoints to a single endpoint (load balancer host or virtual IP address as the endpoint).
* Balance the database connections between the backend database servers.
* Perform health checks and only forward the database connections to healthy nodes.
* Redirect/rewrite/block offending (badly written) queries before they hit the database servers.

For Galera Cluster, ClusterControl supports HAProxy, MariaDB MaxScale and ProxySQL. ClusterControl also supports virtual IP address implementation through Keepalived. If having load balancer is not an option, ensure your applications are aware of this topology changes and redirect the request to the healthy node accordingly. There are a number of MySQL connectors come with built-in automatic failover like php-mysqlnd_ms for PHP and MySQL Connector/J for Java.

For node recovery, if the cluster loses minority of the nodes at one time, the majority of nodes will be very likely to remain operational, thanks to Galera quorum calculation and group communication. When the problematic node comes back up, the node will re-establish group communication with the operational nodes and automatic syncing operation will take place before the node is allowed to join the cluster. In simple words, node recovery process is handled automatically by Galera. Nevertheless, ClusterControl will still oversee this recovery process and notify users on the status and progress. ClusterControl automatic node recovery will only kick-in if Galera automatic recovery fails.

Cluster Recovery
''''''''''''''''

A cluster is deemed as failure if all nodes or majority of the nodes go offline without graceful shut down. Offline in this context means they are not able to see each other through Galera's replication traffic or group communication. Example of cluster failure likes power trip against all or majority of the nodes, MySQL/MariaDB or Galera software crash due to bugs or shared-storage failure. If total failure happens, bootstrap is the only way to go.

In case of network glitch, Galera will always attempt to recover a partitioned cluster once the network issue is resolved. Galera will automatically re-establish the communication between members, exchange node's states and determine the possibility of reforming the primary component by comparing node state, UUIDs and seqnos. If the probability is there, Galera will merge the primary components and cluster can resume in operational state without any intervention. Otherwise, you have to promote at least one of the node to become a primary component or re-bootstrap the cluster.

To re-bootstrap a cluster, pick one node (usually the most advanced node by looking at the highest ``wsrep_committed`` value) and then go to *Cluster Action > Bootstrap Cluster > Bootstrap Node*. ClusterControl will then perform the cluster bootstrapping process from the chosen node. You can choose *Auto-Select* from the dropdown in case you can't figure out which node is the most advanced node. ClusterControl will always try to bootstrap the most up-to-date node if this option is selected.

Otherwise, you can run the following command on the most advanced node to simply promote it again as a primary component:

.. code-block:: mysql

	> SET GLOBAL 'wsrep_provider_options="pc.bootstrap=1"'

To learn more above Galera Cluster recovery when network partitioning happens, check out this blog post, `Galera Cluster Recovery 101 - A Deep Dive into Network Partitioning <https://severalnines.com/blog/galera-cluster-recovery-101-deep-dive-network-partitioning>`_.

Asynchronous Cluster Recovery
'''''''''''''''''''''''''''''

On the other hand, it's also possible to have asynchronous replication between two Galera Clusters. This should be handled differently and we have covered the failover and failback procedures in this blog post, `Asynchronous Replication Between MySQL Galera Clusters - Failover and Failback <https://severalnines.com/blog/asynchronous-replication-between-mysql-galera-clusters-failover-and-failback>`_.

MySQL Replication Master Failover
``````````````````````````````````

In MySQL Replication, the process of promoting a replica (slave) to become a master after the old master has failed is called failover. On the other hand, "switchover" happens when the user triggers the promotion of the replica. A new master is promoted from a replica pointed by the user and the old master, typically, becomes a replica to the new master.

ClusterControl applies industry best practices to make sure that the failover process is performed correctly. It also ensures that the process will be safe - default settings are intended to abort the failover if possible issues are detected. Those settings can be overridden by the user should they want to prioritize failover over data safety. Take note that ClusterControl will only perform automatic recovery only if auto recovery is toggled on.

When recovering a master failure for MySQL Replication, ClusterControl will perform the following procedures:

1) Once a master failure has been detected by ClusterControl, a failover process is initiated and the first failover hook, ``replication_onfail_failover_script`` is immediately executed.

2) Next, master availability is tested. ClusterControl does extensive tests to make sure the master is indeed unavailable. This behavior is enabled by default and it is managed by the following variable:

	* ``replication_check_external_bf_failover`` - Before attempting a failover, perform extended checks by checking the slave status to detect if the master is truly down, and also check if ProxySQL (if installed) can still see the master. If the master is detected to be functioning, then no failover will be performed. Default is 1 meaning the checks are enabled.

3) Next step is to determine which host can be used as a master candidate. ClusterControl does check if a whitelist or a blacklist is defined. You can do that by using the following variables in the cmon configuration file:

	* ``replication_failover_blacklist`` - Comma separated list of hostname:port pairs. Blacklisted servers will not be considered as a candidate during failover. This variable is ignored if ``replication_failover_whitelist`` is set.
	* ``replication_failover_whitelist`` - Comma separated list of hostname:port pairs. Only whitelisted servers will be considered as a candidate during failover. If no server on the whitelist is available (up/connected) the failover will fail.

* Optionally, it is also possible to configure ClusterControl to look for differences in binary log filters across all replicas (as some slaves can be configured to replicate a subset of data from master). It can be done using ``replication_check_binlog_filtration_bf_failover`` variable. By default, those checks are disabled. ClusterControl also verifies there are no errant transactions in place, which could cause issues.

4) Afterwards, the second script is executed: it is defined in ``replication_pre_failover_script`` setting. Next, a master candidate undergoes preparation process. ClusterControl waits for redo logs to be applied (ensuring that data loss is minimal). It also checks if there are other transactions available on remaining replicas, which have not been applied to master candidate. Both behaviors can be controlled by the user, using the following settings in cmon configuration file:

	* ``replication_skip_apply_missing_txs`` - Force failover/switchover by skipping applying transactions from other slaves. Default is disabled. 1 means enabled.
	* ``replication_failover_wait_to_apply_timeout`` - Candidate waits up to this many seconds to apply outstanding relay log (retrieved_gtids) before failing over. Default is -1 (wait forever). 0 means failover immediately.

5) Finally, the master is elected and the last script is executed (a script which can be defined under ``replication_post_failover_script`` variable). This script will perform ``CHANGE MASTER`` statement on all replicas to the new elected master. If a slave encounters error during this operation, by default, ClusterControl won't automatically rebuild the slave. You can instruct ClusterControl to auto-rebuild replicas which cannot replicate from the new master using following setting in CMON configuration file:

	* ``replication_auto_rebuild_slave`` - If the SQL THREAD is stopped and error code is non-zero then the slave will be automatically rebuilt. 1 means enable, 0 means disable (default).

* MySQL Event can also be enabled immediately on the new master by using the following variable:

	* ``replication_failover_events`` - Automatically failover events and enable the ``event_scheduler`` variable on the new master after a replication failover/switchover action. Default is disabled. Enable it by setting to 1.
	
.. Note:: All of the configuration options mentioned in this section can be configured under ``/etc/cmon.d/cmon_X.cnf``, where X is the cluster ID of the MySQL Replication.

For more info on how to control the replication failover behavior performed by ClusterControl (whitelist/blacklist configuration, determine a good/bad master candidate, etc), check out this blog post, `How to Control Replication Failover for MySQL and MariaDB <https://severalnines.com/blog/how-control-replication-failover-mysql-and-mariadb>`_. For a complete list of configuration options related to MySQL Replication, see `ClusterControl Controller Configuration Options  <../../components.html#configuration-options>`_.

Reverse Proxies with Keepalived
````````````````````````````````

To eliminate single point of failure (SPOF) in the load balancer tier, redundant reverse proxy is one of the ways to go. That's why in order to deploy Keepalived using ClusterControl, you need two or more load balancers installed by or imported into ClusterControl. Keepalived will be used to tie load balancers together with a floating IP address in an active-passive mode, where the active node is the one that holding the virtual IP address at one given time.

For production usage, we highly recommend the load balancer software to be running on a standalone host and not co-located with your database nodes. Check out this blog post, `How ClusterControl Configures Virtual IP and What to Expect During Failover <https://severalnines.com/blog/how-clustercontrol-configures-virtual-ip-and-what-expect-during-failover>`_ for more info on how ClusterControl deploys Keepalived and what happens during failover.