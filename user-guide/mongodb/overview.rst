.. _MongoDB - Overview:

Overview
--------

Provides summary of all database nodes in the cluster. This page is accessible only if there is a cluster deployed by ClusterControl via `Deploy Database Cluster <../../user-guide/index.html#deploy-database-cluster>`_ or imported into ClusterControl via `Import Existing Server/Database Cluster <../../user-guide/index.html#import-existing-server-cluster>`_.

.. _MongoDB - Overview - Actions:

Actions
++++++++

Provides shortcuts to the main cluster functionality.

* **Add Node**
	- See `Add Node`_.
	
* **Convert to Shard**
	- Exclusive for MongoDB Replica Set. See `Convert to Shard`_.
	
* **Add Shard**
	- Exclusive for MongoDB Sharded Cluster. See `Add Shard`_.

* **Remove Shard**
	- Exclusive for MongoDB Sharded Cluster. See `Remove Shard`_.

* **Change RPC API Token**
	- Serves as the auntentication string by ClusterControl UI to connect to CMON RPC interface. Each cluster has its own unique token.
	
.. Note:: You can retrieve the RPC API Token value directly from respective ``/etc/cmon.d/cmon_{clusterID}.cnf``.

* **Disable/Enable Cluster Recovery**
	- Disable or enable cluster recovery. 
	- This feature works similarly with the automatic recovery (cluster) toggle button in the summary bar. However, it applies the modification permanently into CMON configuration file for persistency.

* **Disable/Enable Node Recovery**
	- Disable or enable node recovery. This feature works similarly with with the automatic recovery toggle buttons in the summary bar.
	- This feature works similarly with the automatic recovery (node) toggle button in the summary bar. However, it applies the modification permanently into CMON configuration file for persistency.

* **Schedule Maintenance Mode**
	- Schedules a cluster-wide maintenance mode, where ClusterControl will skip raising up alarms and notifications while the mode is active. 
	- All nodes in the cluster (regardless of the role) will be marked under maintenance. A global banner will appear if there is an upcoming maintenance for the corresponding cluster.

* **Delete Cluster**
	- Unregister a database cluster from the ClusterControl UI. This action will remove the selected CMONAPI URL and token for corresponding cluster and will NOT uninstall the actual database cluster.
	- You can still re-register your cluster to ClusterControl at a later stage.

* **Deregister Cluster from UI**
	- Unregister a database cluster from the ClusterControl UI. 
	- You can still re-register your cluster to ClusterControl at a later stage by using `Cluster Registrations <../../user-guide/index.html#cluster-registrations>`_.

Convert to Shard
````````````````

Converts an existing MongoDB replica set to a sharded cluster by adding mongos and config servers into the setup. 

1) Configuration Servers and Routers
''''''''''''''''''''''''''''''''''''
    
*Configuration Server*

* **Server Port**
	- MongoDB config server port. Default is 27019.

* **Add Configuration Servers**
	- Specify the IP address or hostname of the MongoDB config servers. Minimum of one node is required, recommended to use three nodes.

*Routers/Mongos*

* **Server Port**
	- MongoDB mongos server port. Default is 27017.

* **Add More Routers**
	- Specify the IP address or hostname of the MongoDB mongos.
	
2) Database Settings
''''''''''''''''''''

* **Server Data Directory**
	- Location of MongoDB data directory. Default is ``/var/lib/mongodb``.

* **mongodb.conf Template**
	- MongoDB configuration template file under ``/usr/share/cmon/templates``. Default is ``mongodb.conf.org``. Keep it default is recommended.

* **mongos.conf Template**
	- MongoDB configuration template file under ``/usr/share/cmon/templates``. Default is ``mongos.conf.org``. Keep it default is recommended.

* **Install Software**
    - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

* **Deploy**
	- Starts the MongoDB Sharded Cluster deployment.

Add Shard
``````````

Adds a new shard. Once the new shard has been added to the cluster, the MongoDB shard router will start to assign new chunks to it, and the balancer will automatically balance all chunks over all the shards.

1) Define Shards
'''''''''''''''''

* **Replica Set Name**
	- Specify a name for this replica set shard.

* **Server Port**
	- MongoDB shard server port. Default is 27018.

* **Hostname**
	- Specify the IP address or hostname of the MongoDB shard servers. Minimum of one node is required, recommended to use three nodes.
	
* **Advanced Options**
	- Click on this to open set of advanced options for this particular node in this shard:
		- Add slave delay - Specify the amount of delayed slave in miliseconds format.
		- Act as an arbiter - Toggle to 'Yes' if the node is arbiter node. Otherwise, choose 'No'.
	
2) Database Settings
''''''''''''''''''''

* **Server Data Directory**
	- Location of MongoDB data directory. Default is ``/var/lib/mongodb``.

* **mongodb.conf Template**
	- MongoDB configuration template file under ``/usr/share/cmon/templates``. Default is ``mongodb.conf.org``. Keep it default is recommended.

* **mongos.conf Template**
	- MongoDB configuration template file under ``/usr/share/cmon/templates``. Default is ``mongos.conf.org``. Keep it default is recommended.

* **Install Software**
    - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

* **Deploy**
	- Starts the MongoDB Sharded Cluster deployment.


Add Node
````````

Scales the current MongoDB Replica Set or Sharded Cluster deployment by adding single shard, mongos or config server.

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
	- Configuration template must exist under *ClusterControl > Manage > Configurations > Templates*. Use the mongod (shard) configuration file for this deployment.

* **Replica set**
	- Choose the replica set.
	
* **Install Software**
	- Install the required software to run the database. This includes MongoDB server/client together with dependencies.

* **Disable Firewall**
	- Yes - To disable firewall during deployment (recommended).
	- No - Firewall settings will be untouched.

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled.
	
Add Routers/Mongos
'''''''''''''''''''

* **Hostname**
	- IP address or hostname of the mongo host.

* **Port**
	- MongoDB port. Default is 27018.
	
* **Configuration**
	- Configuration template must exist under *ClusterControl > Manage > Configurations > Templates*. Use the mongos configuration file for this deployment.
	
* **Install Software**
	- Installs the required software to run the database. This includes MongoDB server/client together with dependencies.

* **Disable Firewall**
	- Yes - To disable firewall during deployment (recommended).
	- No - Firewall settings will be untouched.

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled.

Remove Shard
``````````````

Removes or moves a replica set in a sharded cluster setup. Removing shards is a bit harder than to add a shard, as this involves moving the data to the other shards before removing the shard itself. For all data that has been sharded over all shards, this will be a job performed by the MongoDB balancer. 

* **Remove Replica Set**
	- Choose the shard you want to remove.

* **Move to Replica Set**
	- Moves the selected shards to another shard/replica set. Any non-sharded database/collection, that was assigned this shard as its primary shard, needs to be moved to another shard and made its new primary shard. For this process, MongoDB needs to know where to move these non-sharded databases/collections to.

* **Remove Shard**
	- Click the button to proceed.

Shard Servers, Config Servers & Mongos Servers
+++++++++++++++++++++++++++++++++++++++++++++++

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
+++++++++++++++++

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
++++++++++++++++

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
+++++

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
