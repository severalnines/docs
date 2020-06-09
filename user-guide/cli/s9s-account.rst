.. include:: <isotech.txt>

s9s account
-----------

Manage user accounts on clusters. The "account" term in this section can be treated as database user account of a managed database server/cluster.

**Usage**

.. code-block:: bash

	s9s account {command} {options}
	

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ create               Creates a new account on the cluster. Note that the account is an account of the cluster and not a user of the Cmon system.
|minus|\ |minus|\ delete               Removes an account.
|minus|\ |minus|\ list, -L             Lists the accounts on the cluster.
====================================== ===========

**Options**

========================================================== ===========
Name, shorthand                                            Description
========================================================== ===========
|minus|\ |minus|\ account=USERNAME[:PASSWORD][@HOSTNAME]   The account to be used or created on the cluster. The command line option argument may contain a username, a password for the user and a hostname identifying the host from where the user may log in. The s9s command line tool will handle the command line option argument as an URL encoded string, so if the password for example contains an ``@`` character, it should be encoded as ``%40``. URL encoded parts are supported anywhere in the string, usernames and passwords (and even hostnames) may also have special characters.
|minus|\ |minus|\ grant                                    Grant privileges for an account on one or more databases.
|minus|\ |minus|\ list                                     Lists the accounts on the cluster.
|minus|\ |minus|\ private                                  Create a secure, more restricted account on the cluster. The actual interpretation of this flag depends on the  controller, the current version is restricting the access to the ProxySQL servers. The account that is created with the ``--private`` option will not be imported into the ProxySQL to have access through the ProxySQL server immediately after they created on the cluster.
|minus|\ |minus|\ privileges=EXPRESSION                    Privileges to be granted to a user account on the server. See `Privilege Expression`_.
|minus|\ |minus|\ with-database                            Creates a database for the new account while creating a new user account on the cluster. The name of the database will be the same as the name of the account and all access rights will be granted for the account to use the database.
========================================================== ===========

Privilege Expression
+++++++++++++++++++++

The privileges are specified using a simple language that is interpreted by the CMON Controller. The language is specified as follows:

.. code-block:: bash

	expression: specification[;...]
	specification: [object[,...]:]privilege[,...]
	object: {
		*
		| *.*
		| database_name.*
		| database_name.table_name
		| database_name
	}

Please note that an object name on itself is a database name (and not a table name) and multiple objects can be enumerated by using the ``,`` as separator. It is also important that multiple specifications can be enumerated using the semicolon (``;``) as separator.

The expression ``MyDb:INSERT,UPDATE;Other:SELECT`` for example defines INSERT and UPDATE privileges on the MyDb database and SELECT privilege on the Other database. The expression ``INSERT,UPDATE`` on the other hand would specify INSERT and UPDATE privileges on all databases and all tables.

**Examples**

Create a new MySQL user account "myuser" with password "secr3tP4ss", and allow it to have ALL PRIVILEGES on database "shop_db" while SELECT on table "account_db.payments":

.. code-block:: bash

	$ s9s account \
		--create \
		--cluster-id=1 \
		--account="myuser:secr3tP4ss@192.168.0.100" \
		--privileges="shop_db.*:ALL;account_db.payments:SELECT"

Create a new PostgreSQL user account called "mydbuser", and allowed the host in the network subnet 192.168.0.0/24 to access database mydbshop:

.. code-block:: bash

	$ s9s account --create \
		--cluster-id=50 \
		--account='mydbuser:k#1298dsajhucb9@192.168.0.0/24' \
		--privileges="mydbshop.*:ALL"

Delete a database user called "joe":
              
.. code-block:: bash

	$ s9s account \
		--delete \
		--cluster-id=1 \
		--account="joe"

Lists the accounts on the cluster.

.. code-block:: bash

	$ s9s account \
		--list \
		--long \
		--cluster-id=1

