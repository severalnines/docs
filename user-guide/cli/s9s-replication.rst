.. include:: <isotech.txt>

s9s replication
---------------

Manage database replication related functions. 

.. Note:: Only applicable for supported database clusters namely MySQL/MariaDB Replication and PostgreSQL Streaming Replication.

**Usage**

.. code-block:: bash

	s9s replication {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ failover             Takes over the role of master from a failed master.
|minus|\ |minus|\ list                 List the replication links.            
|minus|\ |minus|\ promote              Promotes a slave to become a master.
|minus|\ |minus|\ stage                Rebuilds or stages a replication slave.
|minus|\ |minus|\ start                Starts a replication slave previously stopped using the ``--stop`` option.
|minus|\ |minus|\ stop                 Make the slave stop replicating. This option will create a job that does not stop the server but stops the replication on it.
====================================== ===========

**Options**

=========================================== ===========
Name, shorthand                             Description
=========================================== ===========
|minus|\ |minus|\ link-format=FORMATSTRING  The format string that controls the format of the printed information about the replication links. See `Link Format`_.
|minus|\ |minus|\ master=NODE               The replication master.
|minus|\ |minus|\ replication-master=NODE   This is the same as the ``--master`` option.
|minus|\ |minus|\ slave=NODE                The replication slave.
|minus|\ |minus|\ replication-slave=NODE    This is the same as the ``--slave`` option.
=========================================== ===========

Link Format
+++++++++++

The format string controls the format of the printed information about the links. When this command line option is used the specified information will be printed instead of the default columns. The format string uses the ``%`` character to mark variable fields and flag characters as they are specified in the standard ``printf()`` C library functions. The ``%`` specifiers are ended by field name letters to refer to various properties of the replication.

The ``%+12p`` format string for example has the ``+12`` flag characters in it with the standard meaning: the field will be 12 character wide and the ``+`` or ``-`` sign will always be printed with the number. The properties of the links are encoded by letters. The in the ``%4p`` for example the letter ``p`` encodes the ``slave port`` field, so the port number of the slave node will be substituted. Standard ``\`` notation is also available, ``\n`` for example encodes a new-line character.

The following example prints out a customize format to show the replication link among nodes together with the position of master/slave in the replication log for PostgreSQL streaming replication, MySQL GTID and MariaDB GTID:

.. code-block:: bash

	$ s9s replication \
		--list \
		--long \
		--link-format="%16h %4p <- %H %2P %o %O\n"
    192.168.0.81 5432 <- 192.168.0.83 5432 0/71001BA8
    192.168.0.82 5432 <- 192.168.0.83 5432 0/71001BA8
    192.168.0.42 3306 <- 192.168.0.41 3306 dca7e205-90db-11ea-9143-5254008afee6:1-10 3157
    192.168.0.43 3306 <- 192.168.0.41 3306 dca7e205-90db-11ea-9143-5254008afee6:1-10 3157
    192.168.0.92 3306 <- 192.168.0.91 3306 0-54001-49 10285
    192.168.0.93 3306 <- 192.168.0.91 3306 0-54001-49 10285

The s9s-tools support the following fields:

====== ===========
Field  Description
====== ===========
c      The cluster ID of the cluster where the slave node can be found.
C      The master cluster ID property of the slave host. This shows in which cluster the master of the represented replication link can be found.
d      This format specifier denotes the "seconds behind the master" property of the slave.
h      The host name of the slave node in the link.
H      The host name of the master node in the link.
o      The position of the slave in the replication log.
O      The position of the master in the replication log.
p      The port number of the slave.
P      The port number of the master node.
s      A short string representing the link status, e.g. "Online" when everything is ok.
m      A slightly longer, human readable string representing the state of the link. This is actually the "slave_io_state" property of the slave node.
====== ===========


**Examples**

Print all database replication links for clusters that fall under replication category:

.. code-block:: bash

	$ s9s replication --list
	
	
Promote a slave 192.168.0.164 to become a new master for a database cluster named "MySQL Rep 5.7 - Production":

.. code-block:: bash

	$ s9s replication \
		--promote \
		--cluster-name="MySQL Rep 5.7 - Production" \
		--slave="192.168.0.164:3306" \
		--wait

Rebuild the replication slave of 192.168.0.83 from the master, 192.168.0.76 and tag the job as "stage" for cluster ID 1:

.. code-block:: bash

	$ s9s replication \
		--stage \
		--cluster-id=1 \
		--job-tags="stage" \
		--slave="192.168.0.83:3306" \
		--master="192.168.0.76:3306" \
		--wait

Stop the replication slave on node 192.168.0.80:

.. code-block:: bash

	$ s9s replication \
		--stop \
		--cluster-id=1 \
		--slave="192.168.0.80:3306" \
		--wait

Start the replication slave on node 192.168.0.80:

.. code-block:: bash

	$ s9s replication \
		--start \
		--cluster-id=1 \
		--slave="192.168.0.80:3306" \
		--wait

