Nodes
-----

Provides detailed information for each node in the cluster. On the left hand column, you can find a list of all nodes that are members of the cluster including ClusterControl node. If you added slaves to your cluster through ClusterControl, these will also be listed.


Nodes Monitoring
+++++++++++++++++

The node on the list will appear in red colour to indicate it is unhealthy. The tabs show performance and resource usage for a specific node. There are also database specific tabs depending on the type of database running on the host:

* **Overview**
	- Provides a summary of host information and statistic histogram including CPU, disk, network and memory usage.
	- Database node status indicator:

  =========== ===========
  Status      Description
  =========== ===========
  OK          Indicates the node is working fine.
  WARNING     Indicates the node is degraded and not performing as expected.
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
	
* **DB Performance**
	- Overview of chosen database performance counters shown in graph.

* **DB Variables**
	- PostgreSQL variables, similar to ``SHOW ALL`` command.

Nodes Actions
+++++++++++++

The remove icon will only appear when you rollover the mouse pointer on the node icon in the left-hand column. This removes the database node from the cluster.

.. Note:: You can monitor the job's progress at *ClusterControl > Activity > Jobs*.

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
``````````````

Restarts the active monitored process of the selected host. For example, if the node's role is HAProxy, ClusterControl will restart HAProxy process. This is not a system reboot. Only available if the service is started. 

You can configure the graceful shutdown timeout (default is 1800 seconds) in the "Confirm Shutdown" dialog. ClusterControl will give up waiting for a node to gracefully terminate. If the node is still running after the timeout you may send the SIGKILL signal to force the node down by toggling on 'Force stop (SIGKILL) node after the graceful shutdown timeout has been reached' option.

The node will be shutdown and enter maintenance mode.

Enable WAL Archiving
`````````````````````

To utilize PITR, user has to take a :term:`pg_basebackup` backup, and if Write-ahead Log (WAL) files are "archived" after the backup, then it is possible to roll forward to any timestamp from backup creation time until the most recent event captured in the WAL files. This job will update the configuration of the chosen node to enable the WAL archiving.

* **Compress WAL Archive**
	- Toggle on if you want to compress the WAL archive. Default is on.

* **Custom WAL Archive Directory (Optional)**
	- Specify a custom directory for WAL Archive. Default to one path up the data directory, for example if data directory at ``/var/lib/pgsql/10/data`` then the WAL archive directory is located at ``/var/lib/pgsql/10/wal_archive``.

Stop Node
``````````

Stops the monitored process of the selected host. For example, if the node's role is HAProxy, ClusterControl will restart HAProxy process. This is not a system shut down. Only available if the service is started. 

You can configure the graceful shutdown timeout (default is 1800 seconds) in the "Confirm Shutdown" dialog. ClusterControl will give up waiting for a node to gracefully terminate. If the node is still running after the timeout you may send the SIGKILL signal to force the node down by toggling on 'Force stop (SIGKILL) node after the graceful shutdown timeout has been reached' option.

The node will be shutdown and enter maintenance mode.

Start Node
``````````

Starts the monitored process of the selected host. For example, if the node's role is HAProxy, ClusterControl will restart HAProxy process. Only available if the service is stopped. 

Rebuild Replication Slave
``````````````````````````

Exclusive for slave node. It rebuilds replication slave on this node from another master. This is only relevant if you have setup a replication slave for the cluster and you want to re-sync the data. It uses ``pg_basebackup`` to stage the replication data.

.. caution:: *Rebuilding Replication Slave* will wipe out the selected node's PostgreSQL data directory.

Choose a master node from the dropdown list, and then click Proceed to start rebuild the salve. The following actions will happen:

1. Stop PostgreSQL server (slave).
2. Remove content from its data directory.
3. Stream a backup from the master to the slave using ``pg_basebackup``.
4. Start the slave.

Promote Slave
``````````````

Exclusive for slave node. Promotes the selected slave to become the new master. If the master is currently functioning correctly, then stop application queries prior to promoting another slave to safe guard from data loss. Connections on the current running master will be killed after a 10-second grace period.

Unregister Node
````````````````

Removes the database node from the database cluster and/or ClusterControl monitoring. You can choose one of the these three options:

* *Keep the service running* - Node will be unregistered from ClusterControl but the service will be kept running. This node will remain part of the database cluster.
* *Stop service and keep files untouched* - Node will be unregistered from ClusterControl and the service will be stopped. Data files and configuration files will be left intact on the server. The node will be down, but would be part of the database cluster if started.
* *Stop and uninstall service (all configuration files will be deleted)* - Node will be unregistered from ClusterControl and the service will be stopped. Data files and configuration files will be deleted on the server. The monitored service will be disabled to prevent accidental restarts.
