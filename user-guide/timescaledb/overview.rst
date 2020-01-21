.. _TimeScaleDB - Overview:

Overview
--------

Provides summary of all database nodes in the cluster. This page is accessible only if there is a cluster deployed by ClusterControl via :ref:`Deploy Database Cluster` or imported into ClusterControl via :ref:`Import Existing Server Cluster`.

.. _TimeScaleDB - Overview - Actions:

Actions
+++++++

Provides shortcuts to the main cluster functionality. For TimeScaleDB Streaming Replication, the action menu consists of:

* **Add Load Balancer**
	- See :ref:`TimeScaleDB - Manage - Load Balancer`.

* **Add Replication Slave**
	- Deploys a replication slave attached to this cluster. Choose one of the TimeScaleDB node to be a master. See `Add Replication Slave`_.

* **Change RPC API Token**
	- Serves as the authentication string by ClusterControl UI to connect to CMON RPC interface. Each cluster has its own unique token.
	
.. Note:: You can retrieve the RPC API Token value directly from respective ``/etc/cmon.d/cmon_{clusterID}.cnf``.

* **Create Slave Cluster**
	- Create a new cluster that replicates from this cluster. See `Cluster-Cluster Replication`_.

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
	- This action will remove the corresponding cluster from ClusterControl supervision and will NOT uninstall the actual database cluster.
	- If you want to re-add the cluster, you have to use :ref:`Import Existing Server Cluster`.	

* **Deregister Cluster from UI**
	- Unregister a database cluster from the ClusterControl UI.
	- You can still re-register your cluster to ClusterControl at a later stage by using :ref:`UserGuide - Global Settings - Cluster Registrations`.

Add Replication Slave
``````````````````````

TimeScaleDB replication slave requires at least a master node. The following must be true for the master:

* At least one master under the same cluster ID.
* Only TimeScaleDB 9.x, 10.x and 11.x is supported.
* Master's TimeScaleDB port is accessible by ClusterControl and slaves.

For the slave, you would need a separate host or VM, with or without TimeScaleDB installed. If you do not have a TimeScaleDB installed, and choose ClusterControl to install the TimeScaleDB on the slave, ClusterControl will perform the necessary actions to prepare the slave, for example, create a slave user, configure TimeScaleDB, start the server and also start the replication. Prior to the deployment, you must perform the following actions:

* The slave node must be accessible using passwordless SSH from the ClusterControl server
* TimeScaleDB port (default 5432) on the slave is open for connections for at least ClusterControl server and the other members in the cluster.

To prepare the TimeScaleDB configuration file for the slave, go to *ClusterControl > Manage > Configurations > Template Configuration files*. Later, specify this template file when adding a slave.

Add New Replication Slave
''''''''''''''''''''''''''

The slave will be setup from a streamed backup using ``pg_basebackup`` from the master to the slave. 

* **Master Server**
	- Select a master server.

* **Slave Server**
	- Specify the IP address or :term:`FQDN` of the slave node. This node must be accessible from ClusterControl node via passwordless SSH.

* **Do you want to install the Slave server**
	- Yes - Install TimeScaleDB Server packages. It will based on the repository and vendor used by the master. For example, if you are running on TimeScaleDB 10.x, ClusterControl will use the same repository to setup the slave.

* **Disable firewall**
	- Check the box to disable firewall (recommended).

* **Disable SELinux/AppArmor**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled (recommended).

.. Note:: Existing TimeScaleDB server packages will be uninstalled.

Add Existing Replication Slave
''''''''''''''''''''''''''''''

Add an existing replication slave into ClusterControl. Use this feature if you have added a replication slave manually to your cluster and want it to be detected and managed by ClusterControl. ClusterControl will then detect the new database node as being part of the cluster and starts to manage and monitor it as with the rest of the cluster nodes. Useful if a node has been configured outside of ClusterControl e.g, through Puppet, Chef or Ansible.

* **Hostname**
	- IP address or :term:`FQDN` of the target node. If you already have the host added under *ClusterControl > Manage > Hosts*, you can just choose the host from the dropdown menu.

* **Port**
	- TimeScaleDB port. Default is 5432.

Create SSL Encryption
``````````````````````

Enable encrypted SSL client-server connections for the database node(s). The same certificate will be used on all nodes. To enable SSL encryption the nodes must be restarted. Select 'Restart Nodes' to perform a rolling restart of the nodes.

* **Create Certificate**
    - Create a self-signed certificate immediately and use it to setup SSL encryption.

* **Certificate Expiration (days)**
    - Number of days before the certificate become expired and invalid. Default is 10 years (3650 days).

* **Use Certificate**
    - Choose the certificate and key that generated by :ref:`Sidebar - Key Management`.

* **Restart Cluster**
    - Restart Nodes - Automatically perform rolling restart of the nodes after setting up certificate and key.
    - Do Not Restart Nodes - Do nothing after setting up certificate and key. User has to perform the server restart manually.

Cluster-Cluster Replication
````````````````````````````

This feature allows you to create a new cluster that will be replicating from this cluster. One primary use case is for disaster recovery by having a hot standby site/cluster which can take over when the main site/cluster has failed. Clusters can be rebuilt with an existing backup or by streaming from a master on the source cluster.

For PostgreSQL-based clusters, ClusterControl will configure asynchronous streaming replication between a master cluster to a slave cluster.

* **Cluster Provisioning Data**
	- Choose one method to provision the slave's cluster data:
		- *Streaming from the master*: Stream the data from a master using hot backup tool e.g, ``pg_basebackup``.
		- *Stage cluster from backup*: Choose an existing full backup from the dropdown list. If none is listed, take full backup of one of the nodes in your cluster which have binary logging enabled.

* **Replication Master**
	- A node of the source cluster to replicate from. For PostgreSQL/TimescaleDB, pick the master node from the source cluster.

Once the above options have been selected, the cluster deployment wizard will appear similar to deploying a new cluster. See :ref:`Deploy Database Cluster`.

A slave cluster will appear in the database cluster list after deployment finishes. You will notice the slave cluster entry is a bit indented in the list, with a pointed arrow coming from the source cluster, indicating the cluster-cluster replication is now active.

Server Load
++++++++++++

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
++++++++++++++++

Customize your dashboard in the `Overview`_ page by selecting which metrics and graphs to display. For TimeScaleDB nodes, 2 graphs are configured by default:

====================== ===========
Dashboard Name         Description
====================== ===========
Server Load            Shows aggregated load on your database node.
Cache hit ratio        Shows aggregated data on overall hit ratios.
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
++++++++++++++++++++++

Displays a summary of host and database-related stats for all database nodes.

Standalone Nodes Grid
``````````````````````

* **Hostname**
	- The TimeScaleDB master hostname or IP address.
	
* **Version**
	- TimeScaleDB server version.

* **Refresh**
	- Fetch the latest update.

Master Nodes Grid
``````````````````

This grid appears if ClusterControl detects master TimeScaleDB node, where ``select pg_is_in_recovery()`` returns false.

* **Hostname**
	- The TimeScaleDB master hostname or IP address.
	
* **Version**
	- TimeScaleDB server version.

* **Writable**
	- Green tick - Node is writable.
	- Red cross - Node is read-only.
	
* **Refresh**
	- Fetch the latest update.

Slave Nodes Grid
``````````````````

This grid appears if ClusterControl detects any standby TimeScaleDB node, where ``select pg_is_in_recovery()`` returns true.

* **Hostname**
	- The TimeScaleDB slave hostname or IP address.

* **Version**
	- TimeScaleDB server version.

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
++++++

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
