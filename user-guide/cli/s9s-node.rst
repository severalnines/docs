.. include:: <isotech.txt>

s9s node
--------

View and handle nodes.

**Usage**

.. code-block:: bash

	s9s node {command} {options}

**Command**

========================================== ===================
Name, shorthand                            Description
========================================== ===================
|minus|\ |minus|\ change-config            Changes configuration values for the given node.
|minus|\ |minus|\ enable-binary-logging    Creates a job to enable the binary logging on a specific node. Not all clusters support this feature (MySQL does). One need to enable binary logging in order to set up cluster to cluster replication.
|minus|\ |minus|\ list, -L                 List nodes. Default to all clusters.
|minus|\ |minus|\ list-config              Lists the configuration values for the given node.
|minus|\ |minus|\ pull-config              Copy the configuration file(s) from the node to the local computer. Use the ``--output-dir`` to control where the files will be created.
|minus|\ |minus|\ restart                  Restarts the node. This means the process that provides the main functionality on the node (e.g. the MySQL daemon on a Galera node) will be stopped then start again.
|minus|\ |minus|\ set                      Sets various properties of the specified node/host.
|minus|\ |minus|\ set-config               Changes configuration values for the given node.
|minus|\ |minus|\ set-read-only            Creates a job that sets the node to read-only mode. Please note that not all cluster types support read-only mode.
|minus|\ |minus|\ set-read-write           Creates a job that sets the node to read-write mode if it was previously set to read-only mode. Please note that not all cluster types support read-only mode.
|minus|\ |minus|\ start                    Starts the node. This means the process that provides the main functionality on the node (e.g. the MySQL daemon on a Galera node) will be start.
|minus|\ |minus|\ stat                     Prints detailed node information. It can be used in conjunction with ``--graph`` to produce statistical data. See `Graph Options`_.
|minus|\ |minus|\ stop                     Stops the node. This means the process that provides the main functionality on the node (e.g. the MySQL daemon on a Galera node) will be stopped.
|minus|\ |minus|\ unregister               Unregisters the node from ClusterControl.
========================================== ===================

**Options**

============================================== ===========
Name, shorthand                                Description
============================================== ===========
|minus|\ |minus|\ cluster-id=ID, -i            The ID of the cluster in which the node is.
|minus|\ |minus|\ cluster-name=NAME, -n        Name of the cluster to list.
|minus|\ |minus|\ nodes=NODE_LIST              The nodes to list or manipulate. See `Node List`_.
|minus|\ |minus|\ node-format=FORMAT           The format string used to print nodes.
|minus|\ |minus|\ opt-group=GROUP              The configuration option group.
|minus|\ |minus|\ opt-name=NAME                The name of the configuration option.
|minus|\ |minus|\ opt-value=VALUE              The value of the configuration option.
|minus|\ |minus|\ output-dir=DIR               The directory where the files are created.
|minus|\ |minus|\ graph=NAME                   The name of the graph to show. See `Graph Options`_.
|minus|\ |minus|\ begin=TIMESTAMP              The start of the graph interval.
|minus|\ |minus|\ end=TIMESTAMP                The end of the graph interval.
|minus|\ |minus|\ force                        Force the execution of potentially dangerous operations like restarting a master node in a MySQL Replication cluster.
|minus|\ |minus|\ begin=TIMESTAMP              The start time of the graph (the X axis).
|minus|\ |minus|\ density                      If this option is provided will be a probability density function (or histogram) instead of a timeline. The X axis shows the measured values (e.g. MByte/s) while the Y axis hows how many percent of the measurements contain the value. If for example the CPU usage is between 0% and 1% at the 90% of the time the graph will show a 90% bump at the lower end.
|minus|\ |minus|\ end=TIMESTAMP                The end of the graph.
|minus|\ |minus|\ node-format[=FORMATSTRING]   The string that controls the format of the printed information about the nodes. See `Node Format`_.
|minus|\ |minus|\ properties=ASSIGNMENT        One or more assignments specifying property names and values. The assignment operator is the ``=`` character (e.g. ``--properties='alias="newname"'``), multiple assignments are separated by the semicolon ``;``.
|minus|\ |minus|\ output-dir=DIRECTORY         The directory where the output files will be created on the local computer.
============================================== ===========

Graph Options
+++++++++++++

When providing a valid graph name together with the ``--stat`` option a graph will be printed with statistical data. Currently the following graphs are available:

==================== ===========
Option               Description
==================== ===========
cpughz               The graph will show the CPU clock frequency measured in GHz.
cpuload              Shows the average CPU load of the host computer.
cpusys               Percent of time the CPU spent in kernel mode.
cpuidle              Percent of time the CPU is idle on the host.
cpuiowait            Percent of time the CPU is waiting for IO operations.
cputemp              The temperature of the CPU measured in degree Celsius. Please note that to measure the CPU temperature some kernel module might be needed (e.g. it might be necessary to run ``sudo modprobe coretemp``). On multiprocessor systems the graph might show only the first processor.
cpuuser              Percent of time the CPU is running user space programs.
diskfree             The amount of free disk space measured in GBytes.
diskreadspeed        Disk read speed measured in MBytes/sec.
diskreadwritespeed   Disk read and write speed measured in MBytes/sec.
diskwritespeed       Disk write speed measured in MBytes/sec.
diskutilization      The bandwidth utilization for the device in percent.
memfree              The amount of the free memory measure in GBytes.
memutil              The memory utilization of the host measured in percent.
neterrors            The number of receive and transmit errors on the network interface.
netreceivedspeed     Network read speed in MByte/sec.
netreceiveerrors     The number of packets received with error on the given network interface.
nettransmiterrors    The number of packets failed to transmit.
netsentspeed         Network write speed in MByte/sec.
netspeed             Network read and write speed in MByte/sec.
sqlcommands          Shows the number of SQL commands executed measured in 1/s.
sqlcommits           The number of commits measured in 1/s.
sqlconnections       Shows the number of SQL connections.
sqlopentables        The number of open tables in any given moment.
sqlqueries           The number of SQL queries in 1/s.
sqlreplicationlag    Replication lag on the SQL server.
sqlslowqueries       The number of slow queries in 1/s.
swapfree             The size of the free swap space measured in GBytes.
==================== ===========

Node Format
+++++++++++

The string that controls the format of the printed information about the nodes. When this command line option is used the specified information will be printed instead of the default columns. The format string uses the ``%`` character to mark variable fields and flag characters as they are specified in the standard printf() C library functions. The ``%`` specifiers are ended by field name letters to refer to various properties of the nodes.

The ``%+12i`` format string for example has the ``+12`` flag characters in it with the standard meaning the field will be 12 character-wide and the ``+`` or ``-`` sign will always be printed with the number. The properties of the node are encoded by letters. In the ``%16D`` for example, the letter ``D`` encodes the "data directory" field, so the full path of the data directory on the node will be substituted. Standard ``\`` notation is also available, ``\n`` for example encodes a new-line character.

The s9s-tools support the following fields:

============= ===========
Field         Description
============= ===========
A             The IP address of the node.
a             Maintenance mode flag. If the node is in maintenance mode a letter ``M``, otherwise ``-``.
C             The configuration file for the most important process on the node (e.g. the configuration file of the MySQL daemon on a Galera node).
c             The total number of CPU cores in the host. Please note that this number may be affected by hyper-threading. When a computer has 2 identical CPUs, with four cores each and uses 2x hyper-threading it will count as 2x4x2 = 16.
D             The data directory of the node. This is usually the data directory of the SQL server.
d             The PID file on the node.
g             The log file on the node.
I             The numerical ID of the node.
i             The total number of monitored disk devices (partitions) in the cluster.
k             The total number of disk bytes found on the monitored devices in the node. This is a double precision floating point number measured in Terabytes.
L             The replay location. This field currently only has valid value in PostgreSQL clusters.
l             The received location. This field currently only has valid value in PostgreSQL clusters.
M             A message, describing the node's status un human readable format.
m             The total memory size found in the host, measured in GBytes. This value is represented by a double precision floating pointer number, so formatting it  with precision (e.g. ``%6.2m``) is possible. When used with the ``f`` modifier (e.g. ``%6.2fm``) this reports the free memory, the memory that available for allocation, used for cache or used for buffers.
N             The name of the node. If the node has an alias that is used, otherwise the name of the node is used. If the node is registered using the IP address, the IP address is the name.
n             The total number of monitored network interfaces in the host.
P             The port on which the most important service is awaiting for requests.
p             The PID (process ID) on the node that presents the service (e.g. the PID of the MySQL daemon on a Galera node).
O             The user name of the owner of the cluster that holds the node.
o             The name and version of the operating system together with the codename.
R             The role of the node (e.g. "controller", "master", "slave" or "none").
r             The work read-only or read-write indicating if the server is in read only mode or not.
S             The status of the host (e.g. CmonHostUnknown, CmonHostOnline, CmonHostOffLine, CmonHostFailed, CmonHostRecovery, CmonHostShutDown).
s             The list of slaves of the given host in one string.
T             The type of the node, e.g. "controller", "galera", "postgres".
t             The total network traffic (both received and transmitted) measured in MBytes/seconds.
U             The number of physical CPUs on the host.
u             The CPU usage percent found on the host.
V             The version string of the most important software (e.g. the version of the PostgreSQL installed on a PostgreSQL node).
v             The ID of the container/VM in "CLOUD/ID" format. The ``-`` string if no container ID is set for the node.
w             The total swap space found in the host measured in GigaBytes. With the ``f`` modifier (e.g. ``%6.2fw``) this reports the free swap space in GigaBytes.
Z             The name of the CPU model. Should the host have multiple CPUs, this will return the model name of the first CPU.
%             The ``%`` character itself.
============= ===========

**Examples**

List all nodes:

.. code-block:: bash

	$ s9s node --list --long
	ST  VERSION                  CID CLUSTER        HOST       PORT COMMENT
	go- 10.1.22-MariaDB-1~xenial   1 MariaDB Galera 10.0.0.185 3306 Up and running
	co- 1.4.1.1856                 1 MariaDB Galera 10.0.0.205 9500 Up and running
	go- 10.1.22-MariaDB-1~xenial   1 MariaDB Galera 10.0.0.209 3306 Up and running
	go- 10.1.22-MariaDB-1~xenial   1 MariaDB Galera 10.0.0.82  3306 Up and running
	Total: 4
	
Print the configuration for a node:

.. code-block:: bash

	$ s9s node --list-config --nodes=10.0.0.3
	...
	mysqldump   max_allowed_packet                     512M
	mysqldump   user                                   backupuser
	mysqldump   password                               nWC6NSm7PnnF8zQ9
	xtrabackup  user                                   backupuser
	xtrabackup  password                               nWC6NSm7PnnF8zQ9
	MYSQLD_SAFE pid-file                               /var/lib/mysql/mysql.pid
	MYSQLD_SAFE basedir                                /usr/
	Total: 71


The following example shows how a node in a given cluster can be restarted. When this command executed a new job will be created to restart a node. The command line tool will stop and show the job messages until the job is finished:

.. code-block:: bash

	$ s9s node \
		--restart \
		--cluster-id=1 \
		--nodes=192.168.1.117 \
		--log

Change a configuration value for a PostgreSQL server:

.. code-block:: bash

	$ s9s node \
		--change-config \
		--nodes=192.168.1.115 \
		--opt-name=log_line_prefix \
		--opt-value='%m '

Push a configuration option inside my.cnf (max_connections=500) on node 10.0.0.3:

.. code-block:: bash

	$ s9s node \
		--change-config \
		--nodes=10.0.0.3 \
		--opt-group=mysqld \
		--opt-name=max_connections \
		--opt-value=500

Listing the Galera hosts. This can be done by filtering the list of nodes by their properties:

.. code-block:: bash

	$ s9s node \
		--list \
		--long \
		--properties="class_name=CmonGaleraHost"

Create a set of graphs, one for each node shown in the terminal about the load on the hosts. If the terminal is wide enough the graphs will be shown side by side for a compact view:

.. code-block:: bash

	$ s9s node \
		--stat \
		--cluster-id=1 \
		--begin="08:00" \
		--end="14:00" \
		--graph=load


Density function can also be printed to show what were the typical values for the given statistical data. The following example shows what was the typical values for the user mode CPU usage percent:

.. code-block:: bash

	$ s9s node \
		--stat \
		--cluster-id=2 \
		--begin=00:00 \
		--end=16:00 \
		--density \
		--graph=cpuuser

The following example shows how a custom list can be created to show some information about the CPU(s) in some specific hosts:

.. code-block:: bash

	$ s9s node \
	--list \
	--node-format="%N %U CPU %c Cores %6.2u%% %Z\n" 192.168.1.191 192.168.1.195
	192.168.1.191 2 CPU 16 Cores  22.54% Intel(R) Xeon(R) CPU L5520 @ 2.27GHz
	192.168.1.195 2 CPU 16 Cores  23.12% Intel(R) Xeon(R) CPU L5520 @ 2.27GHz

The following list shows some information about the memory, the total memory and the memory available for the applications to allocate (including cache and buffer with the free memory):

.. code-block:: bash

	$ s9s node \
		--list \
		--node-format="%4.2m GBytes %4.2fm GBytes %N\n"
		16.00 GBytes 15.53 GBytes 192.168.1.191
		47.16 GBytes 38.83 GBytes 192.168.1.127


Set a node to read-write mode if it was previously set to read-only mode:

.. code-block:: bash

	$ s9s node \
		--set-read-write \
		--cluster-id=1 \
		--nodes=192.168.0.78 \
		--log

Copy configuration file(s) from a PostgreSQL server 192.168.0.232 into the local host:

.. code-block:: bash

	$ s9s node \
		--pull-config \
		--nodes="192.168.0.232" \
		--output-dir="tmp"
