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

Controller Node
'''''''''''''''

* **Overview**
	- Provides a summary of host information and statistic histogram including CPU, disk, network and memory usage.

* **Top**
	- Provides a snapshot view of processes running on the host, similar to :term:`top` command in Linux.

Database Nodes
'''''''''''''''

* **Overview**
	- Provides a summary of host information and statistic histogram including CPU, disk, network and memory usage.

* **Top**
	- Provides a snapshot view of processes running on the host, similar to :term:`top` command in Linux.
	
* **DB Performance**
	- Overview of chosen database performance counters shown in graph.

* **DB Variables**
	- PostgreSQL variables, similar to ``SHOW ALL`` command.

Nodes Actions
````````````````

The remove icon will only appear when you rollover the mouse pointer on the node icon in the left-hand column. This removes the database node from the cluster.

.. Note:: You can monitor job's progress at *ClusterControl > Activity > Jobs*.

SSH Console
'''''''''''

Opens a web-based SSH terminal in a new browser window that allows to execute shell commands on the server directly from a browser as the configured ``os_user``. This feature only supported with Apache 2.4+ with ClusterControl SSH component is installed and service ``cmon-ssh`` is started. Details at `ClusterControl SSH <../../components.html#clustercontrol-ssh>`_.

Schedule Maintenance Mode
''''''''''''''''''''''''''

Puts individual nodes into maintenance mode which prevents ClusterControl to raise alarms and notifications during the maintenance period. When toggling ON, you can set the maintenance period for a pre-defined time or schedule it accordingly. Specify the reason for auditing purpose. ClusterControl will not degrade the node, hence the node's state remains as what it is unless you perform any maintenance onto it. 

Alarms and notifications for this node will be activated back once the maintenance period is exceeded, or you explicitly toggling it OFF.

Reboot Host
'''''''''''

Initiates a system reboot of the selected host. Once initiated, ClusterControl will monitor the reboot progress every 5 seconds for 10 minutes (600 seconds).

Restart Node
'''''''''''''

Restart the active monitored process of the selected host. For example, if the node's role is HAProxy, ClusterControl will restart HAProxy process.

Rebuild Replication Slave
''''''''''''''''''''''''''

Exclusive for slave node. Rebuilds replication slave on this node from another master. This is only relevant if you have setup a replication slave for the cluster and you want to resync the data. It uses ``pg_basebackup`` to stage the replication data.

.. caution:: 'Rebuilding Replication Slave' will wipe out the selected node's PostgreSQL data directory.

Promote Slave
'''''''''''''

Exclusive for slave node. Promotes the selected slave to become the new master. If the master is currently functioning correctly, then stop application queries prior to promoting another slave to safe guard from data loss. Connections on the current running master will be killed after a 10-second grace period.

Stop Node
''''''''''

Stops the database instance on this node. This is not a system shut down.

Delete Node
''''''''''''

Removes the database node from the cluster. When removing a database node, ClusterControl will perform the following action:

1. The monitored service will be stopped.
2. Data files and configuration files will be left intact on the server.
3. The monitored service will be disabled to prevent accidental restarts.

