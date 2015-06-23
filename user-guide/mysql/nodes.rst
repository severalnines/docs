.. _mysql-nodes:

Nodes
-----

Provides detailed information for each node in the cluster. On the left hand column, you can find a list of all nodes that are members of the cluster including ClusterControl node. If you added slaves, HAProxy, Keepalived or :term:`Garbd` (Galera Arbitrator) to your cluster through ClusterControl, these will also be listed.


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
	- Database related logs. For MySQL standalone, replication and Galera, it shows MySQL error log and xtrabackup related log. For MySQL Cluster, it shows log relative to the role e.g MySQL error logs, cluster logs and data node logs.

* **DB Performance**
	- Overview of choosen database performance counters shown in graph.
	
* **DB Status**
	- MySQL status for this node, similar to ``SHOW STATUS`` command.

* **DB Variables**
	- MySQL variables for this node, similar to ``SHOW VARIABLES`` command.


Nodes Management
````````````````

The remove icon will only appear when you rollover the mouse pointer on the node icon in the left-hand column. This removes the database node from the cluster. Some of the node management jobs are cluster-specific, as described in the next sections.

.. Note:: You can monitor job's progress at *ClusterControl > Logs > Jobs*.

Galera Cluster
''''''''''''''

These are specific functions available for Galera nodes:

* **Shutdown Node**
	- Stops the database instance on this node. This is not a system shut down.
	
* **Reboot Host**
	- Initiates a system reboot on this host.

* **Bootstrap Cluster**
	- Launches the bootstrap cluster window. Similar to *ClusterControl > Actions > Bootstrap Cluster*. ClusterControl will stop all running nodes before bootstrapping the cluster from the selected Galera node.

* **Rebuild Replication Slave**
	- Rebuilds replication slave on this node from another master. This is only relevant if you have setup a replication slave for the cluster and you want to resync the data. It uses Percona Xtrabackup to stage the replication data.

.. caution:: 'Rebuilding Replication Slave' will wipe out the selected node's MySQL datadir content.

* **Start Node**
	- This option is only available if the node is down. It starts the database instance on this node. If you tick 'Perform an initial start?', it will remove all files in the MySQL datadir and force a full resync (SST), which is necessary sometimes if the Galera node fails to reach a synced state after multiple node recovery attempts and there is a filesystem issue.
	
* **Make Primary**
	- This option is only available if the node is down. It makes sense to use it if the Galera node is down and reported as :term:`non-Primary component` from the *Overview* page. ClusterControl will attempt to promote the node from non-Primary state to :term:`Primary component`.

MySQL Cluster
'''''''''''''

These are specific functions available for MySQL cluster nodes:

* **Shutdown Node**
	- Stops the database instance on this node. This is not a system shut down.
	
* **Reboot Host**
	- Initiates a system reboot on this host.
	
* **Start Node**
	- This option is only available if the node is down. It starts the database instance on this node.

MySQL replication
'''''''''''''''''

These are specific functions available for MySQL replication nodes:

* **Shutdown Node**
	- Stops the database instance on this node. This is not a system shut down.
	
* **Reboot Host**
	- Initiates a system reboot on this host.
	
* **Start Node**
	- This option is only available if the node is down. It starts the database instance on this node.

* **Rebuild Replication Slave**
	- Rebuilds replication slave on this node from another master. It uses Percona Xtrabackup to stage the replication data.
	
.. caution:: 'Rebuilding Replication Slave' will wipe out the selected node's MySQL datadir content.
	
* **Stop Slave**
	- Stops the slave thread.

MySQL single
''''''''''''

These are specific functions available for MySQL standalone nodes:

* **Shutdown Node**
	- Stops the database instance on this node. This is not a system shut down.
	
* **Reboot Host**
	- Initiates a system reboot on this host.
	
* **Start Node**
	- This option is only available if the node is down. It starts the database instance on this node.
