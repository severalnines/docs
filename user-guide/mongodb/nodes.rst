.. _mongodb-nodes:

Nodes
-----

Provides detailed information for each node in the cluster. On the left hand column, you can find a list of all nodes that are members of the cluster including ClusterControl node. If you added replica set members, mongos, config servers, shard servers or MongoDB :term:`arbiter` to your cluster through ClusterControl, these will also be listed.


Nodes Monitoring
````````````````

The node on the list will appear in red colour to indicate it is unhealthy. The tabs show performance and resource usage for a specific node. There are also database specific tabs depending on the type of database running on the host:

* **Overview**
	- Provides a summary of host information and statisic histogram including CPU, disk, network and memory usage.
	- Database node status indicator:

  =========== ===========
  Status      Description
  =========== ===========
  OK          Indicates the node is working fine.
  WARNING     Indicates the node is degraded and not performing as expected.
  PROBLEMATIC Indicates the node is down or unreachable.
  =========== ===========

* **Top**
	- Provides a snapshot view of processes running on the host, similar to :term:`top` command in Linux.
	
* **Logs**
	- MongoDB database related log files.

* **DB Performance**
	- Overview of database performance counters and Mongo Stats, similar to :term:`mongostat` command.

Nodes Management
````````````````

Remove Node
''''''''''''

The remove icon will only appear when you rollover the mouse pointer on the node icon in the left-hand column. This removes the database node from the cluster.

Maintenance Mode
'''''''''''''''''

Puts individual nodes into maintenance mode which prevents ClusterControl to raise alarms and notifications during the maintenance period. When toggling ON, you can set the maintenance period for a pre-defined time or schedule it accordingly. Specify the reason for auditing purpose. ClusterControl will not degrade the node, hence the node's state remains as what it is unless you perform any maintenance onto it. 

Alarms and notifications for this node will be activated back once the maintenance period is exceeded, or you explicitly toggling it OFF.

Cluster-Specific Nodes Management
``````````````````````````````````

Some of the node management jobs are cluster-specific, as described in the next sections.

.. Note:: You can monitor job's progress at *ClusterControl > Logs > Jobs*.

Replica Set
'''''''''''

These are specific functions available for replica set nodes:

* **Shutdown Node**
	- Stops the database instance on this node. This is not a system shut down.
	
* **Reboot Host**
	- Initiates a system reboot on this host.

* **Start Node**
	- This option is only available if the node is down. It starts the database instance on this node.
	
* **Reboot Host**
	- Initiates a system reboot on this host.

* **Step Down Node**
	- The host stops being a primary and becomes a secondary and is not eligible to become a primary for a set number of seconds. The nodes in the MongoDB replicaSet with voting power, will elect a new primary with the stepped down primary excluded for the set number of seconds.

* **Freeze Node**
	- Prevents a replica set member from seeking election for the specified number of seconds. If you want to unfreeze a replica set member before the specified number of seconds has elapsed, you can issue the command with a seconds value of 0


Sharded Cluster
'''''''''''''''

These are specific functions available sharded cluster:

* **Shutdown Node**
	- Stops the database instance on this node. This is not a system shut down.
	
* **Reboot Host**
	- Initiates a system reboot on this host.
	
* **Start Node**
	- This option is only available if the node is down. It starts the database instance on this node.
	
* **Reboot Host**
	- Initiates a system reboot on this host.
	
* **Step Down Node**
	- The host stops being a primary and becomes a secondary and is not eligible to become a primary for a set number of seconds. The nodes in the MongoDB replicaSet with voting power, will elect a new primary with the stepped down primary excluded for the set number of seconds.
	
* **Freeze Node**
	- Prevents a replica set member from seeking election for the specified number of seconds. If you want to unfreeze a replica set member before the specified number of seconds has elapsed, you can issue the command with a seconds value of 0
