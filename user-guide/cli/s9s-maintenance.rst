.. include:: <isotech.txt>

s9s maintenance
---------------

View and manipulate maintenance periods.

**Usage**

.. code-block:: bash

	s9s maintenance {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ create               Creates a new maintenance period.
|minus|\ |minus|\ current              Prints the active maintenance for a cluster or for a host. Prints nothing if no maintenance period is active.
|minus|\ |minus|\ list, -L             Lists the registered maintenance period. See `Maintenance List`_.
|minus|\ |minus|\ delete               Deletes an existing maintenance period. The maintenance periods are identified by their UUID strings. The UUID strings by default shown in an abbreviated format. When the ``--full-uuid`` command line option is provided the full length UUID strings will be shown. Deleting a maintenance period is also possible by providing only the first few characters of the UUID when these first characters are unique and enough to identify the maintenance period.
|minus|\ |minus|\ next                 Prints information about the very next maintenance period for a cluster or for a host. Prints nothing if no maintenance is registered to be started in the future.
====================================== ===========

**Options**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ begin=DATETIME       A string representation of the date and time when the maintenance period will start.
|minus|\ |minus|\ cluster-id=ID        The cluster for cluster maintenance.
|minus|\ |minus|\ end=DATETIME         A string representation of the date and time when the maintenance period will end.
|minus|\ |minus|\ full-uuid            Print the full UUID.
|minus|\ |minus|\ nodes=NODELIST       The nodes for the node maintenances. See `Node List`_.
|minus|\ |minus|\ reason=STRING        The reason for the maintenance.
|minus|\ |minus|\ start=DATETIME       A string representation of the date and time when the maintenance period will start. This option is deprecated, please use the ``--begin`` option instead.
|minus|\ |minus|\ uuid=UUID            The UUID to identify the maintenance period.
====================================== ===========

Maintenance List
++++++++++++++++

Using the ``--list`` and ``--long`` command line options a detailed list of the registered maintenance periods can be printed:

.. code-block:: bash

	 $ s9s maintenance --list --long
	 ST UUID    OWNER  GROUP  START    END      HOST/CLUSTER  REASON
	 Ah a7e037a system admins 11:21:24 11:41:24 192.168.1.113 Rolling restart.
	 Total: 1

The list contains the following fields:

============= =============
Field         Description
============= =============
ST            The  short status information, where at the first character position ``A`` stands for 'active' and ``-`` stands for 'inactive'. At the second character position ``h`` stands for 'host related maintenance' and ``c`` stands for 'cluster related maintenance'.
UUID          The unique string that identifies the maintenance period. Normally only the first few characters of the UUID is shown, but if the ``--full-uuid`` command line option is provided the full length string will be printed.
OWNER         The name of the owner of the given maintenance period.
GROUP         The name of the group owner of the maintenance period.
START         The date and time when the maintenance period starts.
END           The date and time when the maintenance period expires.
HOST/CLUSTER  The name of the cluster or host under maintenance.
REASON        A short human readable description showing why the maintenance is required.
============= =============

**Examples**

Create a maintenance period for PostgreSQL node 10.35.112.21, starting on 05:44:55 AM for one day:

.. code-block:: bash

	$ s9s maintenance \
	  --create \
	  --nodes=10.35.112.21:5432 \
    --start=2018-05-19T05:44:55.000Z \
		--end=2018-05-20T05:44:55.000Z \
	  --reason='Upgrading RAM' \
	  --batch

create a new maintenance period for 192.168.1.121 where it starts tomorrow and finishes an hour later:

.. code-block:: bash

	$ s9s maintenance --create \
		--nodes=192.168.1.121 \
		--start="$(date -d 'now + 1 day' '+%Y-%m-%d %H:%M:%S')" \
		--end="$(date -d 'now + 1 day + 1 hour' '+%Y-%m-%d %H:%M:%S')" \
		--reason="Upgrading software."

List out all nodes that are under maintenance period:

.. code-block:: bash

	$ s9s maintenance --list --long
	ST UUID    OWNER GROUP START    END      HOST/CLUSTER REASON
	-h 70346c3 dba   admin 07:42:18 08:42:18 10.0.0.209   Upgrading RAM
	Total: 1

Delete a maintenance period for UUID 70346c3:

.. code-block:: bash

	$ s9s maintenance --delete --uuid=70346c3

Check if there is any ongoing maintenance period for cluster ID 1:

.. code-block:: bash

	$ s9s maintenance --current --cluster-id=1

Check the next maintenance period schedule for node 192.168.0.227 for cluster ID 1:

.. code-block:: bash

	$ s9s maintenance \
		--next \
		--cluster-id=1 \
		--nodes="192.168.0.227"
