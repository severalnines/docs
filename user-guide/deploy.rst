.. _Deploy Database Cluster:

Deploy Database Cluster
-------------------------

Opens a step-by-step modal dialog to deploy a new set of database cluster. The following database cluster types are supported:

* MySQL Replication
* MySQL Galera
	* MySQL Galera Cluster
	* Percona XtraDB Cluster
	* MariaDB Galera Cluster
* MySQL Group Replication (beta)
* MySQL Cluster (NDB)
* MongoDB ReplicaSet
* MongoDB Sharded Cluster
* PostgreSQL Replication

There are prerequisites that need to be fulfilled prior to the deployment:

* Verify that sudo is working properly if you are using a non-root user. See `Operating System User <../requirements.html#operating-system-user>`_.
* Passwordless SSH is configured from ClusterControl node to all database nodes. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

ClusterControl will trigger a deployment job and the progress can be monitored under *ClusterControl > Activity > Jobs*.

MySQL Replication
+++++++++++++++++

Deploys a new MySQL Replication. The database cluster will be automatically added into ClusterControl once deployed. Minimum of two nodes is required. If only one MySQL IP address or hostname is defined, ClusterControl will deploy it as a standalone MySQL server.

By default, ClusterControl deploys MySQL replication with the following configurations:

* GTID enabled (MySQL and Percona only).
* Start all database nodes with ``read_only=ON``. The chosen master will be promoted by disabling read-only dynamically.
* PERFORMANCE_SCHEMA is disabled.
* Generated account credentials are stored inside ``/etc/mysql/secrets-backup.cnf``.

If you would like to customize the above configurations, modify the template base file to suit your needs before proceed to the deployment. See `Base Template Files <user-guide/mysql/manage.html#base-template-files>`_ for details.

Starting from version 1.4.0, it's possible to setup a master-master replication from scratch under 'Define Topology' tab. You can add more slaves later after the deployment completes.

.. Caution:: ClusterControl sets ``read_only=1`` on all slaves but a privileged user (SUPER) can still write to a slave (except for MySQL versions that support ``super_read_only``).

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

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
	- Select the MySQL version for new deployment. For Oracle, only 5.7 is supported. For Percona, 5.6 and 5.7 while MariaDB, 10.1, 10.2 and 10.3 are supported.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.
	
* **Server Port**
	- MySQL server port. Default is 3306.

* **my.cnf Template**
	- MySQL configuration template file under ``/usr/share/cmon/templates``. Default is ``my.cnf.{version}``. Keep the default is recommended.
	
* **Admin/Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.

* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.

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


MySQL Galera 
+++++++++++++

Deploys a new MySQL Galera Cluster. The database cluster will be automatically added into ClusterControl once deployed. A minimal setup is comprised of one Galera node (no high availability, but this can later be scaled with more nodes). However, the minimum of three nodes is recommended for high availability. Garbd (an arbitrator) can be added later after the deployment completes if needed.

By default, ClusterControl deploys MySQL Galera with the following configurations:

* Use xtrabackup-v2 or mariabackup for ``wsrep_sst_method``.
* PERFORMANCE_SCHEMA is disabled.
* Generated account credentials are stored inside ``/etc/mysql/secrets-backup.cnf``.
* Binary logging is disabled.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

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
	- Select the MySQL version for new deployment. For Percona, 5.6 and 5.7 are supported while MariaDB, 10.1, 10.2 and 10.3 are supported.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.

* **Server Port**
	- MySQL server port. Default is 3306.

* **my.cnf Template**
	- MySQL configuration template file under ``/usr/share/cmon/templates``. Default is ``my.cnf.galera``. Keep it default is recommended.
	
* **Admin/Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Add Node**
	- Specify the IP address or hostname of the MySQL nodes. Press enter to add more nodes. Minimum of three nodes is recommended.

* **Deploy**
	- Starts the Galera Cluster deployment.


MySQL Cluster (NDB)
++++++++++++++++++++

Deploys a new MySQL Cluster (NDB) by Oracle. The cluster consists of management nodes, MySQL API nodes and data nodes. The database cluster will be automatically added into ClusterControl once deployed. Minimum of 4 nodes (2 SQL and management + 2 data nodes) is recommended. 

.. Attention:: Every data node must have at least 1.5 GB of RAM for the deployment to succeed.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

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
	- MySQL configuration template file under ``/usr/share/cmon/templates``. The default is ``my.cnf.mysqlcluster``. Keep it default is recommended.

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

MySQL Group Replication (beta)
++++++++++++++++++++++++++++++

Deploys a new :term:`MySQL Group Replication` cluster by Oracle. This is a beta feature introduced in version 1.4.0. The database cluster will be added into ClusterControl automatically once deployed. A minimum of three nodes is required.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

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
	- Oracle - MySQL Group Replication by Oracle.

* **Version**
	- Select the MySQL version. Group Replication is only available on MySQL 5.7+.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.

* **Server Port**
	- MySQL server port. Default is 3306.

* **my.cnf Template**
	- MySQL configuration template file under ``/usr/share/cmon/templates``. Default is ``my.cnf.grouprepl``. Keep it default is recommended.
	
* **Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Add Nodes**
	- Specify the IP address or hostname of the MySQL nodes. Minimum of three nodes is recommended.

* **Deploy**
	- Starts the MySQL Group Replication deployment.


MongoDB ReplicaSet
+++++++++++++++++++

Deploys a new MongoDB Replica Set. The database cluster will be automatically added into ClusterControl once deployed. Minimum of three nodes (including mongo arbiter) is recommended.

.. Attention:: It is possible to deploy only 2 MongoDB nodes (without arbiter). The caveat of this approach is no automatic failover. If the primary node goes down then manual failover is required to make the other server as primary. Automatic failover works fine with 3 nodes and more.

1) General & SSH Settings
````````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

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
	- The supported MongoDB versions are 3.2, 3.4 and 3.6.

* **Server Data Directory**
	- Location of MongoDB data directory. Default is ``/var/lib/mongodb``.

* **Admin User**
	- MongoDB admin user. ClusterControl will create this user and enable authentication.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **Server Port**
	- MongoDB server port. Default is 27017.

* **mongodb.conf Template**
	- MongoDB configuration template file under ``/usr/share/cmon/templates``. Default is ``mongodb.conf.[vendor]``. Keep it default is recommended.
	
* **ReplicaSet Name**
	- Specify the name of the replica set, similar to ``replication.replSetName`` option in MongoDB.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the MongoDB in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Add Nodes**
	- Specify the IP address or hostname of the MongoDB nodes. Minimum of three nodes is required.

* **Deploy**
	- Starts the MongoDB ReplicaSet deployment.


MongoDB Shards
++++++++++++++

Deploys a new MongoDB Sharded Cluster. The database cluster will be automatically added into ClusterControl once deployed. Minimum of three nodes (including mongo arbiter) is recommended.

.. Warning:: It is possible to deploy only 2 MongoDB nodes (without arbiter). The caveat of this approach is no automatic failover. If the primary node goes down then manual failover is required to make the other server as primary. Automatic failover works fine with 3 nodes and more.

1) General & SSH Settings
````````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

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
	- Specify the IP address or hostname of the MongoDB config servers. Minimum of one node is required, recommended to use three nodes.

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
	- Specify the IP address or hostname of the MongoDB shard servers. Minimum of one node is required, recommended to use three nodes.
	
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
	- The supported MongoDB versions are 3.2, 3.4 and 3.6.

* **Server Data Directory**
	- Location of MongoDB data directory. Default is ``/var/lib/mongodb``.

* **Admin User**
	- MongoDB admin user. ClusterControl will create this user and enable authentication.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **Server Port**
	- MongoDB server port. Default is 27017.

* **mongodb.conf Template**
	- MongoDB configuration template file under ``/usr/share/cmon/templates``. Default is ``mongodb.conf.[vendor]``. Keep it default is recommended.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the MongoDB in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.

* **Deploy**
	- Starts the MongoDB Sharded Cluster deployment.

PostgreSQL
+++++++++++

Deploys a new PostgreSQL standalone or streaming replication cluster from ClusterControl. Only PostgreSQL 9.x and 10 is supported.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.
	
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
	- Supported versions are 9.6 and 10.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Create New Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the PostgreSQL in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
3) Define Topology
```````````````````

* **Master A - IP/Hostname**
	- Specify the IP address of the PostgreSQL master node. Press 'Enter' once specified so ClusterControl can verify the reachability via passwordless SSH.
	
* **Add slaves to master A**
	- Add a slave node connected to master A. Press 'Enter' to add more slave.
	
4) Deployment Summary
`````````````````````

* **Synchronous Replication**
	- Toggle on if you would like to use synchronous streaming replication between the master and the chosen slave. Synchronous replication can be enabled per individual slave node with considerable performance overhead.

* **Deploy**
	- Starts the PostgreSQL standalone or replication deployment.