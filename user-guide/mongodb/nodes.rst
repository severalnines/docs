Nodes
-----

Provides detailed information for each node in the cluster. On the left hand column, you can find a list of all nodes that are members of the cluster including ClusterControl node. If you added replica set members, mongos, config servers, shard servers or MongoDB :term:`arbiter` to your cluster through ClusterControl, these will also be listed.


Nodes Monitoring
++++++++++++++++


The node on the list will appear in red colour to indicate it is unhealthy. The tabs show performance and resource usage for a specific node. There are also database specific tabs depending on the type of database running on the host.

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

* **Overview**
	- Provides a summary of host information and statisic histogram including CPU, disk, network and memory usage.

* **Top**
	- Provides a snapshot view of processes running on the host, similar to :term:`top` command in Linux.
	
* **Logs**
	- MongoDB database related log files.

* **DB Performance**
	- Overview of database performance counters and Mongo Stats, similar to :term:`mongostat` command.

Nodes Management
++++++++++++++++

SSH Console
````````````

Opens a web-based SSH terminal in a new browser window that allows to execute shell commands on the server directly from a browser as the configured ``os_user``. This feature only supported with Apache 2.4+ with ClusterControl SSH component is installed and service ``cmon-ssh`` is started. Details at `ClusterControl SSH <../../components.html#clustercontrol-ssh>`_.

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

Start Node
``````````

Starts the monitored process of the selected host. For example, if the node's role is HAProxy, ClusterControl will restart HAProxy process. Only available if the service is stopped. 

Unregister Node
```````````````

Removes the database node from the database cluster and/or ClusterControl monitoring. You can choose one of the these three options:

* *Keep the service running* - Node will be unregistered from ClusterControl but the service will be kept running. This node will remain part of the database cluster.
* *Stop service and keep files untouched* - Node will be unregistered from ClusterControl and the service will be stopped. Data files and configuration files will be left intact on the server. The node will be down, but would be part of the database cluster if started.
* *Stop and uninstall service (all configuration files will be deleted)* - Node will be unregistered from ClusterControl and the service will be stopped. Data files and configuration files will be deleted on the server. The monitored service will be disabled to prevent accidental restarts.


Cluster-Specific Nodes Management
+++++++++++++++++++++++++++++++++

Some of the node management jobs are cluster-specific, as described in the next sections.

.. Note:: You can monitor the job's progress at *ClusterControl > Logs > Jobs*.

Replica Set
````````````

These are specific functions available for replica set nodes:

* **Step Down Node**
	- Only for primary replica set node. The host stops being a primary and becomes a secondary and is not eligible to become a primary for a set number of seconds. The nodes in the MongoDB replicaSet with voting power, will elect a new primary with the stepped down primary excluded for the set number of seconds.

* **Freeze Node**
	- Prevents a replica set member from seeking election for the specified number of seconds. If you want to unfreeze a replica set member before the specified number of seconds has elapsed, you can issue the command with a seconds value of 0.

Sharded Cluster
````````````````

These are specific functions available for sharded cluster nodes:
	
* **Step Down Node**
	- Only for primary replica set node. The host stops being a primary and becomes a secondary and is not eligible to become a primary for a set number of seconds. The nodes in the MongoDB replicaSet with voting power, will elect a new primary with the stepped down primary excluded for the set number of seconds.
	
* **Freeze Node**
	- Prevents a replica set member from seeking election for the specified number of seconds. If you want to unfreeze a replica set member before the specified number of seconds has elapsed, you can issue the command with a seconds value of 0.
