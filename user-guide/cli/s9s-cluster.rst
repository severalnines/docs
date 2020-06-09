.. include:: <isotech.txt>

s9s cluster
-----------

Create, manage and manipulate clusters.

**Usage**

.. code-block:: bash

	s9s cluster {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ add-node             Adds a new node (server) to the cluster or to be more precise creates a new job that will eventually add a new node to the cluster. The name (or IP address) of the node should be specified using the ``--nodes`` command line option. See `Node List`_.
|minus|\ |minus|\ check-hosts          Checks the hosts before installing a cluster.
|minus|\ |minus|\ collect-logs         Creates a job that will collect the log files from the nodes of the cluster.
|minus|\ |minus|\ create               Creates a new cluster. When this command line option is provided the program will contact the controller and register a new job that will eventually create a new cluster.
|minus|\ |minus|\ create-account       Creates a new account to be used on the cluster to access the database(s).
|minus|\ |minus|\ create-database      Creates a database on the cluster.
|minus|\ |minus|\ create-report        When this command line option is provided a new job will be started that will create a report. After the job is executed the report will be  available  on  the controller. If the ``--output-dir`` command line option is provided the report will be created in the given directory on the controller host.
|minus|\ |minus|\ delete-account       Deletes an existing account from the cluster.
|minus|\ |minus|\ disable-recovery     Creates a new job that will disable the autorecovery for the cluster (both cluster autorecovery and node autorecovery). The job can optionally be used to also register a maintenance period for the cluster.
|minus|\ |minus|\ drop                 Drops cluster from the controller.
|minus|\ |minus|\ enable-recovery      Creates a job that will enable the autorecovery for both the cluster and the nodes in the cluster.
|minus|\ |minus|\ import-config        Creates a job that will import all the configuration files from the nodes of the cluster.
|minus|\ |minus|\ list, -L             Lists the clusters.
|minus|\ |minus|\ list-config          This command line option can be used to print the configuration values for the cluster. The cluster configuration in this context is the Cmon Controller's configuration for the given cluster.
|minus|\ |minus|\ list-databases       Lists the databases found on the cluster. Please note that if the cluster has a lot of databases, this option might not show some of them. Sampling a huge number of databases would generate high load and so the controller has an upper limit built into it.
|minus|\ |minus|\ ping                 Checks the connection to the controller.
|minus|\ |minus|\ promote-slave        Promotes a slave node to become a master. This main option will of course work only on clusters where it is meaningful, where there are slaves and masters are possible.
|minus|\ |minus|\ register             Registers an existing cluster in the controller. This option is very similar to the ``--create`` option, but it of course will not install a new cluster, it just registers one.
|minus|\ |minus|\ remove-node          Removes a node from the cluster (creates a new job that will remove the node from the cluster). The name (or IP address) of the node should be specified using  the  ``--nodes`` command line option. See `Node List`_.
|minus|\ |minus|\ rolling-restart      Restarts the nodes (one node at a time) without stopping the cluster.
|minus|\ |minus|\ set-read-only        Creates a job that when executed will set the entire cluster into read-only mode. Please note that not every cluster type supports the read-only mode.
|minus|\ |minus|\ start                Creates a new job to start the cluster.
|minus|\ |minus|\ stat                 Prints the details of one or more clusters.
|minus|\ |minus|\ stop                 Creates and registers and a new job that will stop the cluster when executed.
====================================== ===========

**Options**

=================================================== ===========
Name, shorthand                                     Description
=================================================== ===========
|minus|\ |minus|\ backup-id=NUMBER                  The id of a backup to be restored on the newly created cluster.
|minus|\ |minus|\ batch                             Print no messages. If the application created a job print only the job ID number and exit. If the command prints data do not use syntax highlight, headers, totals, only the pure table to be processed using filters.
|minus|\ |minus|\ cluster-format=FORMATSTRING       The string that controls the format of the printed information about clusters. See `Cluster Format`_.
|minus|\ |minus|\ cluster-id=ID, -i                 The ID of the cluster to manipulate.
|minus|\ |minus|\ cluster-name=NAME, -n             Sets the cluster name. If the operation creates a new cluster this will be the name of the new cluster.
|minus|\ |minus|\ cluster-type=TYPE                 The type of the cluster to install. Currently the following types are supported: ``galera``, ``mysqlreplication``, ``groupreplication`` (or ``group_replication``), ``ndb`` (or ``ndbcluster``), ``mongodb`` (MongoDB ReplicaSet only) and ``postgresql``.
|minus|\ |minus|\ config-template=FILENAME          Use the specified file as configuration template to create the configuration file for the new cluster.
|minus|\ |minus|\ datadir=DIRECTORY                 The directory on the node(s) that will hold the data. The primary use for this command line option is to set the data directory path when a cluster is created.
|minus|\ |minus|\ db-admin=USERNAME                 The user name of the database administrator (e.g. 'root').
|minus|\ |minus|\ db-admin-passwd=PASSWD            The password for the database admin.
|minus|\ |minus|\ donor=ADDRESS                     Currently this option is used when starting a cluster. It can be used to control which node will be started first and used for the others as donor.
|minus|\ |minus|\ job-tags=LIST                     Tags for the job if a job is created.
|minus|\ |minus|\ long, -l                          Print the detailed list.
|minus|\ |minus|\ no-header                         Do not print headers for tables.
|minus|\ |minus|\ no-install                        Skip the cluster software installation part. Assume all software is installed on the node(s). This command line option is considered when installing a new cluster or adding a new node to an existing cluster.
|minus|\ |minus|\ nodes=NODE_LIST                   List of nodes to work with. See `Node List`_.
|minus|\ |minus|\ os-user=USERNAME                  The name of the remote user that is used to gain SSH access on the remote nodes. If this command line option is omitted the name of the local user will be used on the remote host too.
|minus|\ |minus|\ os-key-file=PATH                  The path of the SSH key to install on a new container to allow the user to log in. This command line option can be passed when a new container is created, the argument of the option should be the path of the private key stored on the controller. Although the path of the private key file is passed only the public key will be uploaded to the new container.
|minus|\ |minus|\ output-dir=DIR                    The directory where the files are created. Use in conjunction with ``--create-report`` command.
|minus|\ |minus|\ provider-version=VER              The version string of the software to be installed.
|minus|\ |minus|\ remote-cluster-id=ID              The remote cluster ID for the cluster creation when cluster-to-cluster replication is to be installed. Please note that not all the cluster types support cluster to cluster replication.
|minus|\ |minus|\ use-internal-repos                Use internal repositories when installing software packages. Using this command line option it is possible to deploy clusters and add nodes off-line, without a working internet connection. The internal repositories has to be set up in advance.
|minus|\ |minus|\ vendor=VENDOR                     The name of the software vendor to be installed.
|minus|\ |minus|\ wait                              Waits for the specified job to end. While waiting, a progress bar will be shown unless the silent mode is set.
|minus|\ |minus|\ with-timescaledb                  Install the TimescaleDB option when creating a new cluster. This is currently only supported on PostgreSQL systems.
--------------------------------------------------- -----------
*ACCOUNT, DATABASE & CONFIGURATION MANAGEMENT*
---------------------------------------------------------------
|minus|\ |minus|\ account=NAME[:PASSWD][@HOST]      Account to be created on the cluster.
|minus|\ |minus|\ db-name=NAME                      The name of the database.
|minus|\ |minus|\ opt-group=NAME                    The option group for configuration.
|minus|\ |minus|\ opt-name=NAME                     The name of the configuration item.
|minus|\ |minus|\ opt-value=VALUE                   The value for the configuration item.
|minus|\ |minus|\ with-database                     Create a database for the user too.
--------------------------------------------------- -----------
*CONTAINER & CLOUD*
---------------------------------------------------------------
|minus|\ |minus|\ cloud=PROVIDER                    This option can be used when new container(s) created. The name of the cloud provider where the new container will be created. This command line option can also be used to filter the list of the containers when used together with one of the ``--list`` or ``--stat`` options.
|minus|\ |minus|\ containers=LIST                   A list of containers to be created and used by the created job. This command line option can be used to create container (virtual machines) and then install clusters on them or just add them to an existing cluster as nodes. Please check `s9s container`_ for details.
|minus|\ |minus|\ credential-id=ID                  The cloud credential ID that should be used when creating a new container. This is an optional value, if not provided the controller will find the credential to be used by the cloud name and the chosen region.
|minus|\ |minus|\ firewalls=LIST                    List of firewall (security groups) IDs separated by ``,`` or ``;`` to be used for newly created containers. Check ``s9s-container`` for further details.
|minus|\ |minus|\ generate-key                      Create a new SSH keypair when creating new containers. If this command line option was provided a new SSH keypair will be created and registered for a new user account to provide SSH access to the new container(s). If the command creates more than one containers the same one keypair will be registered for all. The username will be the username of the authenticated cmon-user. This can be overruled by the ``--os-user`` command line option. When the job creates a new cluster the generated keypair will be registered for the cluster and the file path will be saved into the cluster's Cmon configuration file. When adding a node to such a cluster this ``--generate-key`` option should not be passed, the controller will automatically re-use the previously created keypair.
|minus|\ |minus|\ image=NAME                        The name of the image from which the new container will be created. This option is not mandatory, when a new container is created the controller can choose an image if it is needed. To find out what images are supported by the registered container severs please issue the s9s server ``--list-images`` command.
|minus|\ |minus|\ image-os-user=NAME                The name of the initial OS user defined in the image for the first login. Use this option to create containers based on custom images.
|minus|\ |minus|\ os-password=PASSWORD              This command line option can be passed when creating new containers to set the password for the user that will be created on the container. Please note that some virtualization backend might not support passwords, only keys.
|minus|\ |minus|\ subnet-id=ID                      This option can be used when new containers are created to set the subnet ID for the container. To find out what subnets are supported by the registered container severs please issue the s9s server ``--list-subnets`` command.
|minus|\ |minus|\ template=NAME                     The name of the container template. See `Container Template`_.
|minus|\ |minus|\ volumes=LIST                      When a new container is created this command line option can be used to pass a list of volumes that will be created for the container. The list can contain one or more volumes separated by the ``;`` character. Every volume consists three properties separated by the ``:`` character, a volume name, the volume size in gigabytes and a volume type that is either "hdd" or "ssd".  The string ``vol1:5:hdd;vol2:10:hdd`` for example defines two hard-disk volumes, one 5GByte and one 10GByte. For convenience the volume name and the type can be omitted, so that automatically generated volume names are used.
|minus|\ |minus|\ vpc-id=ID                         This option can be used when new containers are created to set the VPC ID for the container. To find out what VPCs are supported by the registered container severs please issue the ``s9s server --list-subnets --long`` command.
--------------------------------------------------- -----------
*LOAD BALANCER*
---------------------------------------------------------------  
|minus|\ |minus|\ admin-password=PASSWORD           The password for the administrator of load balancers.
|minus|\ |minus|\ admin-user=USERNAME               The username for the administrator of load balancers.
|minus|\ |minus|\ dont-import-accounts              If this option is provided the database accounts will not be imported after the loadbalancer is installed and added to the cluster. The accounts can be imported later, but it is not going to be the part of the load balancer installation performed by the controller.
|minus|\ |minus|\ haproxy-config-template=FILENAME  Configuration template for the HAProxy installation.
|minus|\ |minus|\ monitor-password=PASSWORD         The password of the monitoring user of the load balancer.
|minus|\ |minus|\ monitor-user=USERNAME             The username of the monitoring user of the load balancer.
=================================================== ===========

Node List
+++++++++

The list of nodes or hosts enumerated in a special string using a semicolon as field separator (e.g. ``192.168.1.1;192.168.1.2``). The strings in the node list are URLs that can have the following protocols:

=================== ======================
URI                 Description
=================== ======================
``mysql://``        The protocol to install and handle MySQL servers.
``ndbd://``         The protocol for MySQL Cluster (NDB) data node servers.
``ndb_mgmd://``     The protocol for MySQL Cluster (NDB) management node servers. The ``mgmd://`` notation is also accepted.
``haproxy://``      Used to create and manipulate HaProxy servers.
``proxysql://``     Use this to install and handle ProxySql servers.
``maxscale://``     The protocol to install and handle MaxScale servers.
``mongos://``       The protocol to install and handle mongo router servers.
``mongocfg://``     The protocol to install and handle mongo config servers.
``mongodb://``      The protocol to install and handle mongo data servers.
``postgresql://``   The protocol to install and handle PostgreSQL servers.
=================== ======================

Cluster Format
++++++++++++++

The string that controls the format of the printed information about clusters. When this command line option is used, the specified information will be  printed instead of the default columns. The format string uses the ``%`` character to mark variable fields and flag characters as they are specified in the standard printf() C library functions. The ``%`` specifiers are ended by field name letters to refer to various properties of the clusters.

The ``%+12I`` format string for example has the ``+12`` flag characters in it with the standard meaning: the field will be 12 character wide and the ``+``  or ``-``  sign  will always be printed with the number. The properties of the message are encoded by letters. The in the ``%-5I`` for example the letter ``I`` encodes the "cluster ID" field, so the numerical ID of the cluster will be substituted. Standard ``\`` notation is also available, ``\n`` for example encodes a new-line character.

The s9s-tools support the following fields:

============= ===========
Field         Description
============= ===========
a             The number of active alarms on the cluster.
C             The configuration file for the cluster.
c             The total number of CPU cores in the cluster. Please note that this number may be affected by hyper-threading. When a computer has 2 identical CPUs, with four cores each and uses 2x hyper-threading it will count as 2x4x2 = 16.
D             The domain name of the controller of the cluster. This is the string one would get if executed the "domain name" command on the controller host.
G             The name of the group owner of the cluster.
H             The host name of the controller of the cluster. This is the string one would get if executed the "hostname" command on the controller host.
h             The number of the hosts in the cluster including the controller itself.
I             The numerical ID of the cluster.
i             The total number of monitored disk devices (partitions) in the cluster.
k             The  total  number of disk bytes found on the monitored devices in the cluster. This is a double precision floating point number measured in Terabytes. With the ``f`` modifier (e.g. ``%6.2fk``) this will report the free disk space in TeraBytes.
L             The log file of the cluster.
M             A human readable short message that discribes the state of the cluster.
m             The size of memory of all the hosts in the cluster added together, measured in GBytes. This value is represented by a double precision floating pointer number, so formatting it with precision (e.g. ``%6.2m``) is possible. When used with the ``f`` modifier (e.g. ``%6.2fm``) this reports the free memory, the memory that available for allocation, used for cache or used for buffers.
N             The name of the cluster.
n             The total number of monitored network interfaces in the cluster.
O             The name of the owner of the cluster.
S             The state of the cluster.
T             The type of the cluster.
t             The total network traffic (both received and transmitted) measured in MBytes/seconds found in the cluster.
V             The vendor and the version of the main software (e.g. the MySQL server) on the node.
U             The number of physical CPUs on the host.
u             The CPU usage percent found on the cluster.
w             The total swap space found in the cluster measured in GigaBytes. With the ``f`` modifier (e.g. ``%6.2fk``) this reports the free swap space in GigaBytes.
%             The ``%`` character itself.
============= ===========

**Examples**

Create a three-node Percona XtraDB Cluster 5.7 cluster, with OS user vagrant:

.. code-block:: bash

	$ s9s cluster --create \
		--cluster-type=galera \
		--nodes="10.10.10.10;10.10.10.11;10.10.10.12" \
		--vendor=percona \
		--provider-version=5.7 \
		--db-admin-passwd='pa$$word' \
		--os-user=vagrant \
		--os-key-file=/home/vagrant/.ssh/id_rsa \
		--cluster-name='Percona XtraDB Cluster 5.7'

Create a three-node MongoDB Replica Set 3.2 by MongoDB Inc (formerly 10gen) and use the default ``/root/.ssh/id_rsa`` as the SSH key, and let the deployment job running in foreground:

.. code-block:: bash

	$ s9s cluster --create \
		--cluster-type=mongodb \
		--nodes="10.0.0.148;10.0.0.189;10.0.0.219" \
		--vendor=10gen \
		--provider-version='3.2' \
		--os-user=root \
		--db-admin='admin' \
		--db-admin-passwd='MyS3cr3tPass' \
		--cluster-name='MongoDB ReplicaSet 3.2' \
		--wait


An example for creating a MongoDB Sharded Cluster with 3 mongos, 3 mongo config and one shard consists of a three-node replicaset called 'replset2':

.. code-block:: bash

	$ s9s cluster --create \
		--cluster-type=mongodb \
		--vendor=10gen \
		--provider-version=3.2 \
		--db-admin=adminuser \
		--db-admin-passwd=adminpwd \
		--os-user=root \
		--os-key-file=/root/.ssh/id_rsa \
		--nodes="mongos://192.168.1.11;mongos://192.168.1.12;mongos://192.168.1.12;mongocfg://192.168.1.11;mongocfg://192.168.1.12;mongocfg://192.168.1.13;192.168.1.14?priority=5.0;192.168.1.15?arbiter_only=true;192.168.1.16?priority=2;192.168.1.17?rs=replset2;192.168.1.18?rs=replset2&arbiter_only=yes;192.168.1.19?rs=replset2&slave_delay=3&priority=0"


Import and existing Percona XtraDB Cluster 5.7 and let the deployment job running in foreground (provided passwordless SSH from ClusterControl node to all database nodes have been setup correctly):

.. code-block:: bash

	$ s9s cluster --register \
		--cluster-type=galera \
		--nodes="192.168.100.34;192.168.100.35;192.168.100.36" \
		--vendor=percona \
		--provider-version=5.7 \
		--db-admin="root" \
		--db-admin-passwd="root123" \
		--os-user=root \
		--os-key-file=/root/.ssh/id_rsa \
		--cluster-name="My DB Cluster" \
		--wait


Create a MySQL 5.7 replication cluster by Oracle with multiple master and slaves (note the ``?`` sign to identify the node's role in the ``--nodes`` parameter):

.. code-block:: bash

	$ s9s cluster --create \
		--cluster-type=mysqlreplication \
		--nodes="192.168.1.117?master;192.168.1.113?slave;192.168.1.115?slave;192.168.1.116?master;192.168.1.118?slave;192.168.1.119?slave;" \
		--vendor=oracle \
		--db-admin="root" \
		--db-admin-passwd="root123" \
		--cluster-name=ft_replication_23986 \
		--provider-version=5.7 \
		--log

Create a PostgreSQL 12 streaming replication cluster with one master and two slaves (note the ``?`` sign to identify the node's role in the ``--nodes`` parameter):

.. code-block:: bash

	$ s9s cluster --create \
		--cluster-type=postgresql \
		--nodes="192.168.1.81?master;192.168.1.82?slave;192.168.1.83?slave;" \
		--db-admin="postgres" \
		--db-admin-passwd="mySuperStongP455w0rd" \
		--cluster-name=ft_replication_23986 \
		--os-user=vagrant \
		--os-key-file=/home/vagrant/.ssh/id_rsa \
		--provider-version=12 \
		--log

List all clusters with more details:

.. code-block:: bash

	$ s9s cluster --list --long

Delete a cluster with cluster ID 1:

.. code-block:: bash

	$ s9s cluster --delete --cluster-id=1
	
Add a new database node on Cluster ID 1:

.. code-block:: bash

	$ s9s cluster --add-node \
		--nodes=10.10.10.14 \
		--cluster-id=1 \
		--wait

Add a data node to an existing MongoDB Sharded Cluster with cluster ID 12 having replicaset name 'replset2':

.. code-block:: bash

	$ s9s cluster --add-node \
		--cluster-id=12 \
		--nodes="mongodb://192.168.1.20?rs=replset2"

Create an HAProxy load balancer, 192.168.55.198 on cluster ID 1:

.. code-block:: bash

	$ s9s cluster --add-node \
		--cluster-id=1 \
		--nodes="haproxy://192.168.55.198" \
		--wait

Remove a database node from cluster ID 1 as a background job:

.. code-block:: bash

	$ s9s cluster --remove-node \
		--nodes=10.10.10.13 \
		--cluster-id=1

Check if the hosts are part of other cluster and accessible from ClusterControl:

.. code-block:: bash

	$ s9s cluster --check-hosts \
		--nodes="10.0.0.148;10.0.0.189;10.0.0.219"

Schedule a rolling restart of the cluster 20 minutes from now:

.. code-block:: bash

	$ s9s cluster --rolling-restart \
		--cluster-id=1 \
		--schedule="$(date -d 'now + 20 min')"

Create a database on the cluster with the given name:

.. code-block:: bash

	$ s9s cluster --create-database \
		--cluster-id=2 \
		--db-name=my_shopping_db

Create a database account on the cluster and also create a new database to be used by the new user. Grant all access on the new database for the new user:

.. code-block:: bash

	$ s9s cluster --create-account \
		--cluster-id=1 \
		--account="john:passwd@10.10.1.100" \
		--with-database

