.. _pgsql-nodes:

Nodes
-----

Provides detailed information for each node in the cluster. On the left hand column, you can find a list of all nodes that are members of the cluster including ClusterControl node. If you added slaves to your cluster through ClusterControl, these will also be listed.


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
	- Provides a snapshot view of processes running on the host, similar to ``top`` command in Linux.

* **DB Performance**
	- Overview of chosen database performance counters shown in graph.


Nodes Management
````````````````

The remove icon will only appear when you rollover the mouse pointer on the node icon in the left-hand column. This removes the database node from the cluster.

.. Note:: You can monitor job's progress at *ClusterControl > Logs > Jobs*.

These are specific functions available for PostgreSQL standalone and replication nodes:

* **Shutdown Node**
	- Stops the database instance on this node. This is not a system shut down.
	
* **Reboot Host**
	- Initiates a system reboot on this host.
	
* **Start Node**
	- This option is only available if the node is down. It starts the database instance on this node.
