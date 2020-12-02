.. _Deploy Database Cluster:

Deploy Database Cluster
-------------------------

Opens a step-by-step modal dialog to deploy a new set of database cluster. The following database cluster types are supported:

* MySQL Replication
* MySQL Galera
	* Percona XtraDB Cluster
	* MariaDB Galera Cluster
* MySQL Cluster (NDB)
* TimescaleDB (standalone or streaming replication)
* PostgreSQL (standalone or streaming replication)
* MongoDB ReplicaSet
* MongoDB Sharded Cluster

There are prerequisites that need to be fulfilled prior to the deployment:

* Passwordless SSH is configured from ClusterControl node to all database nodes. See :ref:`Requirements - Passwordless SSH`.
* Verify that sudo is working properly if you are using a non-root user. See :ref:`Requirements - Operating System User`.

ClusterControl will trigger a deployment job and the progress can be monitored under *ClusterControl > Activity > Jobs*.

.. _Deploy - MySQL Replication:

MySQL Replication
+++++++++++++++++

Deploys a new MySQL Replication or a standalone MySQL server. The database cluster will be automatically added into ClusterControl once deployed. A minimum of two nodes is required for MySQL replication. If only one MySQL IP address or hostname is defined, ClusterControl will deploy it as a standalone MySQL server with binary log enabled.

By default, ClusterControl deploys MySQL replication with the following configurations:

* MySQL GTID with ``log_slave_updates`` enabled (MySQL and Percona only).
* MariaDB GTID with ``log_slave_updates`` enabled (MariaDB only).
* All database nodes will be configured with ``read_only=ON`` and ``super_read_only=ON`` (if supported). The chosen master will be promoted by disabling the read-only in the runtime.
* ``PERFORMANCE_SCHEMA`` is disabled.
* ClusterControl will create and grant necessary privileges for two MySQL/MariaDB users – ``cmon`` for monitoring and management and ``backupuser`` for backup and restore purposes.
* The generated account credentials are stored inside ``secrets-backup.cnf under`` the MySQL configuration directory.
* ClusterControl will configure semi-synchronous replication.

If you would like to customize the above configurations, modify the template base file to suit your needs before proceed to the deployment. See :ref:`MySQL - Manage - Configurations - Base Template Files` for details.

Starting from version 1.4.0, it's possible to setup a master-master replication from scratch under 'Define Topology' tab. You can add more slaves later after the deployment completes.

.. Caution:: ClusterControl sets ``read_only=1`` on all slaves but a privileged user (SUPER) can still write to a slave (except for MySQL versions that support ``super_read_only``).

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See :ref:`Requirements - Operating System User`.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See :ref:`Requirements - Passwordless SSH`.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port**
	- Specify the SSH port for the target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
* **Cluster Name**
	- Specify a name for the cluster.

* **Install Software**
  - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
  - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled (recommended).

2) Define MySQL Servers
````````````````````````
    
* **Vendor**
	- Percona - Percona Server by Percona
	- MariaDB - MariaDB Server by MariaDB
	- Oracle - MySQL Server by Oracle

* **Version**
	- Select the MySQL version for new deployment. For Oracle, only 5.7 and 8.0 are supported. For Percona, 5.6, 5.7 and 8.0 while MariaDB, 10.1, 10.2, 10.3, 10.4 and 10.5 are supported.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.
	
* **Server Port**
	- MySQL server port. Default is 3306.

* **my.cnf Template**
	- MySQL configuration template file under ``/etc/cmon/templates`` or ``/usr/share/cmon/templates``. See :ref:`MySQL - Manage - Configurations - Base Template Files` for details.
	
* **Admin/Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.

* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already set up on the nodes. User has to set up the software repository manually on every database node as well as all future database/load balancer nodes in this cluster. Commonly, this is the best option if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members. Depending on the database vendor and version, this might eat up the disk space of the ClusterControl host significantly.

3) Define Topology
````````````````````

* **Master A - IP/Hostname**
	- Specify the IP address of the primary MySQL master node.
	
* **Add slaves to master A**
	- Add a slave node connected to master A. Press enter to add more slaves.

* **Add Second Master Node**
	- Opens the add node wizard for secondary MySQL master node.

* **Master B - IP/Hostname**
	- Only available if you click *Add Second Master Node*.
	- Specify the IP address of the other MySQL master node. ClusterControl will setup a master-master replication between these nodes. Master B will be read-only once deployed (secondary master), letting Master A to hold the write role (primary master) for the replication chain.
	
* **Add slaves to master B**
	- Only available if you click *Add Second Master Node*.
	- Add a slave node connected to master B. Press 'Enter' to add more slave.
	
* **Deploy**
	- Starts the MySQL Replication deployment.

.. _Deploy - MySQL Galera:

MySQL Galera 
+++++++++++++

Deploys a new MySQL Galera Cluster. The database cluster will be automatically added into ClusterControl once deployed. A minimal setup is comprised of one Galera node (no high availability, but this can later be scaled with more nodes). However, a minimum of three nodes is recommended for high availability. Garbd (an arbitrator) can be added later after the deployment completes if needed.

By default, ClusterControl deploys MySQL Galera with the following configurations:

* Use xtrabackup-v2 or mariabackup (depending on the vendor chosen) for ``wsrep_sst_method``.
* All database nodes will be configured with ``read_only=ON`` and ``super_read_only=ON`` (if supported). The chosen master will be promoted by disabling the read-only in the runtime.
* ``PERFORMANCE_SCHEMA`` is disabled.
* Binary logging is disabled.
* ClusterControl will create and grant necessary privileges for two MySQL/MariaDB users – ``cmon`` for monitoring and management and ``backupuser`` for backup and restore purposes.
* The generated account credentials are stored inside ``secrets-backup.cnf under`` the MySQL configuration directory.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See :ref:`Requirements - Operating System User`.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See :ref:`Requirements - Passwordless SSH`.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
* **Cluster Name**
	- Specify a name for the cluster.

* **Install Software**
  - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
  - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled (recommended).

2) Define MySQL Servers
````````````````````````
    
* **Vendor**
	- Percona - Percona XtraDB Cluster by Percona
	- MariaDB - MariaDB Server (Galera embedded) by MariaDB

* **Version**
	- Select the MySQL version for new deployment. For Percona, 5.6 and 5.7, 8.0 are supported while MariaDB, 10.1, 10.2, 10.3, 10.4 and 10.5 are supported.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.

* **Server Port**
	- MySQL server port. Default is 3306.

* **my.cnf Template**
	- MySQL configuration template file under ``/etc/cmon/templates`` or ``/usr/share/cmon/templates``. See :ref:`MySQL - Manage - Configurations - Base Template Files` for details.
	
* **Admin/Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Add Node**
	- Specify the IP address or hostname of the MySQL nodes. Press 'Enter' once specified so ClusterControl can verify the node reachability via passwordless SSH. A minimum of three nodes is recommended.

* **Deploy**
	- Starts the Galera Cluster deployment.

.. _Deploy - MySQL Cluster:

MySQL Cluster (NDB)
++++++++++++++++++++

Deploys a new MySQL Cluster (NDB) by Oracle. The cluster consists of management nodes, MySQL API nodes and data nodes. The database cluster will be automatically added into ClusterControl once deployed. A minimum of 4 nodes (2 SQL and management + 2 data nodes) is recommended. 

.. Attention:: Every data node must have at least 1.5 GB of RAM for the deployment to succeed.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See :ref:`Requirements - Operating System User`.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See :ref:`Requirements - Passwordless SSH`.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
* **Cluster Name**
	- Specify a name for the cluster.

* **Install Software**
  - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
  - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled (recommended).

2) Define Management Servers
``````````````````````````````
    
* **Server Port**
	- MySQL Cluster management port. Default to 1186.

* **Server Data Directory**
	- MySQL Cluster data directory for NDB. Default is ``/var/lib/mysql-cluster``.

* **Management Server 1**
	- Specify the IP address or hostname of the first management server.

* **Management Server 2**
	- Specify the IP address or hostname of the second management server.

3) Define Data Nodes
``````````````````````

* **Server Port**
	- MySQL Cluster data node port. Default to 2200.

* **Server Data Directory**
	- MySQL Cluster data directory for NDB. Default is ``/var/lib/mysql-cluster``.

* **Add Nodes**
	- Specify the IP address or hostname of the MySQL Cluster data node. It's recommended to have data nodes in pair. You can add up to 14 data nodes to your cluster. Every data node must have at least 1.5GB of RAM.

4) Define MySQL Servers
````````````````````````

* **my.cnf Template**
	- MySQL configuration template file under ``/etc/cmon/templates`` or ``/usr/share/cmon/templates``. See :ref:`MySQL - Manage - Configurations - Base Template Files` for details.

* **Server Port**
	- MySQL server port. Default to 3306.
	
* **Server Data Directory**
	- MySQL data directory. Default is ``/var/lib/mysql``.

* **Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all nodes in the cluster.

* **Add Nodes**
	- Specify the IP address or hostname of the MySQL Cluster API node. You can use the same IP address with management node, co-locate both roles in a same host.

* **Deploy**
	- Starts the MySQL Cluster deployment.

.. _Deploy - TimeScaleDB:

TimeScaleDB
+++++++++++

Deploys a new TimeScaleDB standalone or streaming replication cluster. Only TimeScaleDB 9.6 and later are supported. A minimum of two nodes is required for TimeScaleDB streaming replication.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See :ref:`Requirements - Operating System User`.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See :ref:`Requirements - Passwordless SSH`.
	
* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Cluster Name**
	- Specify a name for the database.

* **Install Software**
  - Check the box if you use clean and minimal VMs. Existing PostgreSQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
  - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled (recommended).

2) Define PostgreSQL Servers
````````````````````````````

* **Server Port**
	- PostgreSQL server port. Default is 5432.

* **User**
	- Specify the PostgreSQL super user for example, postgres.

* **Password**
	- Specify the password for *User*.

* **Version**
	- Supported versions are 9.6, 10, 11 and 12.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Create New Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the PostgreSQL in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
3) Define Topology
```````````````````

* **Master A - IP/Hostname**
	- Specify the IP address of the TimeScaleDB master node. Press 'Enter' once specified so ClusterControl can verify the node reachability via passwordless SSH.
	
* **Add slaves to master A**
	- Add a slave node connected to master A. Press 'Enter' to add more slave.
	
4) Deployment Summary
`````````````````````

* **Synchronous Replication**
	- Toggle on if you would like to use synchronous streaming replication between the master and the chosen slave. Synchronous replication can be enabled per individual slave node with considerable performance overhead.

* **Deploy**
	- Starts the TimeScaleDB standalone or replication deployment.


.. _Deploy - PostgreSQL:

PostgreSQL
+++++++++++

Deploys a new PostgreSQL standalone or streaming replication cluster from ClusterControl. Only PostgreSQL 9.6 and later are supported. A minimum of two nodes is required for PostgreSQL streaming replication.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See :ref:`Requirements - Operating System User`.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See :ref:`Requirements - Passwordless SSH`.
	
* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Cluster Name**
	- Specify a name for the database.

* **Install Software**
  - Check the box if you use clean and minimal VMs. Existing PostgreSQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
  - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled (recommended).

2) Define PostgreSQL Servers
````````````````````````````

* **Server Port**
	- PostgreSQL server port. Default is 5432.

* **User**
	- Specify the PostgreSQL super user for example, postgres.

* **Password**
	- Specify the password for *User*.

* **Version**
	- Supported versions are 9.6, 10, 11 and 12.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Create New Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the PostgreSQL in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
3) Define Topology
```````````````````

* **Master A - IP/Hostname**
	- Specify the IP address of the PostgreSQL master node. Press 'Enter' once specified so ClusterControl can verify the node reachability via passwordless SSH.
	
* **Add slaves to master A**
	- Add a slave node connected to master A. Press 'Enter' to add more slave.
	
4) Deployment Summary
`````````````````````

* **Synchronous Replication**
	- Toggle on if you would like to use synchronous streaming replication between the master and the chosen slave. Synchronous replication can be enabled per individual slave node with considerable performance overhead.

* **Deploy**
	- Starts the PostgreSQL standalone or replication deployment.

.. _Deploy - MongoDB ReplicaSet:

MongoDB ReplicaSet
+++++++++++++++++++

Deploys a new MongoDB Replica Set. The database cluster will be automatically added into ClusterControl once deployed. A minimum of three nodes (including mongo arbiter) is recommended.

.. Attention:: It is possible to deploy only 2 MongoDB nodes (without arbiter). The caveat of this approach is no automatic failover. If the primary node goes down then manual failover is required to make the other server as primary. Automatic failover works fine with 3 nodes and more.

1) General & SSH Settings
````````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See :ref:`Requirements - Operating System User`.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See :ref:`Requirements - Passwordless SSH`.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Cluster Name**
	- Specify a name for the cluster.
	
* **Install Software**
  - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
  - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled (recommended).

2) Define MongoDB Servers
````````````````````````````
    
* **Vendor**
	- Percona - Percona Server for MongoDB by Percona.
	- MongoDB - MongoDB Server by MongoDB Inc.

* **Version**
	- The supported MongoDB versions are 3.6, 4.0 and 4.2.

* **Server Data Directory**
	- Location of MongoDB data directory. Default is ``/var/lib/mongodb``.

* **Admin User**
	- MongoDB admin user. ClusterControl will create this user and enable authentication.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **Server Port**
	- MongoDB server port. Default is 27017.

* **mongodb.conf Template**
	- MongoDB configuration template file under ``/etc/cmon/templates`` or ``/usr/share/cmon/templates``. See :ref:`MongoDB - Manage - Configurations - Base Template Files` for details.
	
* **ReplicaSet Name**
	- Specify the name of the replica set, similar to ``replication.replSetName`` option in MongoDB.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the MongoDB in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Add Nodes**
	- Specify the IP address or hostname of the MongoDB nodes. A minimum of three nodes is required.

* **Deploy**
	- Starts the MongoDB ReplicaSet deployment.

.. _Deploy - MongoDB Shards:

MongoDB Shards
++++++++++++++

Deploys a new MongoDB Sharded Cluster. The database cluster will be automatically added into ClusterControl once deployed. A minimum of three nodes (including mongo arbiter) is recommended.

.. Warning:: It is possible to deploy only 2 MongoDB nodes (without arbiter). The caveat of this approach is no automatic failover. If the primary node goes down then manual failover is required to make the other server as primary. Automatic failover works fine with 3 nodes and more.

1) General & SSH Settings
````````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See :ref:`Requirements - Operating System User`.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See :ref:`Requirements - Passwordless SSH`.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Cluster Name**
	- Specify a name for the cluster.

* **Install Software**
  - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
  - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (RedHat/CentOS) if enabled (recommended).

2) Configuration Servers and Routers
``````````````````````````````````````````
    
*Configuration Server*

* **Server Port**
	- MongoDB config server port. Default is 27019.

* **Add Configuration Servers**
	- Specify the IP address or hostname of the MongoDB config servers. A minimum of one node is required, recommended to use three nodes.

*Routers/Mongos*

* **Server Port**
	- MongoDB mongos server port. Default is 27017.

* **Add More Routers**
	- Specify the IP address or hostname of the MongoDB mongos.

3) Define Shards
`````````````````
    
* **Replica Set Name**
	- Specify a name for this replica set shard.

* **Server Port**
	- MongoDB shard server port. Default is 27018.

* **Add Node**
	- Specify the IP address or hostname of the MongoDB shard servers. A minimum of one node is required, recommended to use three nodes.
	
* **Advanced Options**
	- Click on this to open set of advanced options for this particular node in this shard:
		- Add slave delay - Specify the amount of delayed slave in milliseconds format.
		- Act as an arbiter - Toggle to 'Yes' if the node is arbiter node. Otherwise, choose 'No'.

* **Add Another Shard**
  - Create another shard. You can then specify the IP address or hostname of MongoDB server that falls under this shard.
	
4) Database Settings
``````````````````````

* **Vendor**
	- Percona - Percona Server for MongoDB by Percona
	- MongoDB - MongoDB Server by MongoDB Inc

* **Version**
	- The supported MongoDB versions are 3.6, 4.0 and 4.2.

* **Server Data Directory**
	- Location of MongoDB data directory. Default is ``/var/lib/mongodb``.

* **Admin User**
	- MongoDB admin user. ClusterControl will create this user and enable authentication.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **Server Port**
	- MongoDB server port. Default is 27017.

* **mongodb.conf Template**
	- 	- MongoDB configuration template file under ``/etc/cmon/templates`` or ``/usr/share/cmon/templates``. See :ref:`MongoDB - Manage - Configurations - Base Template Files` for details.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the MongoDB in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.

* **Deploy**
	- Starts the MongoDB Sharded Cluster deployment.
