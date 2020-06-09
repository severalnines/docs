.. include:: <isotech.txt>

s9s controller
--------------

View and handle controller (CMON instance), allows building a highly available cluster of CMON instances to achieve ClusterControl high availability.

.. Note:: CMON HA feature is still a beta feature.

This command can help setup CMON with high availability feature using the following simple steps:

1) Install a CMON Controller together with the CMON Database serving as a permanent storage for the controller. The CMON HA will not replicate the CMON Database, so it has to be accessible from all the controllers and if necessary it has to provide redundancy on itself.
2) Enable the CMON HA subsystem using the ``--enable-cmon-ha`` on the running controller. This will create one CmonController class object. Check the object using the ``--list`` or ``--stat`` option. The CMON HA is now enabled, but there is no redundancy, only one controller is running. The one existing controller in this stage should be a leader although there are no followers.
3) Install additional Cmon Controllers one by one and start them the usual way. The next controllers should use the same CMON Database and should have the same configuration files. When the additional controllers are started they will find the leader in the CMON Database and will ask the leader to let them join. When the join is successful one more CmonController will be created for every joining controller.

.. Note:: See this two-part blog series to see how to install and configure ClusterControl CMON HA, `ClusterControl CMON HA for Distributed Database High Availability - Part 1 - Installation <https://severalnines.com/database-blog/clustercontrol-cmon-ha-distributed-database-high-availability-part-one-installation>`_ and `ClusterControl CMON HA for Distributed Database High Availability - Part 2 (GUI Access Setup) <https://severalnines.com/database-blog/clustercontrol-cmon-ha-distributed-database-high-availability-part-two-gui-access>`_.

**Usage**

.. code-block:: bash

	s9s controller {command} {options}

**Command**

========================================== ===================
Name, shorthand                            Description
========================================== ===================
|minus|\ |minus|\ create-snapshot          Creates job that will create a controller to controller snapshot of the Cmon HA subsystem. Creating a snapshot manually using this command line option is not necessary for the Cmon HA to operate, this command line option is made for testing and repairing.
|minus|\ |minus|\ enable-cmon-ha           Enabled the CMON HA subsystem. By default Cmon HA is not enabled for compatibility reasons, so this command line option is implemented to enable the controller to controller communication. When the CMON HA is enabled CmonController class objects will be created and used to implement the high  availability features (e.g. the leader election). So if the controller at least one CmonController object, the CMON HA is enabled, if not, it is not enabled.
|minus|\ |minus|\ list                     Lists the CmonController type objects known to the controller. If the CMON HA is not enabled there will be no such objects, if it is enabled one or more controllers will be listed. With the ``--long`` option a more detailed list will be shown where the state of the controllers can be checked.
|minus|\ |minus|\ ping                     Sends a ping request to the controller and prints the information received. Please note that there is an other ping request for the clusters, but this ping request is quite different from that. This request does not need a cluster, it is never redirected (follower controllers will also reply to this request) and it is replied with some basic information about the Cmon HA subsystem.
|minus|\ |minus|\ stat                     Prints more details about the controller objects.
========================================== ===================       


**Examples**

Create a controller to controller snapshot of CMON HA subsystem:

.. code-block:: bash

	$ s9s controller \
		--create-snapshot \
		--log

Enable CMON HA feature for the current CMON instance:

.. code-block:: bash

	$ s9s controller --enable-cmon-ha

.. Note:: See this two-part blog series to see how to install and configure ClusterControl CMON HA, `ClusterControl CMON HA for Distributed Database High Availability - Part 1 - Installation <https://severalnines.com/database-blog/clustercontrol-cmon-ha-distributed-database-high-availability-part-one-installation>`_ and `ClusterControl CMON HA for Distributed Database High Availability - Part 2 (GUI Access Setup) <https://severalnines.com/database-blog/clustercontrol-cmon-ha-distributed-database-high-availability-part-two-gui-access>`_.

List out all controllers participate in the CMON HA cluster:

.. code-block:: bash

	$ s9s controller \
		--list \
		--long

Send ping request to the controller and print the output in JSON format, with some text filtering on the output:

.. code-block:: bash

	$ s9s controller \
		--ping \
		--print-json \
		--json-format='status: ${controller_status}\n'


Print more details about the controller objects:

.. code-block:: bash

	$ s9s controller --stat
