.. _Deploy in the Cloud:

Deploy in the Cloud
-------------------

Opens a step-by-step modal dialog to deploy a new set of database cluster in the cloud. Supported cloud providers are:

* Amazon Web Service
* Google Cloud Platform
* Microsoft Azure

The following database cluster types are supported:

* MySQL Replication:
	* Percona Server 8.0
	* Oracle MySQL Server 8.0
	* MariaDB Server 10.3
* MySQL Galera:
	* Percona XtraDB Cluster 5.7
	* MariaDB 10.2
	* MariaDB 10.3
* MongoDB ReplicaSet:
	* Percona Server for MongoDB 3.6
	* MongoDB 3.6
	* MongoDB 4.0
* PostgreSQL 11 Streaming Replication
* TimeScaleDB 11 Streaming Replication

There are prerequisites that need to be fulfilled prior to the deployment:

* A working cloud credential profile on the supported cloud platform. See :ref:`Sidebar - Integrations - Cloud Providers`.
* The date and time for ClusterControl node must be synced with NTP server. See :ref:`Requirements - Timezone`.
* If the cloud instance is inside a private network, the network must support auto-assign public IP address. ClusterControl only connects to the created cloud instance via public network. 

Under the hood, the deployment process does the following:

1. Create cloud instances.
2. Configure security groups and networking.
3. Verify the SSH connectivity from ClusterControl to all created instances.
4. Deploy database on every instance.
5. Configure the clustering or replication links.
6. Register the deployment into ClusterControl.

ClusterControl will trigger a deployment job and the progress can be monitored under *ClusterControl > Activity > Jobs*. 

.. Attention:: This feature is still in beta. See `Known Limitations`_ for details.

Cluster Details
+++++++++++++++++

* **Select Cluster Type**
	- Choose a cluster.
	
* **Select Vendor and Version**
	- MySQL Replication Cluster - Percona Server 8.0, Oracle MySQL Server 8.0, MariaDB Server 10.3
	- MySQL Galera - Percona XtraDB Cluster 5.7, MariaDB 10.2, MariaDB 10.3
	- MongoDB Replica Set - MongoDB 3.4 and MongoDB 4.0 by MongoDB, Inc and Percona Server for MongoDB 3.4 by Percona (replica set only).
	- PostgreSQL Streaming Replication - PostgreSQL 11.0 (streaming replication only).
	- TimeScaleDB - TimeScaleDB 11.0 (streaming replication only).

Configure Cluster
+++++++++++++++++


MySQL Replication
``````````````````````

* **Select Number of Nodes**
	- How many nodes for the database cluster. You can start with one but two is recommended.

* **Cluster Name**
	- This value will be used as the instance name or tag. No space is allowed.

* **MySQL Server Port**
	- MySQL server port. Default is 3306.

* **MySQL Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.

* **my.cnf Template**
	- MySQL configuration template file under ``/etc/cmon/templates`` or ``/usr/share/cmon/templates``. See :ref:`MySQL - Manage - Configurations - Base Template Files` for details.

* **MySQL Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.

MySQL Galera Cluster
``````````````````````

* **Select Number of Nodes**
	- How many nodes for the database cluster. You can start with one but three (or bigger odd number) is recommended.

* **Cluster Name**
	- This value will be used as the instance name or tag. No space is allowed.

* **MySQL Server Port**
	- MySQL server port. Default is 3306.

* **MySQL Root Password**
	- Specify MySQL root password. ClusterControl will configure the same MySQL root password for all instances in the cluster.

* **my.cnf Template**
	- MySQL configuration template file under ``/etc/cmon/templates`` or ``/usr/share/cmon/templates``. See :ref:`MySQL - Manage - Configurations - Base Template Files` for details.

* **MySQL Server Data Directory**
	- Location of MySQL data directory. Default is ``/var/lib/mysql``.

MongoDB Replica Set
``````````````````````

* **Select Number of Nodes**
	- How many nodes for the database cluster. You can start with one but three (or bigger odd number) is recommended.

* **Cluster Name**
	- This value will be used as the instance name or tag. No space is allowed.

* **Admin User**
	- MongoDB admin user. ClusterControl will create this user and enable authentication.

* **Admin Password**
	- Password for MongoDB *Admin User*.

* **Server Data Directory**
	- Location of MongoDB data directory. Default is ``/var/lib/mongodb``.

* **Server Port**
	- MongoDB server port. Default is 27017.

* **mongodb.conf Template**
	- MongoDB configuration template file under ``/etc/cmon/templates`` or ``/usr/share/cmon/templates``. See :ref:`MongoDB - Manage - Configurations - Base Template Files` for details.
	
* **ReplicaSet Name**
	- Specify the name of the replica set, similar to ``replication.replSetName`` option in MongoDB.

PostgreSQL Streaming Replication
`````````````````````````````````

* **Select Number of Nodes**
	- How many nodes for the database cluster. You can start with one but two or more is recommended. 

.. Note:: The first virtual machine that comes up will be configured as a master.

* **Cluster Name**
	- This value will be used as the instance name or tag. No space is allowed.

* **User**
	- Specify the PostgreSQL super user for example, postgres.

* **Password**
	- Specify the password for *User*.

* **Server Port**
	- PostgreSQL server port. Default is 5432.

TimeScaleDB Streaming Replication
`````````````````````````````````

* **Select Number of Nodes**
	- How many nodes for the database cluster. You can start with one but two or more is recommended. 

.. Note:: The first virtual machine that comes up will be configured as a master.

* **Cluster Name**
	- This value will be used as the instance name or tag. No space is allowed.

* **User**
	- Specify the TimeScaleDB super user for example, postgres.

* **Password**
	- Specify the password for *User*.

* **Server Port**
	- TimeScaleDB server port. Default is 5432.

Select Credential
+++++++++++++++++

Select one of the existing cloud credentials or you can create a new one by clicking on the *Add New Credential* button.

* **Add New Credential**
	- Opens the cloud credential configuration wizard. See :ref:`Sidebar - Integrations - Cloud Providers`.

Select Virtual Machine
+++++++++++++++++++++++

Most of the settings in this step are dynamically populated from the cloud provider by the chosen credentials.

* **Operating System**
	- Choose a supported operating system from the dropdown.

* **Instance Size**
	- Choose an instance size for the cloud instance.

* **Virtual Private Cloud (VPC)**
	- Exclusive for AWS. Choose a virtual private cloud network for the cloud instance.

* **Add New**
	- Opens the *Add VPC* wizard. Specify the tag name and IP address block.

* **SSH Key**
	- SSH key location on the ClusterControl host. This key must be able to authenticate to the created cloud instances passwordlessly.

* **Storage Type**
	- Choose the storage type for the cloud instance.

* **Allocate Storage**
	- Specify the storage size for the cloud instance in GB.

Deployment Summary
++++++++++++++++++

* **Subnet**
	- Choose one existing subnet for the selected network.

* **Add New Subnet**
	- Opens the *Add Subnet* wizard. Specify the subnet name, availability zone and IP CIDR block address. E.g: 10.0.10.0/24

Known Limitations
++++++++++++++++++

There are known limitations for the cloud deployment feature:

* There is currently no 'accounting' in place for the cloud instances. You will need to manually remove created cloud instances.
* You cannot deploy a load balancer automatically with a cloud instance.

We appreciate your feedbacks, feature requests and bug reports. Contact us via the support channel or create a feature request. See :ref:`FAQ` for details.