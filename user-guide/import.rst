Import Existing Server/Cluster
------------------------------

Opens a wizard to import the existing database setup into ClusterControl. The following database cluster types are supported:

* MySQL Replication
* MySQL Galera
	* MySQL Galera Cluster
	* Percona XtraDB Cluster
	* MariaDB Galera Cluster
* MySQL Cluster (NDB)
* MongoDB ReplicaSet
* MongoDB Shards
* PostgreSQL (standalone or replication)

There are some prerequisites that need to be fulfilled prior to adding the existing setup. The existing database cluster/server must:

* Verify that sudo is working properly if you are using a non-root user. See 
* Passwordless SSH from ClusterControl node to database nodes has been configured correctly. See
* The target cluster must not be in degraded state. For example, if you have a three-node Galera cluster, all nodes must be alive, accessible and in synced.

For more details, refer to the `Requirement <../requirements.html>`_ section. Each time you add an existing cluster or server, ClusterControl will trigger a job under *ClusterControl > Settings > Cluster Jobs*. You can see the progress and status under this page. A window will also appear with messages showing the progress.

Import Existing MySQL Replication
+++++++++++++++++++++++++++++++++

ClusterControl is able to manage/monitor an existing set of MySQL servers (standalone or replication). Individual hosts specified in the same list will be added to the same server group in the UI. ClusterControl assumes that you are using the same MySQL root password for all instances specified in the group and it will attempt to determine the server role as well (master, slave, multi or standalone).

When importing an existing MySQL Replication, ClusterControl will do the following:

* Verify SSH connectivity to all nodes.
* Detect the host environment and operating system.
* Discover the database role of each node (master, slave, multi, standalone).
* Pull the configuration files.
* Generate the authentication key and register the node into ClusterControl.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

2) Define MySQL Servers
``````````````````````````

* **Vendor**
	- Percona for Percona Server
	- MariaDB for MariaDB Server
	- Oracle for MySQL Server

* **MySQL Version**
	- Supported version:
		- Percona Server (5.5, 5.6, 5.7)
		- MariaDB Server (10.1, 10.2)
		- MySQL Server (5.7)

* **Basedir**
	- MySQL base directory. Default is ``/usr``. ClusterControl assumes all MySQL nodes are using the same base directory.

* **Server Port**
	- MySQL port on the target server/cluster. Default to 3306. ClusterControl assumes MySQL is running on the same port on all nodes.

* **Admin/Root User**
	- MySQL user on the target server/cluster. This user must able to perform GRANT statement. Recommended to use MySQL 'root' user.
	
* **Admin/Root Password**
	- Password for *MySQL User*. ClusterControl assumes that you are using the same MySQL root password for all instances specified in the group.

* **"information_schema" Queries**
	- Toggle on to enable information_schema queries to track databases and tables growth. Queries to the information_schema may not be suitable when having many database objects (hundreds of databases, hundreds of tables in each database, triggers, users, events, stored procedures, etc). If disabled, the query that would be executed will be logged so it can be determined if the query is suitable in your environment.
	- This is not recommended for clusters with more than 2000 database objects.

* **Node AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects any of the nodes in the cluster is down.
	
* **Cluster AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects the cluster is down or degraded.

* **Import as Standalone Nodes**
	- Toggle on if you only importing a standalone node (by specifying only one node under 'Add Nodes' section).

* **Add Nodes**
	- Enter the MySQL single instances' IP address or hostname that you want to group under this cluster.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the MySQL instances, import configurations and start managing them. 

Import Existing MySQL Galera
++++++++++++++++++++++++++++

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
2) Define MySQL Servers
``````````````````````````

* **Vendor**
	- Percona XtraDB - Percona XtraDB Cluster by Percona
	- MariaDB - MariaDB Galera Cluster by MariaDB
	- Codership - MySQL Galera Cluster by Codership

* **MySQL Version**
	- Select MySQL version of the target cluster.

* **Basedir**
	- MySQL base directory. Default is ``/usr``. ClusterControl assumes MySQL is having the same base directory on all nodes.

* **Port**
	- MySQL port on the target cluster. Default to 3306. ClusterControl assumes MySQL is running on the same port on all nodes.

* **Admin/Root User**
	- MySQL user on the target cluster. This user must be able to perform GRANT statement. Recommended to use MySQL 'root' user.
	
* **Admin/Root Password** 
	- Password for *MySQL User*. The password must be the same on all nodes that you want to add into ClusterControl.

* **"information_schema" Queries**
	- Toggle on to enable information_schema queries to track databases and tables growth. Queries to the information_schema may not be suitable when having many database objects (hundreds of databases, hundreds of tables in each database, triggers, users, events, stored procedures, etc). If disabled, the query that would be executed will be logged so it can be determined if the query is suitable in your environment.
	- This is not recommended for clusters with more than 2000 database objects.
	
* **Node AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects any of the nodes in the cluster is down.
	
* **Cluster AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects the cluster is down or degraded.

* **Automatic Node Discovery**
	- You only need to specify ONE Galera node and ClusterControl will discover the rest based on ``wsrep_cluster_address``.

* **Add Node**
	- Specify the target node and press 'Enter' for each of them. If you have *Automatic Node Discovery* enabled, you only need to specify only one node.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the Galera node, discover the configuration for the rest of the members and start managing/monitoring the cluster.


Import Existing MySQL Cluster
+++++++++++++++++++++++++++++

ClusterControl is able to manage and monitor an existing production deployed MySQL Cluster (NDB). Minimum of 2 management nodes and 2 data nodes is required. 

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

2) Define Management Server 
````````````````````````````

* **Management server 1**
	- Specify the IP address or hostname of the first MySQL Cluster management node.

* **Management server 2**
	- Specify the IP address or hostname of the second MySQL Cluster management node.

* **Server Port**
	- MySQL Cluster management port. The default port is 1186.


3) Define Data Nodes
``````````````````````````

* **Port**
	- MySQL Cluster data node port. The default port is 2200.

* **Add Nodes**
	- Specify the IP address or hostname of the MySQL Cluster data node.

4) Define MySQL Servers
``````````````````````````
	
* **Root Password** 
	- MySQL root password.
	
* **Server Port**
	- MySQL port. Default to 3306.

* **MySQL Installation Directory**
	- MySQL server installation path where ClusterControl can find the ``mysql`` binaries.

* **Enable information_schema Queries**
	-	Toggle on to enable information_schema queries to track databases and tables growth. Queries to the information_schema may not be suitable when having many database objects (hundreds of databases, hundreds of tables in each database, triggers, users, events, stored procedures, etc). If disabled, the query that would be executed will be logged so it can be determined if the query is suitable in your environment.
	- This is not recommended for clusters with more than 2000 database objects.
	
* **Enable Node AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects any of the nodes in the cluster is down.
	
* **Enable Cluster AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects the cluster is down or degraded.

* **Add Nodes**
	- Specify the IP address or hostname of the MySQL Cluster API/SQL node.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the MySQL Cluster nodes, discover the configuration for the rest of the nodes and start managing/monitoring the cluster.

Import Existing MongoDB ReplicaSet
+++++++++++++++++++++++++++++++++++

ClusterControl is able to manage and monitor an existing MongoDB/Percona Server for MongoDB 3.x replica set.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
2) Define MongoDB Servers
``````````````````````````

* **Vendor**
	- Percona - Percona Server for MongoDB by Percona (formerly Tokutek).
	- MongoDB - MongoDB Server by MongoDB Inc (formerly 10gen).

* **Version**
	- The supported version is 3.2.

* **Server Port**
	- MongoDB server port. Default is 27017.

* **Admin User**
	- MongoDB admin user.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **MongoDB Auth DB**
	- MongoDB database to authenticate against. Default is ``admin``.

* **Hostname**
	- Specify one IP address or hostname of the MongoDB replica set member. ClusterControl will automatically discover the rest.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the specified MongoDB node, discover the configuration for the rest of the nodes and start managing/monitoring the cluster.

Import Existing MongoDB Shards
+++++++++++++++++++++++++++++++

ClusterControl is able to manage and monitor an existing MongoDB/Percona Server for MongoDB 3.x sharded cluster setup.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

2) Set Routers/Mongos
``````````````````````````
    
*Configuration Server*

* **Server Port**
	- MongoDB mongos server port. Default is 27017.

* **Add More Routers**
	- Specify the IP address or hostname of the MongoDB mongos.
	
3) Database Settings
``````````````````````````

* **Vendor**
	- Percona - Percona Server for MongoDB by Percona
	- MongoDB - MongoDB Server by MongoDB Inc

* **Version**
	- The supported version is 3.2.

* **Admin User**
	- MongoDB admin user.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **MongoDB Auth DB**
	- MongoDB database to authenticate against. Default is ``admin``.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the specified MongoDB mongos, discover the configuration for the rest of the members and start managing/monitoring the cluster.

Import Existing PostgreSQL
++++++++++++++++++++++++++

ClusterControl is able to manage/monitor an existing set of PostgreSQL 9.x servers. Individual hosts specified in the same list will be added to the same server group in the UI. ClusterControl assumes that you are using the same postgres password for all instances specified in the group.

1) General & SSH Settings
``````````````````````````

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

2) Define PostgreSQL Servers
``````````````````````````````

* **Server Port**
	- PostgreSQL port on the target server/cluster. Default to 5432. ClusterControl assumes PostgreSQL is running on the same port on all nodes.

* **User**
	- PostgreSQL user on the target server/cluster. Recommended to use PostgreSQL 'postgres' user.

* **Password**
	- Password for *User*. ClusterControl assumes that you are using the same postgres password for all instances under this group.
	
* **Version**
	- PostgreSQL server version on the target server/cluster.

* **Basedir**
	- PostgreSQL base directory. Default is ``/usr``. ClusterControl assumes all PostgreSQL nodes are using the same base directory.

* **Add Node**
	- Specify all PostgreSQL instances that you want to group under this cluster.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the PostgreSQL instances, import configurations and start managing them.

