.. _intro:

Introduction
============

This documentation covers ClusterControl version 1.4.1 which released on April 4th, 2017. This release contains key new features along with performance improvements and bug fixes. Release changelog is available `at Change Logs <changelog.html>`_.

What is ClusterControl?
-----------------------

ClusterControl is an agentless management and automation software for database clusters. It helps deploy, monitor, manage and scale your database server/cluster directly from ClusterControl user interface.

ClusterControl consists of 5 components:

+----------------------------------+---------------------------+------------------------------------------------------------------------------------+
| Component                        | Package naming            | Role                                                                               |
+==================================+===========================+====================================================================================+
| ClusterControl controller (cmon) | clustercontrol-controller | The brain of ClusterControl. A backend service performing automation, management,  |
|                                  |                           | monitoring and scheduling tasks. All the collected data will be stored directly    |
|                                  |                           | inside CMON database.                                                              |
+----------------------------------+---------------------------+------------------------------------------------------------------------------------+
| ClusterControl REST API [#f1]_   | clustercontrol-cmonapi    | Interprets request and response data between ClusterControl UI and CMON database.  |
+----------------------------------+---------------------------+------------------------------------------------------------------------------------+
| ClusterControl UI                | clustercontrol            | A modern web user interface to visualize and manage the cluster. It interacts with | 
|                                  |                           | CMON controller via remote procedure call (RPC) or REST API interface.             |
+----------------------------------+---------------------------+------------------------------------------------------------------------------------+
| ClusterControl NodeJS            | clustercontrol-nodejs     | This optional package is introduced in ClusterControl version 1.2.12 to provide an |
|                                  |                           | interface for notification services and integration with 3rd party tools.          |
+----------------------------------+---------------------------+------------------------------------------------------------------------------------+
| ClusterControl CLI               | s9s-tools                 | Open-source command line tool to manage and monitor clusters provisioned by        |
|                                  |                           | ClusterControl.                                                                    |
+----------------------------------+---------------------------+------------------------------------------------------------------------------------+


Supported Database Server/Cluster
---------------------------------

ClusterControl supports the following database servers/clusters:

- Galera Cluster
	- MySQL Galera Cluster (Codership)
	- Percona XtraDB Cluster (Percona)
	- MariaDB Galera Cluster (MariaDB)
- MySQL Cluster (NDB)
- MySQL Replication (master-master and master-slave)
- MySQL Group Replication (beta)
- MySQL Standalone
- MongoDB/Percona Server for MongoDB
	- Replica set
	- Sharded cluster
	- Replicated sharded cluster
- PostgreSQL
	- Single instance
	- Replication
	
Supported Load Balancer
------------------------

ClusterControl supports the following routing softwares:

- HAproxy
- MaxScale
- ProxySQL
- Keepalived (virtual IP address only)

How does it work?
-----------------

ClusterControl components must reside on an independent node apart from your database cluster. For example, if you have a three-nodes Galera cluster, ClusterControl should be installed on the fourth node. Following is an example deployment of having a Galera cluster with ClusterControl:

.. image:: img/cc_deploy.png
   :alt: Example deployment
   :align: center

Once the cmon service is started, it will load up all configuration options inside :file:`/etc/cmon.cnf` and :file:`/etc/cmon.d/cmon_*.cnf` (if exists) into CMON database. Each CMON configuration file represents a cluster with distinct cluster ID. It starts by registering hosts, collecting information and periodically perform check-ups and scheduled jobs on all managed nodes through SSH. Setting up a passwordless SSH is vital in ClusterControl. ClusterControl connects to all managed nodes as ``os_user`` using SSH key defined in ``ssh_identity`` inside CMON configuration file. Details on this is explained under `Passwordless SSH <requirements.html#passwordless-ssh>`_ section.

What user really needs to do is to access ClusterControl UI located at :samp:`http://{ClusterControl_host}/clustercontrol` and starts managing your database infrastructure from there. You can begin by importing existing database clusters, or create a new database server/cluster or register another cluster monitored by another ClusterControl server. ClusterControl supports monitoring multiple clusters and cluster types under single ClusterControl server as shown in the following figure:

.. image:: img/cc_deploy_multiple.png
   :alt: Example multiple cluster deployment
   :align: center

ClusterControl exposes all functionality through remote procedure calls (RPC) on port 9500 (authenticated by a RPC token) and REST API accessible at :samp:`http://{ClusterControl_host}/cmonapi` (authenticated by an API token). The ClusterControl UI interacts with those interfaces to retrieve monitoring data (cluster load, host status, alarms, backup status etc.) or to send management commands (add/remove nodes, run backups, upgrade a cluster, etc.). The following diagram illustrates the architecture of ClusterControl:

.. image:: img/cc_arch.png
   :alt: ClusterControl architecture
   :align: center

ClusterControl has minimal performance impact due to its monitoring responsibility and will not cause any downtime to your database server/cluster. In fact, it will perform automatic recovery (if enabled) when it finds a failed database node or cluster.

What it can do?
---------------

ClusterControl is able to handle most of the administration tasks required to maintain database servers or clusters. Here are some of the tasks that ClusterControl can perform on your database infrastructure:

* Monitor host statistics (CPU/RAM/disk/network/swap)
* Provision multiple database server/cluster in a single CMON process
* Monitor database's stats, variable, log files, queries, for individual node as well as cluster-wide
* Database configuration management
* Database cluster/node recovery
* Trigger alarm and send notifications
* Schedule and perform database backup (mysqldump, Xtrabackup, pgdump, mongodump, mongodb-consistent-backup)
* Database backup status
* Restore backups (MySQL/PostgreSQL)
* Upload backups to AWS S3/Glacier
* Stop/Start/Bootstrap database service
* Deploy a new database server/cluster
* Add existing MySQL/MariaDB server/cluster, MongoDB/TokuMX replica set and PostgreSQL server
* Scale your database cluster (add/remove Galera node, garbd and replication slave)
* Deploy database load balancers (HAproxy, MaxScale, ProxySQL) and virtual IP address (Keepalived)
* Monitor HAproxy/MaxScale/ProxySQL statistics
* Manage MySQL user privileges
* Upgrade MySQL servers
* Promote MySQL slave to master
* Set up a delayed slave
* Stage/Failover replication slave from a master
* Manage private keys and certificates for databases' SSL
* and many more..

For more details, please refer to `ClusterControl product page <http://severalnines.com/product/clustercontrol>`_. You might also want to look at the `ClusterControl changelog <changelog.html>`_ for the latest development update.

.. rubric:: Footnotes

.. [#f1]

    We are gradually in the process of migrating all functionalities in REST API to RPC interface. Kindly expect the REST API to be obselete in the near future.

