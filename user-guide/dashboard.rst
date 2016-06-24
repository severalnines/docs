Dashboard
============

This page is the landing page once you logged in. It provides a summary of database clusters monitored under ClusterControl.

.. image:: img/cc_cluster_list_13.png
   :align: center

*Section 1*

ClusterControl's top menu.

* **Global Notifications**
	- Provides aggregated view of all alarms and notifications raised across all clusters monitored by ClusterControl.

* **Global Messages**
	- Global list of jobs that have been performed across clusters (e.g., deploying a new cluster, adding an existing cluster and cloning). Pick a job to see its running messages.

* **Settings**
	- See `Settings <admin_settings.html>`_ section.

* **Help**
	- Open ClusterControl online documentation page at http://www.severalnines.com/docs
	- Open Severalnines community support forums at http://support.severalnines.com/forums

* **Logout**
	- Logs out from ClusterControl and return to login page.

*Section 2*

Database server/cluster deployment functions.

* **Add Existing Server/Cluster**
	- See `Add Existing Server/Cluster`_ section.

* **Create Database Cluster**
	- See `Create Database Cluster`_ section.

* **Create Database Node**
	- See `Create Database Node`_ section.
	
*Section 3*

List of database clusters managed under ClusterControl with summarized status. Database cluster deployed by (or added into) ClusterControl will be listed in this page. See `Database Cluster List`_ section.

Add Existing Server/Cluster
----------------------------

Opens a single-page wizard to import the configuration of the existing database setup into ClusterControl. The following database cluster types are supported:

* Galera (MySQL Galera Cluster, Percona XtraDB Cluster and MariaDB Galera Cluster)
* MySQL Replication (master-slave)
* A pool of single-instance MySQL servers
* MongoDB/TokuMX replica set
* PostgreSQL single-instance

There are some prerequisites that need to be fulfilled prior to adding the existing setup. The existing database cluster/server must:

* Verify that sudo is working properly if you are using a non-root user
* Passwordless SSH from ClusterControl node to database nodes has been configured correctly
* The target server/cluster must not in degraded state. For example, if you have a three-node Galera cluster, all nodes must alive and in synced.

For more details, refer to the `Requirement <../../requirements.html>`_ section. Each time you add an existing cluster or server, ClusterControl will trigger a job under *ClusterControl > Settings > Cluster Jobs*. You can see the progress and status under this page. A window will also appear with messages showing the progress.

Add Existing Galera Cluster
````````````````````````````

Choose *MySQL Galera Cluster* as the database type. Fill in all required information.

* **Vendor**
	- Codership - MySQL Galera Cluster by Codership
	- Percona XtraDB - Percona XtraDB Cluster by Percona
	- MariaDB - MariaDB Galera Cluster by MariaDB

* **MySQL Version**
	- Select MySQL version of the target cluster

* **Hostname**
	- Please note that you only need to specify ONE Galera node and ClusterControl will discover the rest based on ``wsrep_cluster_address``.

* **User**
	- MySQL user on the target server/cluster. This user must able to perform GRANT statement. Recommended to use MySQL 'root' user.
	
* **Password** 
	- Password for *MySQL User*. The password must be the same on all nodes that you want to add into ClusterControl.

* **Port**
	- MySQL port on the target server/cluster. Default to 3306. ClusterControl assumes MySQL is running on the same port on all nodes.
	
* **Basedir**
	- MySQL base directory. Default is ``/usr``. ClusterControl assumes MySQL is having the same base directory on all nodes.

* **Enable information_schema Queries**
	- Use information_schema to query MySQL statistics. This is not recommended for clusters with more than 2000 tables/databases.
	
* **Enable Node AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects any of the nodes in the cluster is down.
	
* **Enable Cluster AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects the cluster is down or degraded.

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.

* **Add Cluster**
	- Click the button to start the import. ClusterControl will connect to the Galera node, discover the configuration for the rest of the nodes and start managing/monitoring the cluster.

Add existing MySQL server/replication
``````````````````````````````````````

ClusterControl is able to manage/monitor an existing set of MySQL servers (standalone or replication). Individual hosts specified in the same list will be added to the same server group in the UI. ClusterControl assumes that you are using the same MySQL root password for all instances specified in the group, and it will determine the server role (master, slave, multi or standalone).

Choose *MySQL Server* as the database type. Fill in all required information.


* **User**
	- MySQL user on the target server/cluster. This user must able to perform GRANT statement. Recommended to use MySQL 'root' user.
	
* **Password**
	- Password for *MySQL User*. The user must have ability to perform GRANT ClusterControl assumes that you are using the same MySQL root password for all instances specified in the group.

* **Port**
	- MySQL port on the target server/cluster. Default to 3306. ClusterControl assumes MySQL is running on the same port on all nodes.

* **Basedir**
	- MySQL base directory. Default is ``/usr``. ClusterControl assumes all MySQL nodes are using the same base directory.

* **Add Host**
	- Enter the MySQL single instances' IP address or hostname that you want to group under this cluster.

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.

* **Add Cluster**
	- Click the button to start the import. ClusterControl will connect to the MySQL instances, import configurations and start managing them. 

Add existing MySQL Cluster
````````````````````````````

ClusterControl is able to manage and monitor an existing production deployed MySQL Cluster (NDB). Minimum of 2 management nodes and 2 data nodes is required. Choose *MySQL/NDB Cluster* as the database type. Fill in all required information.

Choose *MySQL Galera Cluster* as the database type. Fill in all required information.


* **Hostname**
	- Please note that you only need to specify ONE Galera node and ClusterControl will discover the rest based on wsrep_cluster_address.
	
* **Basedir**
	- MySQL base directory. Default is ``/usr``. ClusterControl assumes MySQL is having the same base directory on all nodes.

* **Enable information_schema Queries**
	- Use information_schema to query MySQL statistics. This is not recommended for clusters with more than 2000 tables/databases.
	
* **Enable Node AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects any of the nodes in the cluster is down.
	
* **Enable Cluster AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects the cluster is down or degraded.
  
* **Management Servers**

  * **Management server 1**
		- Specify the IP address or hostname of the first MySQL Cluster management node.

  * **Management server 2**
		- Specify the IP address or hostname of the second MySQL Cluster management node.

  * **Port**
		- Cluster management port. The default port is 1186.

* **Data Node**

  * **Data Node server 1**
		- Specify the IP address or hostname of the MySQL Cluster data node.

  * **Data Node server 2**
		- Specify the IP address or hostname of the MySQL Cluster data node.

  * **Add two more Data Nodes**
		- It's recommended to have data nodes in pair. If you would like to add two data nodes, click this button to add two more input fields.

  * **Remove last two Data Nodes**
		- Remove the last two input fields for Data Node server.

  * **Port**
		- Data node port. The default port is 2200.

* **SQL Node**

  * **MySQL server 1**
		- Specify the IP address or hostname of the MySQL Cluster SQL node.

  * **MySQL server 2**
		- Specify the IP address or hostname of the MySQL Cluster SQL node.

  * **Add SQL Node**
		- Add one more input field for MySQL server.

  * **Remove SQL Node**
		- Remove the last input field for MySQL server.

  * **Server Port**
		- MySQL port. Default to 3306.

  * **MySQL Installation Directory**
		- MySQL server installation path.

  * **Root Password** 
		- MySQL root password.
  
* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands.

* **Add Cluster**
	- Click the button to start the import. ClusterControl will connect to the Galera node, discover the configuration for the rest of the nodes and start managing/monitoring the cluster.

Add existing MongoDB replica set
`````````````````````````````````

ClusterControl is able to manage and monitor an existing MongoDB 3.x replica set. Choose *Mongodb Replicaset* as the database type. Fill in all required information.

* **Vendor**
	- Percona - Percona Server for MongoDB by Percona. (formerly Tokutek)
	- MongoDB - MongoDB Server by MongoDB Inc. (formerly 10gen)

* **Hostname**
	- Specify one IP address or hostname of the MongoDB replica set member. ClusterControl will automatically discover the rest. 

* **User**
	- Specify the MongoDB root user. The user must have equivalent to built-in superuser role access for MongoDB 3.x.

* **Password**
	- Specify password for **User**.

* **Port**
	- MongoDB port on the target cluster. Default to 27017. ClusterControl assumes MongoDB is running on the same port on all nodes.

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or sudo user has no sudo password.

* **Add Cluster**
	- Click the button to start the import. ClusterControl will connect to the specified MongoDB node, discover the configuration for the rest of the nodes and start managing/monitoring the cluster.


Add existing PostgreSQL servers
````````````````````````````````

ClusterControl is able to manage/monitor an existing set of PostgreSQL 9.x servers (standalone). Individual hosts specified in the same list will be added to the same server group in the UI. ClusterControl assumes that you are using the same postgres password for all instances specified in the group.

Choose Postgres Server as the database type. Fill in all required information.

* **User**
	- PostgreSQL user on the target server/cluster. Recommended to use PostgreSQL 'postgres' user.

* **Password**
	- Password for *Postgres User*. ClusterControl assumes that you are using the same postgres password for all instances specified in the group.

* **Port**
	- PostgreSQL port on the target server/cluster. Default to 5432. ClusterControl assumes PostgreSQL is running on the same port on all nodes.

* **Basedir**
	- PostgreSQL base directory. Default is ``/usr``. ClusterControl assumes all PostgreSQL nodes are using the same base directory.

* **Add Host**
	- Specify all MySQL single instances that you want to group under this cluster.

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.

* **Add Cluster**
	- Click the button to start the import. ClusterControl will connect to the PostgreSQL instances, import configurations and start managing them. 

Create Database Cluster
------------------------

Opens a wizard to deploy a new set of database cluster. The following database cluster types are supported:

* MySQL Replication (master-slave)
* Galera Cluster (MySQL Galera Cluster, Percona XtraDB Cluster and MariaDB Galera Cluster)
* MySQL Cluster (NDB)

There are some prerequisites that need to be fulfilled prior to adding the existing setup. The existing database cluster/server must:

* Verify that sudo is working properly if you are using a non-root user
* Passwordless SSH from ClusterControl node to database nodes has been configured correctly

For more details, refer to the `Requirement <../../requirements.html>`_ section. Each time you create a database cluster, ClusterControl will trigger a job under *ClusterControl > Settings > Cluster Jobs*. You can see the progress and status under this page. A window will also appear with messages showing the progress.


MySQL Replication Cluster
``````````````````````````

Deploys a new MySQL Replication. The database cluster will be automatically added into ClusterControl once deployed. Minimum of two nodes is required. You can add more slaves later after the deployment completes.

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

* **Install Software**
    - Yes - Use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - No - Existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.
    
* **Master Node**
	- Specify the IP address or hostname of the MySQL master node.
  
* **Slave Node #**
	- Specify the IP address or hostname of the MySQL slave node.
    
* **Add Slave Node**
    - Add a new input box for slave node.

* **Remove last Slave Node**
    - Remove the last input box for DB node.

* **Vendor**
	- Percona XtraDB - Percona Server by Percona
	- MariaDB - MariaDB Server by MariaDB
	- Oracle - MySQL Server by Oracle

* **Version**
	- Select the MySQL version. For Oracle, only 5.7 is supported. For Percona, 5.6 and 5.7 are available. If you choose MariaDB, only 10.1 will be available.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.

* **Server Port**
	- MySQL port for all nodes. Default is 3306.

* **My.cnf Template**
	- MySQL configuration template file under ``/usr/share/cmon/templates``. Default is my.cnf.repl[version]. Keep it default is recommended.
	
* **Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Deploy**
	- Starts the MySQL Replication Cluster deployment.


MySQL Galera Cluster
``````````````````````

Deploys a new Galera Cluster. The database cluster will be automatically added into ClusterControl once deployed. Minimum of three nodes (excluding garbd) is required. Garbd can be added later after the deployment completes.

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

* **Install Software**
    - Yes - Use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - No - Existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.
    
* **DB Node #**
	- Specify the IP address or hostname of the database nodes. A minimum of three servers is required to handle split brain/network partitioning.
    
* **Add DB Node**
    - Add a new input box for DB node.

* **Remove last DB Node**
    - Remove the last input box for DB node.

* **Vendor**
	- Percona XtraDB - Percona XtraDB Cluster by Percona
	- MariaDB - MariaDB Galera Cluster by MariaDB
	- Codership - MySQL Galera Cluster by Codership

* **Version**
	- Select the MySQL version. For Codership and Percona, 5.5 and 5.6 are available. If you choose MariaDB, 5.5 and 10.1 will be available.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.

* **Server Port**
	- MySQL port for all nodes. Default is 3306.

* **My.cnf Template**
	- MySQL configuration template file under ``/usr/share/cmon/templates``. Default is my.cnf.galera. Keep it default is recommended.
	
* **Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Deploy**
	- Starts the Galera Cluster deployment.

MySQL/NDB Cluster
``````````````````

Deploys a new MySQL (NDB) Cluster by Oracle. The cluster consists of management nodes, MySQL API nodes and data nodes. The database cluster will be automatically added into ClusterControl once deployed. Minimum of 4 nodes (2 API/mgmd + 2 data nodes) is recommended.

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

* **Uninstall Existing MySQL Packages**
	- ClusterControl expects the target hosts use clean and minimal OS. Existing MySQL dependencies will be removed.
    
* **Management Server 1**
	- Specify the IP address or hostname of the first management server.

* **Management Server 2**
	- Specify the IP address or hostname of the second management server.

* **Port**
	- MySQL Cluster management port. Default to 1186.

* **Datadir**
	- MySQL Cluster data directory for NDB. Default is ``/var/lib/mysql-cluster``.

* **Data Node server [n]**
	- Specify the IP address or hostname of the MySQL Cluster data node.

* **Add two more Data Nodes**
	- It's recommended to have data nodes in pair. If you would like to use more than two data nodes, click this button to add two more input fields.

* **Remove last two Data Nodes**
	- Removes the last two input fields for data nodes.

* **Port**
	- MySQL Cluster data node port. Default to 2200.
    
* **Datadir**
	- MySQL Cluster data directory for NDB. Default is ``/var/lib/mysql-cluster``.

* **MySQL server [n]**
	- Specify the IP address or hostname of the MySQL Cluster SQL/API node.

* **Add one more SQL Nodes**
	- If you would like to use more than two SQL nodes, click this button to add one more input field.

* **Remove last SQL Nodes**
	- Removes the last input field for SQL node.

* **Server Port**
	- MySQL server port. Default to 3306.

* **my.cnf Template**
	- MySQL configuration template file under ``/usr/share/cmon/templates``. The default is my.cnf.mysqlcluster. Keep it default is recommended.

* **Server Data Directory**
	- MySQL SQL/API node data directory. Default is ``/var/lib/mysql``.

* **Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.

* **Deploy**
	- Starts the MySQL Cluster deployment.


Create Database Node
--------------------

This page provides ability to create a new single node of following databases in your environment:

* MySQL Replication Master
* MySQL Galera
* MongoDB ReplicaSet Node
* PostgreSQL

Once a single node is deployed, it can then be managed from the ClusterControl interface. Single nodes can be scaled into clusters with a single click of a button. You can scale MySQL replication with read-copy slaves, Percona XtraDB and MariaDB Galera are turned into Galera Clusters, MongoDB into a replica set and PostgreSQL into a master-slave replication at later stage via `Add Node <user-guide/mysql/overview.html#add-node>`_.

MySQL Replication Master
`````````````````````````

Deploy entire master-slave MySQL replication setup from ClusterControl. One would start by creating a master under this tab.

================================== ===========
Field                              Description
================================== ===========
Vendor                             Supported vendor is Percona Server and Oracle
Version                            Choose the MySQL version that you want to install. 5.6.x is recommended with GTID support
Template                           MySQL configuration template under ``/usr/share/cmon/templates``. Leave it blank if you want ClusterControl to generate the configuration file automatically.
Hostname                           The IP address or hostname of the target node. Ensure you can perform passwordless SSH to the node using the specified SSH User, SSH Port Number and SSH Key Path
Port                               MySQL port
Data Directory                     Location of MySQL data directory
Password                           MySQL root password
SSH User                           SSH user that ClusterControl will use to remotely access the target node
SSH Key Path                       Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.
SSH Port Number                    SSH port of target node
Need Sudo Password                 Click on the link and specify the sudo password for the SSH user if applicable
Disable Firewall                   Yes - Firewall will be disabled, No - Firewall will not be disabled
Disable AppArmor/SElinux           Check to disable AppArmor (Ubuntu) or SElinux (Redhat or CentOS)
Uninstall Existing MySQL Packages  All existing MySQL related package will be removed before ClusterControl performs the new installation on the target node
Enable Semi-sync Replication       Check the box to let ClusterControl configure this node with semi-sync replication plugin
Repository                         Default Repository - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository. Internal Repostory - Provision software by using the pre-existing software repository already setup on the nodes. User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections. Local Mirrored Repository - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
Deploy                             Start the database deployment
================================== ===========

Then, you can add a replication slave to the setup via `Add Node <user-guide/mysql/overview.html#add-node>`_. 

MySQL Galera
`````````````

Deploy a single Galera node. You can then use `Add Node`_ to scale out the cluster into three or more nodes. You can also use `Create Database Cluster`_ to deploy all nodes at once.

================================= ===========
Field                             Description
================================= ===========
Vendor                            Supported vendors are Codership, Percona XtraDB and MariaDB Galera
Version                           Choose the MySQL version that you want to install
Data Center                       Segment ID. Database node that have the same number are considered in the same “data center”.
my.cnf Template                   MySQL configuration template under ``/usr/share/cmon/templates``
Hostname                          The IP address or hostname of the target node. Ensure you can perform passwordless SSH to the node using the specified SSH User, SSH Port Number and SSH Key Path
Port                              MySQL port of target node
Data Directory                    Location of MySQL data directory
Password                          MySQL root password
SSH User                          SSH user that ClusterControl will use to remotely access the target node
SSH Key Path                      Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.
SSH Port Number                   SSH port of target node
Need Sudo Password                Click on the link and specify the sudo password for the SSH user if applicable
Disable Firewall                  Yes - Firewall will be disabled, No - Firewall will not be disabled
Disable AppArmor/SElinux          Check to disable AppArmor (Ubuntu) or SElinux (Redhat or CentOS)
Uninstall Existing MySQL Packages All existing MySQL related package will be removed before ClusterControl performs the new installation on the target node
Repository                        Default Repository - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository. Internal Repostory - Provision software by using the pre-existing software repository already setup on the nodes. User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections. Local Mirrored Repository - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
Deploy                            Start the database deployment
================================= ===========

Then, you can add another Galera node to the setup via `Add Node <user-guide/mysql/overview.html#add-node>`_.

MongoDB ReplicaSet
```````````````````

Deploy a MongoDB replica set setup from ClusterControl. One would start by creating a MongoDB node (with replSet configured) under this tab. Starting from ClusterControl v1.3, MongoDB 3.x is supported.

=================================== ===========
Field                               Description
=================================== ===========
Vendor                              Supported vendors are Percona (Percona Server for MongoDB) and MongoDB (MongoDB Server)
Hostname                            The IP address or hostname of the target node. Ensure you can perform passwordless SSH to the need using the specified SSH User, SSH Port Number and SSH Key Path.
Port                                MongoDB port.
User                                MongoDB user.
Password                            MongoDB admin password.
RS Name                             The replica set name.
Data Directory                      Location of MongoDB data directory.
mongodb.conf Template               MongoDB configuration template under ``/usr/share/cmon/templates``. Default is mongodb.conf.org.
Need Sudo Password                  Click on the link and specify the sudo password for the SSH user if applicable.
SSH User                            SSH user that ClusterControl will use to remotely access the target node.
SSH Key Path                        Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.
SSH Port Number                     SSH port of target node.
Disable Firewall                    Yes - Firewall will be disabled, No - Firewall will not be disabled.
Disable AppArmor/SElinux            Check to disable AppArmor (Ubuntu) or SElinux (Redhat or CentOS).
Uninstall Existing MongoDB Packages All existing MongoDB related package will be removed before ClusterControl performs the new installation on the target node.
Repository                          Default Repository - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository. Internal Repostory - Provision software by using the pre-existing software repository already setup on the nodes. User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections. Local Mirrored Repository - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
Deploy                              Start the database deployment.
=================================== ===========

Then, you can add a MongoDB replica node to the setup via `Add Node <user-guide/mysql/overview.html#add-node>`_.

PostgreSQL
```````````

Deploy a new PostgreSQL standalone or replication cluster from ClusterControl. One would start by creating a PostgreSQL master node under this tab. Only PostgreSQL 9.x is supported in this version.

====================================== ===========
Field                                  Description
====================================== ===========
Hostname                               The IP address or hostname of the target node. Ensure you can perform passwordless SSH to the need using the specified SSH User, SSH Port Number and SSH Key Path.
User                                   PostgreSQL user. ClusterControl will create this user automatically.
Password                               PostgreSQL admin password.
SSH User                               SSH user that ClusterControl will use to remotely access the target node.
SSH Key Path                           Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.
Need Sudo Password                     Click on the link and specify the sudo password for the SSH user if applicable.
Uninstall Existing PostgreSQL Packages All existing PostgreSQL related package will be removed before ClusterControl performs the new installation on the target node.
Repository                             Default Repository - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository. Internal Repostory - Provision software by using the pre-existing software repository already setup on the nodes. User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections. Local Mirrored Repository - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
Deploy                                 Start the database deployment.
====================================== ===========

Then, you can add a replication slave to the setup via `Add Node <user-guide/mysql/overview.html#add-node>`_.

Database Cluster List
---------------------

Each row represents the summarized status of a database cluster:

+----------------------+---------------------------------------------------------------------------------------------------------------------+
| Field                | Description                                                                                                         |
+======================+=====================================================================================================================+
| Cluster Name         | The cluster name, configured under *ClusterControl > Settings > General Settings > Cluster Settings > Cluster Name* |
+----------------------+---------------------------------------------------------------------------------------------------------------------+
| ID                   | The cluster identifier number                                                                                       |
+----------------------+---------------------------------------------------------------------------------------------------------------------+
| Version              | Database server major version                                                                                       |
+----------------------+---------------------------------------------------------------------------------------------------------------------+
| Database Vendor      | Database vendor                                                                                                     |
+----------------------+---------------------------------------------------------------------------------------------------------------------+
| Cluster Type         | The database cluster type:                                                                                          |
|                      |                                                                                                                     |
|                      | * MYSQL_SERVER - Standalone MySQL server                                                                            |
|                      | * REPLICATION - MySQL replication                                                                                   |
|                      | * GALERA - MySQL Galera Cluster, Percona XtraDB Cluster, MariaDB Galera Cluster                                     |
|                      | * MYSQL CLUSTER - MySQL Cluster                                                                                     |
|                      | * MONGODB - MongoDB/TokuMX replica Set, MongoDB/TokuMX Sharded Cluster, MongoDB/TokuMX Replicated Sharded Cluster   |
|                      | * POSTGRESQL - Standalone or Replicated PostgreSQL server                                                           |
+----------------------+---------------------------------------------------------------------------------------------------------------------+
| Cluster Status       | The cluster status:                                                                                                 |
|                      |                                                                                                                     |
|                      | * ACTIVE - The cluster is up and running. All cluster nodes are running normally.                                   |
|                      | * DEGRADED - The full set of nodes in a cluster is not available. One or more nodes is down or unreachable.         |
|                      | * FAILURE - The cluster is down. Probably that all or most of the nodes are down or unreachable, resulting the      |
|                      |   cluster fails to operate as expected.                                                                             |
+----------------------+---------------------------------------------------------------------------------------------------------------------+
| Auto Recovery        | The auto recovery status of Galera Cluster:                                                                         |
|                      |                                                                                                                     |
|                      | * Cluster - If sets to ON, ClusterControl will perform automatic recovery if it detects cluster failure.            |
|                      | * Node - If sets to ON, ClusterContorl will perform automatic recovery if it detects node failure.                  |
+----------------------+---------------------------------------------------------------------------------------------------------------------+
| Node Type and Status | See table on node status indicators further below.                                                                  |
+----------------------+---------------------------------------------------------------------------------------------------------------------+

Node status indicator:

==================== ============
Indicator            Description
==================== ============
Green (tick)         OK: Indicates the node is working fine.
Yellow (exclamation) WARNING: Indicates the node is degraded and not fully performing as expected.
Red (cross)          PROBLEMATIC: Indicates the node is down or unreachable.
==================== ============
