.. include:: <isotech.txt>

s9s metatype
------------

Lists meta-types supported by the controller.

**Usage**

.. code-block:: bash

	s9s metatype {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ list, -L             Lists the names of the types the controller supports. See `Property List`_.
|minus|\ |minus|\ list-cluster-types   Lists the cluster types the controller supports. With the ``--long`` option also lists the vendors and the versions.
|minus|\ |minus|\ list-properties      Lists the properties the given metatype has. Use the ``--type`` command line option to pass the name of the metatype.
====================================== ===========

**Options**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ type=TYPENAME        The name of the type.
====================================== ===========

Property List
+++++++++++++

Using the ``--list-properties`` and ``--long`` command line options a detailed list of the metatype properties can be printed:

.. code-block:: bash

	$ s9s metatype --list-properties --type=CmonUser --long
	ST NAME               UNIT DESCRIPTION
	rw email_address      -    The email address of the user.
	rw first_name         -    The first name of the user.
	rw groups             -    The list of groups for the user.
	rw job_title          -    The job title of the user.
	r- last_login         -    The date&time of the last successful login.

The list contains the following fields:

============================ ===========
Field                        Description
============================ ===========
ST                           The short status information, where at the first character position ``r`` stands for 'readable' and ``-`` shows that the property is not readable by the client program. At the second character position ``w`` stands for 'writable' and ``-`` shows that the property is not writable by the client.
NAME                         The name of the property.
UNIT                         The unit in which the given property is measured (e.g. 'byte'). This field shows a single ``-`` character if the unit is not applicable.
DESCRIPTION                  The short human readable description of the property.
============================ ===========

**Examples**

List the metatypes the controller supports:

.. code-block:: bash

	$ s9s metatype --list

List a detailed list of the properties the CmonUser type has:

.. code-block:: bash

	$ s9s metatype \
		--list-properties \
		--type=CmonUser \
		--long

List all cluster types currently managed by this controller:

.. code-block:: bash

	$ s9s metatype \
		--list-cluster-types \
		--long
