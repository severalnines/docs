.. _mongodb-overview:

Overview
--------

Provides summary of all database nodes in the cluster.

Actions
````````

Provides shortcuts to main cluster functionality. Each of database clusters has its own set of functionality as described below:

MongoDB Replica Set
'''''''''''''''''''

* **Add Node to Replica Set**
	- See `Add Node to Replica Set`_.

* **Delete Cluster**
	- Unregister a database cluster from the ClusterControl UI. This action will remove the selected CMONAPI URL and token for corresponding cluster and will NOT uninstall the actual database cluster.
	- You can still re-register your cluster to ClusterControl at a later stage.

Add Node to Replica Set
''''''''''''''''''''''''

Adds a replica member or arbiter node. 

* **Node Type**
	- DB server - MongoDB node to be part of the same replica set.
	- Arbiter 
		- MongoDB arbiter node to be part of the same replica set. 
		- You can add an arbiter to an existing MongoDB node or a new node. If you are doing this, choose "No" under *Install Software*.

* **Hostname**
	- IP address or hostname of the target host.

* **Port**
	- MongoDB port. Default is 27017 for MongoDB replica set and 3000 for MongoDB arbiter node.
	
* **Configuration**
	- Configuration template must exist under *ClusterControl > Manage > Configurations > Template Configuration Files*.

* **Replica set**
	- Choose the replica set.
	
* **Install Software**
	- Install the required software to run the database. This includes MongoDB server/client together with dependencies.
	
* **Software Packages**
	- Install MongoDB using packages defined in *ClusterControl > Manage > Software Packages*.

* **Disable Firewall**
	- Yes - To disable firewall during deployment (recommended).
	- No - Firewall settings will be untouched.

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled.

opscounter
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

Customize your dashboard in the `Overview`_ page by selecting which metrics and graphs to display. The created custom dashboards will appear as tabs beside *Dash Settings*.

* **Dashboard Name**
	- Give a name to the dashboard.

* **Metric**
	- Select an available metric from the list.

* **Scale**
	- Choose between linear or logarithmic graph scale.

* **Selected as Default Graph**
	- Choose Yes if you want to set the graph as default when viewing the Overview page.

.. Note:: You can rearrange dashboard order by drag and drop above.


Nodes Statistics
`````````````````

This provides a summary of host and replication-related stats for all nodes. These values are refreshed every *Refresh rate* values defined at the top of the page. 

Each database cluster has it’s own set of statistics as explained below:

MongoDB Replica Set
....................

* **Node**
	- MongoDB instance consists of node's IP address or hostname and MongoDB port.

* **Role**
	- Instance role:
		- Primary - The primary node receives all write operations.
		- Secondary - Secondaries replicate operations from the primary to maintain an identical data set.
		- Arbiter - mongod instances that are part of a replica set but do not hold data. Arbiters participate in elections in order to break ties.

* **Message**
	- Latest MongoDB status on the instance.

* **Global Lock**
	- Ratio - The value of ratio displays the relationship between lockTime and totalTime. See `serverStatus.globalLock.ratio <http://docs.mongodb.org/v2.2/reference/server-status/#serverStatus.globalLock.ratio>`_.
	- Queue - The value of total provides a combined total of operations queued waiting for the lock. See `serverStatus.globalLock.currentQueue.total <http://docs.mongodb.org/v2.6/reference/command/serverStatus/#serverStatus.globalLock.currentQueue.total>`_.

* **Replication Lag**
	- Delay between an operation on the primary and the application of that operation from the oplog to the secondary in seconds.

* **Connections**
	- The value of current corresponds to the number of connections to the database server from clients over unused available incoming connections the database can provide. See `serverStatus.connections.current <http://docs.mongodb.org/manual/reference/command/serverStatus/#serverStatus.connections.current>`_ and `serverStatus.connections.available <http://docs.mongodb.org/manual/reference/command/serverStatus/#serverStatus.connections.available>`_.

* **PFs**
	- Reports the total number of page faults that require disk operations. See `serverStatus.extra_info.page_faults <http://docs.mongodb.org/manual/reference/command/serverStatus/#serverStatus.extra_info.page_faults>`_.

* **pfs/ops**
	- Page faults ratio over operations in percentage.


Hosts
`````

Shows collected system statistics in a grid as below:

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
