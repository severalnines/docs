.. include:: <isotech.txt>

s9s process
-----------

View processes running on nodes.

**Usage**

.. code-block:: bash

	s9s process {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ list, -L             Lists the processes.
|minus|\ |minus|\ list-digests         Prints statement digests together with statistical data showing how long it took them to be executed. The printed list will not contain individual SQL statements but patterns that collect multiple statements of similar form merged into groups by the similarities.
|minus|\ |minus|\ list-queries         Lists the queries, internal SQL processes of the cluster.
|minus|\ |minus|\ top-queries          Continuously showing the internal SQL processes in an interactive UI, similar ClusterControl UI *Top Queries* page.
|minus|\ |minus|\ top                  Continuously showing the processes in an interactive UI like the well-known "top" utility. Please note that if the terminal program supports the UI can be controller with the mouse.
====================================== ===========

**Options**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ cluster-id=ID        The ID of the cluster to show.
|minus|\ |minus|\ client=PATTERN       Shows only the processes that originate from clients that match the given pattern.
|minus|\ |minus|\ limit=N              Limits the number of processes shown in the list.
|minus|\ |minus|\ server=PATTERN       Shows only the processes that are executed by servers that match the given pattern.
|minus|\ |minus|\ sort-by-memory       Sorts the processes by resident memory size instead of cpu usage.
|minus|\ |minus|\ sort-by-time         Sorts the SQL queries by their runtime. The longer running queries are going to be on top.
|minus|\ |minus|\ update-freq=INTEGER  Update frequency for screen refresh in seconds.
====================================== ===========

**Examples**

Continuously print aggregated view of processes (similar to ``top`` output) of all nodes for cluster ID 1:

.. code-block:: bash

	$ s9s process --top --cluster-id=1

List aggregated view of processes (similar to ``ps`` output) of all nodes for cluster ID 1:

.. code-block:: bash

	$ s9s process --list --cluster-id=1

Print out aggregated digested SQL statements on all MySQL nodes in cluster ID 1:

.. code-block:: bash

	$ s9s process \
		--list-digests \
		--cluster-id=1 \
		--human-readable \
		--limit=10 \
		'*:3306'

Print aggregated list of database top queries which contains string "INSERT" on all nodes in cluster ID 1 with refresh rate every 1 second:

.. code-block:: bash

	$ s9s process \
		--top-queries \
		--cluster-id=1 \
		--update-freq=1 \
		'INSERT*'

Print all database queries on cluster ID 1 that are coming from a client with IP address 192.168.0.127 which having only "INSERT" string:

.. code-block:: bash

	$ s9s process \
		--list-queries \
		--cluster-id=1 \
		--client='192.168.0.127:*' \
		'INSERT*'

Print all database queries on cluster ID 1 that are reaching the database server 192.168.0.81:

.. code-block:: bash

	$ s9s process \
		--list-queries \
		--cluster-id=1 \
		--server='192.168.0.81:*'
