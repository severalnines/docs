.. _intro:

Introduction
============

This documentation covers ClusterControl version 1.8.1 which was released on November 13\ :sup:`th`\ , 2020. This release contains key new features along with performance improvements and bug fixes. The release changelog is available at :ref:`Changelog`.

What is ClusterControl? 
-----------------------

ClusterControl is an agentless management and automation software for database clusters. It helps deploy, monitor, manage and scale your database server/cluster directly from ClusterControl user interface.

ClusterControl consists of a number of components:

+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| Component                          | Package naming               | Role                                                                               |
+====================================+==============================+====================================================================================+
| ClusterControl Controller (cmon)   | clustercontrol-controller    | The brain of ClusterControl. A backend service performing automation, management,  |
|                                    |                              | monitoring and scheduling tasks. All the collected data will be stored directly    |
|                                    |                              | inside the CMON database.                                                          |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl UI                  | clustercontrol               | A modern web user interface to visualize and manage the cluster. It interacts with | 
|                                    |                              | CMON controller via remote procedure call (RPC) interface.                         |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl SSH                 | clustercontrol-ssh           | Optional package introduced in ClusterControl 1.4.2 for ClusterControl's           |
|                                    |                              | web SSH console. Only works with Apache 2.4+.                                      |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl Notifications       | clustercontrol-notifications | Optional package introduced in ClusterControl 1.4.2 providing a service and user   |
|                                    |                              | interface for notification services and integration with third party tools.        |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl Cloud               | clustercontrol-cloud         | Optional package introduced in ClusterControl 1.5.0 providing a service and user   |
|                                    |                              | interface for integration with cloud providers.                                    |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl Cloud File Manager  | clustercontrol-clud          | Optional package introduced in ClusterControl 1.5.0 providing a command-line       |
|                                    |                              | interface to interact with storage objects on cloud.                               |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+
| ClusterControl CLI                 | s9s-tools                    | Open-source command line tool to manage and monitor clusters provisioned by        |
|                                    |                              | ClusterControl.                                                                    |
+------------------------------------+------------------------------+------------------------------------------------------------------------------------+

.. seealso:: :ref:`ClusterControl Components <Components>`.

Supported Database Server/Cluster
---------------------------------

ClusterControl supports the following database servers/clusters:

- Galera Cluster
	- Percona XtraDB Cluster (Percona)
	- MariaDB Galera Cluster (MariaDB)
- MySQL Cluster (NDB)
- MySQL/MariaDB
	- Standalone
	- Replication (master-master and master-slave)
- MongoDB/Percona Server for MongoDB
	- Replica set
	- Sharded cluster
	- Replicated sharded cluster
- PostgreSQL/TimescaleDB
	- Standalone
	- Streaming replication (asynchronous and synchronous)
	
Supported Load Balancer
------------------------

ClusterControl supports the following routing softwares:

- HAProxy
- MariaDB MaxScale
- ProxySQL
- Keepalived (virtual IP address only)

How Does it Work?
-----------------

ClusterControl components must reside on an independent node apart from your database cluster. For example, if you have a three-node Galera cluster, ClusterControl should be installed on the fourth node. Following is an example deployment of having a Galera cluster with ClusterControl:

.. image:: img/cc_deploy.png
   :alt: Example deployment
   :align: center

Once the cmon service is started, it would load up all configuration options inside :file:`/etc/cmon.cnf` and :file:`/etc/cmon.d/cmon_*.cnf` (if exists) into CMON database. Each CMON configuration file represents a cluster with distinct cluster ID. It starts by registering hosts, collecting information and periodically perform check-ups and scheduled jobs on all managed nodes through SSH. Setting up a passwordless SSH is vital in ClusterControl for agentless management purposes. For monitoring, ClusterControl can be configured with both agentless and agent-based setup, see :ref:`Components - ClusterControl Controller - Monitoring Operations` for details.

ClusterControl connects to all managed nodes as ``os_user`` using SSH key defined in ``ssh_identity`` inside CMON configuration file. Details on this is explained under `Passwordless SSH <requirements.html#passwordless-ssh>`_ section.

What user really needs to do is to access ClusterControl UI located at :samp:`http://{ClusterControl_host}/clustercontrol` and start managing your database infrastructure from there. You can begin by importing an existing database cluster, or create a new database server or cluster, on-premises or in the cloud. ClusterControl supports monitoring multiple clusters and cluster types under a single ClusterControl server as shown in the following figure:

.. image:: img/cc_deploy_multiple2.png
   :alt: Example multiple cluster deployment
   :align: center

ClusterControl controller exposes all functionality through remote procedure calls (RPC) on port 9500 (authenticated by a RPC token), port 9501 (RPC with TLS) and integrates with a number of modules like notifications (9510), cloud (9518) and web SSH (9511). The client components, ClusterControl UI or ClusterControl CLI interact with those interfaces to retrieve monitoring data (cluster load, host status, alarms, backup status etc.) or to send management commands (add/remove nodes, run backups, upgrade a cluster, etc.). 

The following diagram illustrates the architecture of ClusterControl:

.. image:: img/cc_arch2.png
   :alt: ClusterControl architecture
   :align: center

ClusterControl has minimal performance impact especially with agent-based monitoring setup and will not cause any downtime to your database server or cluster. In fact, it will perform automatic recovery (if enabled) when it finds a failed database node or cluster.

Features
--------

ClusterControl is able to handle most of the administration tasks required to maintain database servers or clusters. Here are some of the tasks that ClusterControl can perform on your database infrastructure:

* Monitor host statistics (CPU/RAM/disk/network/swap)
* Provision multiple database server/cluster in a single CMON process
* Monitor database's stats, variable, log files, queries, for individual node as well as cluster-wide
* Database configuration management
* Database cluster/node recovery
* Trigger alarm and send notifications
* Schedule and perform database backup (mysqldump, Percona Xtrabackup, MariaDB Backup, pg_dumpall, pg_basebackup, pgBackRest, mongodump, mongodb-consistent-backup)
* Database backup status
* Restore backups
* Verify backup restoration on a standalone host
* MySQL/PostgreSQL/TimeScaleDB point-in-time recovery
* Upload backups to AWS S3/Google Cloud Storage/Azure Storage
* Stop/Start/Bootstrap database service
* Rebuild a database node from a backup to avoid SST
* Deploy a new database server/cluster on-premises or on cloud (AWS, Google Cloud, MS Azure)
* Add existing MySQL/MariaDB server/cluster, MongoDB replica set and PostgreSQL server
* Scale your database cluster (add/remove Galera node, garbd and replication slave)
* Deploy database load balancers (HAProxy, MaxScale, ProxySQL) and virtual IP address (Keepalived)
* Monitor HAProxy/MaxScale/ProxySQL statistics
* Manage MySQL user privileges
* Upgrade MySQL servers
* Promote MySQL/PostgreSQL/TimeScaleDB slave to master
* Set up a delayed slave
* Stage a replication slave from a master or an existing backup
* Manage private keys and certificates for databases' SSL
* Client-server encryption, replication encryption, backup encryption (at-rest or in-transit)
* Create cluster from backup
* Cluster-cluster replication
* and many more..

For more details, please refer to `ClusterControl product page <http://severalnines.com/product/clustercontrol>`_. You might also want to look at the :ref:`ClusterControl changelog <Changelog>` for the latest development update.
