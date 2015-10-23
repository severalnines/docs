Overview
--------

Provides summary of all database nodes in the cluster.

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
.........................................

Standalone Nodes Grid
+++++++++++++++++++++

* **Hostname**
	- The PostgreSQL master hostname or IP address.
	
* **Version**
	- PostgreSQL server version.

* **Refresh**
	- Fetch the latest update.

Master Nodes Grid
+++++++++++++++++

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
+++++++++++++++++

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
