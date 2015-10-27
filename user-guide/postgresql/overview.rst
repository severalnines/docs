Overview
--------

Provides summary of all database nodes in the cluster.

Actions
```````

Provides shortcuts to main cluster functionality. Each of database clusters has its own set of functionality as described below:

PostgreSQL (Standalone or Replication)
''''''''''''''''''''''''''''''''''''''

* **Add Node**
	- See `Add Node`_ section.

* **Add Replication Slave**
	- Deploys a replication slave attached to this cluster. Choose one of the Galera node to be a master. See `Add Replication Slave`_.

* **Delete Cluster**
	- This action will remove the corresponding cluster from ClusterControl supervision and will NOT uninstall the actual database cluster.
	- If you want to re-add the cluster, you have to use `Add Existing Server/Cluster <../../user-guide/index.html#add-existing-server-cluster>`_.	

* **Deregister Cluster from UI**
	- Unregister a database cluster from the ClusterControl UI. 
	- You can still re-register your cluster to ClusterControl at a later stage by using `Cluster Registrations <../../user-guide/index.html#cluster-registrations>`_.

Add Node
''''''''

Deploys a new or add existing PostgreSQL standalone database node. To add a new slave, see `Add Replication Slave`_.

Create and add a new DB node
............................

If you specify a new hostname or IP address, make sure that the node is accessible from ClusterControl node and following conditions are true:

* ClusterControl host is able to connect to the target host via passwordless SSH.
* The target host must use the same operating system distribution with the ClusterControl host.

* **Hostname**
	- IP address or :term:`FQDN` of the target node. If you already have the host added under *ClusterControl > Manage > Hosts*, you can just choose the host from the dropdown menu.

* **Configuration**
	- Choose a MySQL configuration template for the new node. The configuration file should be created at *ClusterControl > Manage > Configurations > Template Configuration Files*.
	
* **Install Software**
	- If you already have the database server installed on the target host but not yet configured, you can tell ClusterControl to skip the database installation part by choosing 'No'.

* **Disable Firewall**
	- Check the box to disable firewall(recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled.

Add an existing DB node
.......................

Use this function if you have created a DB node manually and want it to be detected/managed by ClusterControl. ClusterControl will then detect the new DB node as being part of the database group and starts to manage and monitor it as with the rest of the database nodes. Useful if a node has been created outside of ClusterControl e.g, through Puppet, Chef or Ansible.

* **Hostname**
	- IP address or :term:`FQDN` of the target node. If you already have the host added under *ClusterControl > Manage > Hosts*, you can just choose the host from the dropdown menu.

* **Port**
	- PostgreSQL port. Default is 5432.


Add Replication Slave
'''''''''''''''''''''

PostgreSQL replication slave requires at least a master node.

The following must be true for the masters:

* At least one master among under the same cluster ID
* Only PostgreSQL 9.x is supported
* Master’s PostgreSQL port is accessible by ClusterControl and slaves

For the slave, you would need a separate host or VM, with or without PostgreSQL installed. If you do not have a PostgreSQL installed, and choose ClusterControl to install the PostgreSQL on the slave, ClusterControl will perform the necessary actions to prepare the slave, for example, create slave user, configure PostgreSQL, start the server and also start the replication. Prior to the deployment, you must perform following actions:

* The slave node must be accessible using passwordless SSH from the ClusterControl server
* PostgreSQL port (default 5432) on the slave is open for connections.

To prepare the PostgreSQL configuration file for the slave, go to *ClusterControl > Manage > Configurations > Template Configuration files*. Later, specify this template file when adding a slave.


Server Load
````````````

The Server Load graph provides overview of aggregated load on your database server.

* **Dash Settings**
	- Customize the Cluster Load dashboard. See `Custom Dashboard`_ section.

* **Filter by Host**
	- Show the data for selected host on corresponding graph.

* **Connections**
	- The number of aggregated connections to the database nodes.
	
* **Commits**
	- The number of COMMITS statements on the database node.

* **Fetched**
	- The number of aggregated SELECT queries on the database node.

* **Inserted**
	- The number of aggregated INSERT queries on the database node.

* **Updated**
	- The number of aggregated UPDATE queries on the database node.

* **Deleted**
	- The number of aggregated DELETE queries on the database node.

* **Rollbacks**
	- The number of ROLLBACKS statements on the database node.

Custom Dashboard
````````````````

Customize your dashboard in the `Overview`_ page by selecting which metrics and graphs to display. For Galera nodes, 2 graphs are configured by default:

====================== ===========
Dashboard Name         Description
====================== ===========
Server Load            Shows aggregated load on your database node.
Cache hit ration       Shows aggregated data on overall hit ratios.
====================== ===========

The created custom dashboards will appear as tabs beside *Dash Settings*.

* **Dashboard Name**
	- Give a name to the dashboard.

* **Metric**
	- Select an available metric from the list.

* **Scale**
	- Choose between linear or logarithmic graph scale.

* **Selected as Default Graph**
	- Choose Yes if you want to set the graph as default when viewing the Overview page.

.. Note:: You can rearrange dashboard order by drag and drop above.

Hosts/Nodes Statistics
``````````````````````

This provides a summary of host and replication-related stats for all nodes. Each database cluster has it’s own set of statistics as explained below:

PostgreSQL single instance or replication
''''''''''''''''''''''''''''''''''''''''''

Standalone Nodes Grid
.....................

* **Hostname**
	- The PostgreSQL master hostname or IP address.
	
* **Version**
	- PostgreSQL server version.

* **Refresh**
	- Fetch the latest update.

Master Nodes Grid
..................

This grid appears if ClusterControl detects the PostgreSQL node (using ``select pg_is_in_recovery()``) returns false.

* **Hostname**
	- The PostgreSQL master hostname or IP address.
	
* **Version**
	- PostgreSQL server version.

* **Writable**
	- Green tick - Node is writable.
	- Red cross - Node is read-only.
	
* **Refresh**
	- Fetch the latest update.

Slave Nodes Grid
................

This grid appears if ClusterControl detects the PostgreSQL node (using ``select pg_is_in_recovery()``) returns true.

* **Hostname**
	- The PostgreSQL slave hostname or IP address.

* **Version**
	- PostgreSQL server version.

* **Replication State**
	- Current WAL sender state.

* **Master Host**
	- The master host that the slave is connected to.

* **Received Location**
	- Last transaction log position sent on this connection.

* **Replay Location**
	- Last transaction log position replayed into the database on this standby server.

* **Lag (sec.)**
	- How many seconds this slave behind the master.

* **Writable**
	- Green tick - Node is writable.
	- Red cross - Node is read-only.

* **Refresh**
	- Fetch the latest update.

Hosts
`````

Shows collected system statistics in a table as below:

* **Ping**
	- Ping round trip from ClusterControl host to each host in milliseconds.

* **CPU util/steal**
	- Total of CPU utilization in percentage.

* **Loadavg (1/5/15)**
	- Load value captured for 1, 5 and 15 minutes average.

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
	
* **Refresh**
	- Fetch the latest update.
