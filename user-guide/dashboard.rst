Dashboard
============

This page is the landing page once you logged in. It provides a summary of database clusters monitored under ClusterControl.

.. image:: img/cc_cluster_list_140.png
   :align: center

*Section 1*

ClusterControl's top menu.

* **Activity**
	- Provides global list of activities that have been performed across clusters (e.g., deploying a new cluster, adding an existing cluster and cloning). Pick a job to see its running messages or job details. It also shows aggregated view of all alarms raised across and ClusterControl logs (with severity) for all clusters monitored by ClusterControl.
	- Job status indicator:

+------------+--------------------------------------+
| Job status | Description                          |
+============+======================================+
| FINISHED   | The job is successfully executed.    |
+------------+--------------------------------------+
| FAILED     | The job is executed but failed.      |
+------------+--------------------------------------+
| RUNNING    | The job is started and in progress.  |
+------------+--------------------------------------+
| ABORTED    | The job is started but terminated.   |
+------------+--------------------------------------+
| DEFINED    | The job is defined and yet to start. |
+------------+--------------------------------------+
	
* **Deploy**
	- Opens the Deploy/Import Cluster popup. See `Deploy Database Cluster`_ section.

* **Import**
	- Opens the Deploy/Import Cluster popup. See `Import Existing Server/Database Cluster`_ section.

* **Settings**
	- ClusterControl global settings. See `Settings <admin_settings.html>`_ section.

* **Help**
	- Documentation - Opens ClusterControl online documentation page at http://www.severalnines.com/docs
	- Support Forum - Opens Severalnines community support forums at http://support.severalnines.com/forums . Community users are encouraged to use this support channel. For licensed user, please raise a support ticket at http://support.severalnines.com/tickets/new .
	- What's new? - Opens the What's new popup. This popup also appears the first time a user logs in after new installation or upgrade.

* **Logout**
	- Logs out from ClusterControl and return to login page.
	
*Section 2*

List of database clusters managed under ClusterControl with summarized status. Database cluster deployed by (or added into) ClusterControl will be listed in this page. See `Database Cluster List`_ section.


Deploy Database Cluster
------------------------

Opens a wizard to deploy a new set of database cluster. The following database cluster types are supported:

* MySQL Replication
* MySQL Galera
	* MySQL Galera Cluster
	* Percona XtraDB Cluster
	* MariaDB Galera Cluster
* MySQL Group Replication (beta)
* MySQL Cluster (NDB)
* MongoDB ReplicaSet
* MongoDB Shards

There are some prerequisites that need to be fulfilled prior to the deployment:

* Verify that sudo is working properly if you are using a non-root user.
* Passwordless SSH is configured correctly from ClusterControl node to all database nodes.

For more details, refer to the `Requirement <../../requirements.html>`_ section. Each time you create a database cluster, ClusterControl will trigger a job under *ClusterControl > Activity > Jobs*. You can see the progress and status under this page.

MySQL Replication
`````````````````

Deploys a new MySQL Replication. The database cluster will be automatically added into ClusterControl once deployed. Minimum of two nodes is required. The first node in the list is the master. You can add more slaves later after the deployment completes. Starting from version 1.4.0, it's possible to setup a master-master replication from scratch under 'Define Topology' tab.

1) SSH Settings
'''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
* **Cluster Name**
	- Specify a name for the cluster.

* **Install Software**
    - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

2) Define MySQL Servers
'''''''''''''''''''''''
    
* **Vendor**
	- Percona XtraDB - Percona Server by Percona
	- MariaDB - MariaDB Server by MariaDB
	- Oracle - MySQL Server by Oracle

* **Version**
	- Select the MySQL version. For Oracle, only 5.7 is supported. For Percona, 5.6 and 5.7 are available. If you choose MariaDB, only 10.1 is supported.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.
	
* **Server Port**
	- MySQL server port. Default is 3306.

* **my.cnf Template**
	- MySQL configuration template file under ``/usr/share/cmon/templates``. Default is ``my.cnf.repl[version]``. Keep the default is recommended.
	
* **Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.

* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.

3) Define Topology
'''''''''''''''''''

* **Master A - IP/Hostname**
	- Specify the IP address of the MySQL master node.
	
* **Add slaves to master A**
	- Add a slave node connected to master A. Press enter to add more slave.

* **Add Second Master Node**
	- Open the add node wizard for secondary master.

* **Master B - IP/Hostname**
	- Only available if you click *Add Second Master Node*.
	- Specify the IP address of the other MySQL master node. ClusterControl will setup a master-master replication between these nodes. Master B will be read-only once deployed (secondary master), letting Master A to hold the write role (primary master) for the replication chain.
	
* **Add slaves to master B**
	- Only available if you click *Add Second Master Node*.
	- Add a slave node connected to master B. Press enter to add more slave.
	
* **Deploy**
	- Starts the MySQL Replication deployment.


MySQL Galera 
`````````````

Deploys a new MySQL Galera Cluster. The database cluster will be automatically added into ClusterControl once deployed. A minimal setup is comprised of one Galera node (no high availability, but this can later be scaled with more nodes). However, the recommendation is a minimum of three nodes for high availability. Garbd (an arbitrator) can be added later after the deployment completes if needed.

1) SSH Settings
'''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
* **Cluster Name**
	- Specify a name for the cluster.

* **Install Software**
    - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

2) Define MySQL Servers
'''''''''''''''''''''''
    
* **Vendor**
	- Percona XtraDB - Percona XtraDB Cluster by Percona
	- MariaDB - MariaDB Galera Cluster by MariaDB
	- Codership - MySQL Galera Cluster by Codership

* **Version**
	- Select the MySQL version. For Codership, 5.5 and 5.6 are available, while Percona supports 5.5, 5.6 and 5.7. If you choose MariaDB, 5.5 and 10.1 are available.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.

* **Server Port**
	- MySQL server port. Default is 3306.

* **my.cnf Template**
	- MySQL configuration template file under ``/usr/share/cmon/templates``. Default is my.cnf.galera. Keep it default is recommended.
	
* **Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Add Nodes**
	- Specify the IP address or hostname of the MySQL nodes. Minimum of three nodes is recommended.

* **Deploy**
	- Starts the Galera Cluster deployment.

MySQL Group Replication
````````````````````````

Deploys a new :term:`MySQL Group Replication` by Oracle. This is beta feature introduced in version 1.4.0. The database cluster will be added into ClusterControl automatically once deployed. A minimum of three nodes is required.

1) SSH Settings
'''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
* **Cluster Name**
	- Specify a name for the cluster.

* **Install Software**
    - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

2) Define MySQL Servers
'''''''''''''''''''''''
    
* **Vendor**
	- Oracle - MySQL Group Replication by Oracle.

* **Version**
	- Select the MySQL version. Group Replication is only available on MySQL 5.7.

* **Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.

* **Server Port**
	- MySQL server port. Default is 3306.

* **my.cnf Template**
	- MySQL configuration template file under ``/usr/share/cmon/templates``. Default is my.cnf.grouprepl. Keep it default is recommended.
	
* **Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the Galera Cluster in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Add Nodes**
	- Specify the IP address or hostname of the MySQL nodes. Minimum of three nodes is recommended.

* **Deploy**
	- Starts the MySQL Group Replication deployment.

MySQL/NDB Cluster
``````````````````

Deploys a new MySQL (NDB) Cluster by Oracle. The cluster consists of management nodes, MySQL API nodes and data nodes. The database cluster will be automatically added into ClusterControl once deployed. Minimum of 4 nodes (2 API/mgmd + 2 data nodes) is recommended.

1) SSH Settings
'''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.
	
* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
* **Cluster Name**
	- Specify a name for the cluster.

* **Install Software**
    - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

2) Define Management Servers
'''''''''''''''''''''''''''''
    
* **Server Port**
	- MySQL Cluster management port. Default to 1186.

* **Server Data Directory**
	- MySQL Cluster data directory for NDB. Default is ``/var/lib/mysql-cluster``.

* **Management Server 1**
	- Specify the IP address or hostname of the first management server.

* **Management Server 2**
	- Specify the IP address or hostname of the second management server.

3) Define Data Nodes
''''''''''''''''''''

* **Server Port**
	- MySQL Cluster data node port. Default to 2200.

* **Server Data Directory**
	- MySQL Cluster data directory for NDB. Default is ``/var/lib/mysql-cluster``.

* **Add Nodes**
	- Specify the IP address or hostname of the MySQL Cluster data node. It's recommended to have data nodes in pair. You can add up to 14 data nodes to your cluster.

4) Define MySQL Servers
'''''''''''''''''''''''

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

MongoDB ReplicaSet
``````````````````

Deploys a new MongoDB Replica Set. The database cluster will be automatically added into ClusterControl once deployed. Minimum of three nodes (including mongo arbiter) is recommended.

.. Warning:: It is possible to deploy only 2 MongoDB nodes (without arbiter) but it is highly not recommended. The caveat of this approach is no automatic failover. If the primary node goes down then manual failover is required to make the other server as primary. Automatic failover works fine with 3 nodes and more.

1) SSH Settings
'''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Cluster Name**
	- Specify a name for the cluster.
	
* **Install Software**
    - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

2) Define MongoDB Servers
''''''''''''''''''''''''''
    
* **Vendor**
	- Percona - Percona Server for MongoDB by Percona.
	- MongoDB - MongoDB Server by MongoDB Inc.

* **Version**
	- The supported version is 3.2.

* **Server Data Directory**
	- Location of MongoDB data directory. Default is ``/var/lib/mongodb``.

* **Admin User**
	- MongoDB admin user. ClusterControl will create this user and enable authentication.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **Server Port**
	- MongoDB server port. Default is 27017.

* **mongodb.conf Template**
	- MongoDB configuration template file under ``/usr/share/cmon/templates``. Default is mongodb.conf.[vendor]. Keep it default is recommended.
	
* **ReplicaSet Name**
	- Specify the name of the replica set, similar to ``replSet`` option in MongoDB.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the MongoDB in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Add Nodes**
	- Specify the IP address or hostname of the MongoDB nodes. Minimum of three nodes is required.

* **Deploy**
	- Starts the MongoDB ReplicaSet deployment.


MongoDB Shards
``````````````

Deploys a new MongoDB Sharded Cluster. The database cluster will be automatically added into ClusterControl once deployed. Minimum of three nodes (including mongo arbiter) is recommended.

.. Warning:: It is possible to deploy only 2 MongoDB nodes (without arbiter) but it is highly not recommended. The caveat of this approach is no automatic failover. If the primary node goes down then manual failover is required to make the other server as primary. Automatic failover works fine with 3 nodes and more.

1) SSH Settings
'''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.
	
* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Cluster Name**
	- Specify a name for the cluster.

* **Install Software**
    - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

* **Disable Firewall**
	- Check the box to disable firewall (recommended).

* **Disable AppArmor/SELinux**
	- Check the box to let ClusterControl disable AppArmor (Ubuntu) or SELinux (Redhat/CentOS) if enabled (recommended).

2) Configuration Servers and Routers
'''''''''''''''''''''''''''''''''''''
    
*Configuration Server*

* **Server Port**
	- MongoDB config server port. Default is 27019.

* **Add Configuration Servers**
	- Specify the IP address or hostname of the MongoDB config servers. Minimum of one node is required, recommended to use three nodes.

*Routers/Mongos*

* **Server Port**
	- MongoDB mongos server port. Default is 27018.

* **Add More Routers**
	- Specify the IP address or hostname of the MongoDB mongos.

3) Define Shards
'''''''''''''''''
    
* **Replica Set Name**
	- Specify a name for this replica set shard.

* **Server Port**
	- MongoDB shard server port. Default is 27017.

* **Add Node**
	- Specify the IP address or hostname of the MongoDB shard servers. Minimum of one node is required, recommended to use three nodes.
	
* **Advanced Options**
	- Click on this to open set of advanced options for this particular node in this shard:
		- Add slave delay - Specify the amount of delayed slave in miliseconds format.
		- Act as an arbiter - Toggle to 'Yes' if the node is arbiter node. Otherwise, choose 'No'.

* **Add Another Shard**
  - Create another shard. You can then specify the IP address or hostname of MongoDB server that falls under this shard.
	
4) Database Settings
''''''''''''''''''''

* **Vendor**
	- Percona - Percona Server for MongoDB by Percona
	- MongoDB - MongoDB Server by MongoDB Inc

* **Version**
	- The supported version is 3.2.

* **Server Data Directory**
	- Location of MongoDB data directory. Default is ``/var/lib/mongodb``.

* **Admin User**
	- MongoDB admin user. ClusterControl will create this user and enable authentication.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **Server Port**
	- MongoDB server port. Default is 27017.

* **mongodb.conf Template**
	- MongoDB configuration template file under ``/usr/share/cmon/templates``. Default is mongodb.conf.[vendor]. Keep it default is recommended.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the MongoDB in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.

* **Deploy**
	- Starts the MongoDB Sharded Cluster deployment.


Import Existing Server/Database Cluster
---------------------------------------

Opens a single-page wizard to import the existing database setup into ClusterControl. The following database cluster types are supported:

* MySQL Replication
* MySQL Galera
	* MySQL Galera Cluster
	* Percona XtraDB Cluster
	* MariaDB Galera Cluster
* MySQL Cluster (NDB)
* MongoDB ReplicaSet
* MongoDB Shards
* PostgreSQL (single-instance)

There are some prerequisites that need to be fulfilled prior to adding the existing setup. The existing database cluster/server must:

* Verify that sudo is working properly if you are using a non-root user
* Passwordless SSH from ClusterControl node to database nodes has been configured correctly
* The target server/cluster must not in degraded state. For example, if you have a three-node Galera cluster, all nodes must alive and in synced.

For more details, refer to the `Requirement <../../requirements.html>`_ section. Each time you add an existing cluster or server, ClusterControl will trigger a job under *ClusterControl > Settings > Cluster Jobs*. You can see the progress and status under this page. A window will also appear with messages showing the progress.

Add Existing MySQL Replication
```````````````````````````````

ClusterControl is able to manage/monitor an existing set of MySQL servers (standalone or replication). Individual hosts specified in the same list will be added to the same server group in the UI. ClusterControl assumes that you are using the same MySQL root password for all instances specified in the group and it will attempt to determine the server role as well (master, slave, multi or standalone).

Choose *MySQL Replication* as the database type. Fill in all required information.

1) SSH Settings
''''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

2) Define MySQL Servers
''''''''''''''''''''''''

* **Vendor**
	- Percona for Percona Server
	- MariaDB for MariaDB Server
	- Oracle for MySQL Server

* **MySQL Version**
	- Supported version:
		- Percona Server (5.5, 5.6, 5.7)
		- MariaDB Server (10.1)
		- MySQL Server (5.7)

* **Basedir**
	- MySQL base directory. Default is ``/usr``. ClusterControl assumes all MySQL nodes are using the same base directory.

* **Port**
	- MySQL port on the target server/cluster. Default to 3306. ClusterControl assumes MySQL is running on the same port on all nodes.

* **User**
	- MySQL user on the target server/cluster. This user must able to perform GRANT statement. Recommended to use MySQL 'root' user.
	
* **Root Password**
	- Password for *MySQL User*. ClusterControl assumes that you are using the same MySQL root password for all instances specified in the group.

* **Add Nodes**
	- Enter the MySQL single instances' IP address or hostname that you want to group under this cluster.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the MySQL instances, import configurations and start managing them. 

Import Existing MySQL Galera
``````````````````````````````

Choose *MySQL Galera Cluster* as the database type. Fill in all required information.

1) SSH Settings
''''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
2) Define MySQL Servers
''''''''''''''''''''''''

* **Vendor**
	- Percona XtraDB - Percona XtraDB Cluster by Percona
	- MariaDB - MariaDB Galera Cluster by MariaDB
	- Codership - MySQL Galera Cluster by Codership

* **MySQL Version**
	- Select MySQL version of the target cluster.

* **Basedir**
	- MySQL base directory. Default is ``/usr``. ClusterControl assumes MySQL is having the same base directory on all nodes.

* **Port**
	- MySQL port on the target server/cluster. Default to 3306. ClusterControl assumes MySQL is running on the same port on all nodes.

* **User**
	- MySQL user on the target server/cluster. This user must able to perform GRANT statement. Recommended to use MySQL 'root' user.
	
* **Password** 
	- Password for *MySQL User*. The password must be the same on all nodes that you want to add into ClusterControl.

* **Enable information_schema Queries**
	- Use information_schema to query MySQL statistics. This is not recommended for clusters with more than 2000 tables/databases.
	
* **Enable Node AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects any of the nodes in the cluster is down.
	
* **Enable Cluster AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects the cluster is down or degraded.

* **Hostname**
	- Please note that you only need to specify ONE Galera node and ClusterControl will discover the rest based on ``wsrep_cluster_address``.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the Galera node, discover the configuration for the rest of the nodes and start managing/monitoring the cluster.


Import Existing MySQL Cluster
``````````````````````````````

ClusterControl is able to manage and monitor an existing production deployed MySQL Cluster (NDB). Minimum of 2 management nodes and 2 data nodes is required. 

Choose *MySQL/NDB Cluster* as the database type. Fill in all required information.

1) SSH Settings
''''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

2) Define Management Server 
'''''''''''''''''''''''''''

* **Management server 1**
	- Specify the IP address or hostname of the first MySQL Cluster management node.

* **Management server 2**
	- Specify the IP address or hostname of the second MySQL Cluster management node.

* **Server Port**
	- MySQL Cluster management port. The default port is 1186.


3) Define Data Nodes
'''''''''''''''''''''

* **Port**
	- MySQL Cluster data node port. The default port is 2200.

* **Add Nodes**
	- Specify the IP address or hostname of the MySQL Cluster data node.

4) Define MySQL Servers
''''''''''''''''''''''''
	
* **Root Password** 
	- MySQL root password.
	
* **Server Port**
	- MySQL port. Default to 3306.

* **MySQL Installation Directory**
	- MySQL server installation path where ClusterControl can find the ``mysql`` binaries.

* **Enable information_schema Queries**
	- Use information_schema to query MySQL statistics. This is not recommended for clusters with more than 2000 tables or databases.
	
* **Enable Node AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects any of the nodes in the cluster is down.
	
* **Enable Cluster AutoRecovery**
	- ClusterControl will perform automatic recovery if it detects the cluster is down or degraded.

* **Add Nodes**
	- Specify the IP address or hostname of the MySQL Cluster API/SQL node.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the MySQL Cluster nodes, discover the configuration for the rest of the nodes and start managing/monitoring the cluster.

Import Existing MongoDB ReplicaSet
````````````````````````````````````

ClusterControl is able to manage and monitor an existing MongoDB/Percona Server for MongoDB 3.x replica set.

1) SSH Settings
''''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.
	
2) Define MongoDB Servers
'''''''''''''''''''''''''

* **Vendor**
	- Percona - Percona Server for MongoDB by Percona. (formerly Tokutek)
	- MongoDB - MongoDB Server by MongoDB Inc. (formerly 10gen)

* **Version**
	- The supported version is 3.2.

* **Server Port**
	- MongoDB server port. Default is 27017.

* **Admin User**
	- MongoDB admin user.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **Hostname**
	- Specify one IP address or hostname of the MongoDB replica set member. ClusterControl will automatically discover the rest.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the specified MongoDB node, discover the configuration for the rest of the nodes and start managing/monitoring the cluster.

MongoDB Shards
``````````````

ClusterControl is able to manage and monitor an existing MongoDB/Percona Server for MongoDB 3.x sharded cluster setup.

1) SSH Settings
'''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

2) Set Router/Mongos
''''''''''''''''''''
    
*Configuration Server*

* **Server Port**
	- MongoDB mongos server port. Default is 27018.

* **Add More Routers**
	- Specify the IP address or hostname of the MongoDB mongos.
	
3) Database Settings
'''''''''''''''''''''

* **Vendor**
	- Percona - Percona Server for MongoDB by Percona
	- MongoDB - MongoDB Server by MongoDB Inc

* **Version**
	- The supported version is 3.2.

* **Admin User**
	- MongoDB admin user.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the specified MongoDB node, discover the configuration for the rest of the nodes and start managing/monitoring the cluster.

Add existing PostgreSQL servers
````````````````````````````````

ClusterControl is able to manage/monitor an existing set of PostgreSQL 9.x servers (standalone). Individual hosts specified in the same list will be added to the same server group in the UI. ClusterControl assumes that you are using the same postgres password for all instances specified in the group.

Choose *Postgres Server* as the database type. Fill in all required information.

1) SSH Settings
''''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.

* **Sudo Password**
	- Specify the password if the SSH user that you specified under *SSH User* requires sudo password to run super-privileged commands. Ignore this if *SSH User* is root or have no sudo password.
	
* **SSH Port**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

2) Define PostgreSQL Server
'''''''''''''''''''''''''''

* **Server Port**
	- PostgreSQL port on the target server/cluster. Default to 5432. ClusterControl assumes PostgreSQL is running on the same port on all nodes.

* **User**
	- PostgreSQL user on the target server/cluster. Recommended to use PostgreSQL 'postgres' user.

* **Password**
	- Password for *Postgres User*. ClusterControl assumes that you are using the same postgres password for all instances specified in the group.

* **Basedir**
	- PostgreSQL base directory. Default is ``/usr``. ClusterControl assumes all PostgreSQL nodes are using the same base directory.

* **Add Nodes**
	- Specify all PostgreSQL single instances that you want to group under this cluster.

* **Import**
	- Click the button to start the import. ClusterControl will connect to the PostgreSQL instances, import configurations and start managing them. 


Deploy Database Node
--------------------

This page provides ability to create a new single node of following database in your environment:

* PostgreSQL

Once a single node is deployed, it can then be managed from the ClusterControl interface. Single node can be scaled into clusters with a single click of a button. You can scale PostgreSQL into a master-slave replication at later stage via `Add Node <user-guide/postgresql/overview.html#add-node>`_.

PostgreSQL
```````````

Deploys a new PostgreSQL standalone or replication cluster from ClusterControl. One would start by creating a PostgreSQL master node under this tab. Only PostgreSQL 9.x is supported in this version.

1) SSH Settings
'''''''''''''''

* **SSH User**
	- Specify root if you have root credentials.
	- If you use 'sudo' to execute system commands, specify the name that you wish to use here. The user must exists on all nodes. See `Operating System User <../../requirements.html#operating-system-user>`_.
	
* **SSH Key Path**
	- Specify the full path of SSH key (the key must exist in ClusterControl node) that will be used by *SSH User* to perform passwordless SSH. See `Passwordless SSH <../../requirements.html#passwordless-ssh>`_.
	
* **Sudo Password**
	- If you use sudo with password, specify it here. Ignore this if *SSH User* is root or sudoer does not need a sudo password.

* **SSH Port Number**
	- Specify the SSH port for target nodes. ClusterControl assumes SSH is running on the same port on all nodes.

* **Cluster Name**
	- Specify a name for the database.

* **Install Software**
    - Check the box if you use clean and minimal VMs. Existing MySQL dependencies will be removed. New packages will be installed and existing packages will be uninstalled when provisioning the node with required software.
    - If unchecked, existing packages will not be uninstalled, and nothing will be installed. This requires that the instances have already provisioned the necessary software.

2) Define PostgreSQL Servers
''''''''''''''''''''''''''''

* **Server Port**
	- PostgreSQL server port. Default is 5432.

* **User**
	- Specify the PostgreSQL root user for example, postgres.

* **Password**
	- Specify the PostgreSQL root password.
	
* **Repository**
	- Use Vendor Repositories - Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
	- Do Not Setup Vendor Repositories - Provision software by using repositories already setup on the nodes. The User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
	- Use Mirrored Repositories - Create and mirror the current database vendor's repository and then deploy using the local mirrored repository. This is a preferred option when you have to scale the PostgreSQL in the future, to ensure the newly provisioned node will always have the same version as the rest of the members.
	
* **Hostname**
	- Specify the IP address or hostname of the PostgreSQL node.

* **Create**
	- Starts the PostgreSQL deployment for single instance.

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
| Database Vendor      | Database vendor icon                                                                                                |
+----------------------+---------------------------------------------------------------------------------------------------------------------+
| Cluster Type         | The database cluster type:                                                                                          |
|                      |                                                                                                                     |
|                      | * MYSQL_SERVER - Standalone MySQL server                                                                            |
|                      | * REPLICATION - MySQL replication                                                                                   |
|                      | * GALERA - MySQL Galera Cluster, Percona XtraDB Cluster, MariaDB Galera Cluster                                     |
|                      | * GROUP REPLICATION - MySQL Group Replication                                                                       |
|                      | * MYSQL CLUSTER - MySQL Cluster                                                                                     |
|                      | * MONGODB - MongoDB replica Set, MongoDB Sharded Cluster, MongoDB Replicated Sharded Cluster                        |
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
Red (wrench)         MAINTENANCE: Indicates that maintenance mode is on for this node.
Dark red (cross)     PROBLEMATIC: Indicates the node is down or unreachable.
==================== ============
