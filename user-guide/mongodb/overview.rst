.. _mongodb-overview:

Overview
--------

Provides summary of all database nodes in the cluster.

Actions
````````

Provides shortcuts to main cluster functionality.

* **Add Node to Replica Set**
	- See `Add Node to Replica Set`_.

* **Delete Cluster**
	- Unregister a database cluster from the ClusterControl UI. This action will remove the selected CMONAPI URL and token for corresponding cluster and will NOT uninstall the actual database cluster.
	- You can still re-register your cluster to ClusterControl at a later stage.

* **Deregister Cluster from UI**
	- Unregister a database cluster from the ClusterControl UI. 
	- You can still re-register your cluster to ClusterControl at a later stage by using `Cluster Registrations <../../user-guide/index.html#cluster-registrations>`_.

Add Node to Replica Set
''''''''''''''''''''''''

Adds a replica member or arbiter node. 

* **Node Type**
	- DB server - MongoDB node to be part of the same replica set.
	- Arbiter 
		- MongoDB arbiter node to be part of the same replica set. 
		- You can add an 	arbiter to an existing MongoDB node or a new node. If you are doing this, choose "No" under *Install Software*.

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

* **Disable Firewall**
	- Yes - To disable firewall during deployment (recommended).
	- No - Firewall settings will be untouched.

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled.

Shard Servers, Config Servers & Mongos Servers
``````````````````````````````````````````````

The opscounter graph provides overview of aggregated operation on the MongoDB shard servers. For sharded cluster, there will be another two sections for Config and Mongos Servers.

* **Dash Settings**
	- Customize the Cluster Load dashboard. See `Custom Dashboard`_ section.

* **query**
	- The number of aggregated query across all nodes.

* **insert**
	- The number of aggregated insert command across all nodes.

* **update**
	- The number of aggregated update command across all nodes.

* **delete**
	- The number of aggregated delete command across all nodes.

* **getmore**
	- The number of aggregated getmore command across all nodes.

* **command**
	- The total of all commands running across all nodes.

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

This provides a summary of host and replication-related stats for all nodes. Each database cluster has its own set of statistics as explained below:

* **Node**
	- MongoDB instance consists of node's IP address or hostname and MongoDB service port.

* **Role**
	- Instance role:
		- Primary - The primary node receives all write operations.
		- Secondary - Secondaries replicate operations from the primary to maintain an identical data set.
		- ConfigSvr - Stores the metadata for a sharded cluster.
		- Arbiter - mongod instances that are part of a replica set but do not hold data. Arbiters participate in elections in order to break ties.

* **Message**
	- Latest MongoDB status on the instance.
	
* **Uptime**
	- MongoDB service uptime.

* **Global Lock**
	- Ratio - The value of ratio displays the relationship between lockTime and totalTime. See `serverStatus.globalLock.ratio <http://docs.mongodb.org/v2.2/reference/server-status/#serverStatus.globalLock.ratio>`_.
	- Queue - The value of total provides a combined total of operations queued waiting for the lock. See `serverStatus.globalLock.currentQueue.total <http://docs.mongodb.org/v2.6/reference/command/serverStatus/#serverStatus.globalLock.currentQueue.total>`_.

* **Replication Lag**
	- Delay between an operation on the primary and the application of that operation from the oplog to the secondary in seconds.

* **Connections**
	- The value of current corresponds to the number of connections to the database server from clients over unused available incoming connections the database can provide. See `serverStatus.connections.current <http://docs.mongodb.org/manual/reference/command/serverStatus/#serverStatus.connections.current>`_ and `serverStatus.connections.available <http://docs.mongodb.org/manual/reference/command/serverStatus/#serverStatus.connections.available>`_.

Hosts
`````

Shows collected system statistics in a grid as below:

* **Ping**
	- Ping round trip from ClusterControl host to each host in microseconds.

* **CPU Util(%)**
	- Total of CPU utilization in percentage.

* **Loadavg 1/5/15**
	- Load value captured for 1, 5 and 15 minutes average.

* **Net tx/s / rx/s**
	- Amount of data transmitted and received by the host.

* **Disk Read/sec**
	- Disk read of ``monitored_mountpoint``.

* **Disk Writes/sec**
	- Disk write of ``monitored_mountpoints``.

* **Uptime**
	- Host uptime.

* **Last Updated**
	- The last time ClusterControl fetch for host's status.
