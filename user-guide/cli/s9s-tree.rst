.. include:: <isotech.txt>

s9s tree
----------

Create, manage and manipulate CMON Directory Tree (CDT).

**Usage**

.. code-block:: bash

	s9s tree {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ access               Check the access rights for the authenticated user to the given CDT entry. This main option is made to be used in shell scripts, the exit code of the s9s program shows if the user has the privileges specified in the command line.
|minus|\ |minus|\ add-acl              Add an access control list (ACL) entry to an object in the tree. Overwrites the ACL if the given type of ACL already exists.
|minus|\ |minus|\ add-tag              This main option can be used to add a new tag to the tag list of an existing object.
|minus|\ |minus|\ cat                  This option can be used to print the content of a CDT file entry similar way the ``cat`` standard utility is used to print normal files.
|minus|\ |minus|\ chown                Change the ownership of an object. The new owner (and optionally the group owner) should be passed through the ``--owner`` command line option.
|minus|\ |minus|\ delete               Remove CDT entries.
|minus|\ |minus|\ get-acl              Print the ACL of a CDT entry.
|minus|\ |minus|\ list                 Print the Cmon Directory Tree in list format.
|minus|\ |minus|\ mkdir                Create a new directory in the tree.
|minus|\ |minus|\ move                 Move an object to a new location in the tree or rename the entry. If the target contains the ``/`` character, it is assumed to be a directory and the source entry will be moved to that directory with its name unchanged. If the target contains no ``/``, it is assumed to be a new name and the source entry will be renamed while kept in the same directory.
|minus|\ |minus|\ remove-acl           Remove an ACL entry from the ACL of an object.
|minus|\ |minus|\ remove-tag           Remove a tag from the tag list of an existing object.
|minus|\ |minus|\ rmdir                Remove an empty directory from the tree.
|minus|\ |minus|\ save                 Saves data (text) into an existing CDT entry that has the proper type (the type is file).
|minus|\ |minus|\ touch                Creates a CDT entry that is a file.
|minus|\ |minus|\ tree                 Print the tree in its original tree format.
|minus|\ |minus|\ watch                Opens an interactive UI to watch and manipulate the CDT filesystem and its entries.
====================================== ===========

**Options**

======================================== ===========
Name, shorthand                          Description
======================================== ===========
|minus|\ |minus|\ acl=ACLSTRING          An ACL entry in string format. See `ACL Text Forms`_.
|minus|\ |minus|\ all                    The CDT entries that has a name starting with ``.`` considered to be hidden entries. These are only printed if the ``--all command`` line option is provided.
|minus|\ |minus|\ owner=USER[:GROUP]     The user name and group name of the owner.
|minus|\ |minus|\ recursive              Print also the sub-items of the tree. The ``--chown`` will change the ownership for sub-items too. Please note that the ``--tree`` is always recursive, no need for this command line option there.
|minus|\ |minus|\ refresh                Recollect the data.
|minus|\ |minus|\ tag=STRING             Specify one single tag when adding or removing tags of a tag list that belongs to an object.
======================================== ===========


ACL Text Forms
++++++++++++++

A long and a short text form for representing ACLs is defined. In both forms, ACL entries are represented as three colon separated fields - an ACL entry tag type, an ACL entry qualifier, and the discretionary access permissions. The first field contains one of the following entry tag type keywords:

======== ===========
Tag type Description
======== ===========
user     A user ACL entry specifies the access granted to either the file owner (entry tag type ``ACL_USER_OBJ``) or a specified user (entry tag type ``ACL_USER``).
group    A group ACL entry specifies the access granted to either the file group (entry tag type ``ACL_GROUP_OBJ``) or a specified group (entry tag type ``ACL_GROUP``).
mask     A mask ACL entry specifies the maximum access which can be granted by any ACL entry except the user entry for the file owner and the other entry (entry tag type ``ACL_MASK``).
other    An other ACL entry specifies the access granted to any process that does not match any user or group ACL entries (entry tag type ``ACL_OTHER``).
======== ===========

The second field contains the user or group identifier of the user or group associated with the ACL entry for entries of entry tag type ``ACL_USER`` or ``ACL_GROUP``, and is empty for all other entries. A user identifier can be a user name or a user ID number in decimal form. A group identifier can be a group name or a group ID number in decimal form.

The third field contains the discretionary access permissions. The read, write and search/execute permissions are represented by the ``r``, ``w``, and ``x`` characters, in this order. Each of these characters is replaced by the - character to denote that a permission is absent in the ACL entry.  When converting from the text form to the internal representation, permissions that are absent need not be specified.

White space is permitted at the beginning and end of each ACL entry, and immediately before and after a field separator (the colon character).

LONG TEXT FORM
``````````````

The long text form contains one ACL entry per line. In addition, a number sign (``#``) may start a comment that extends until the end of the line. If an ``ACL_USER``, ``ACL_GROUP_OBJ`` or ``ACL_GROUP`` ACL entry contains permissions that are not also contained in the ``ACL_MASK`` entry, the entry is followed by a number sign, the string "effective:", and the effective access permissions defined by that entry. This is an example of the long text form:

.. code-block:: bash

	user::rw-
	user:lisa:rw-         #effective:r--
	group::r--
	group:toolies:rw-     #effective:r--
	mask::r--
	other::r--


SHORT TEXT FORM
````````````````

The short text form is a sequence of ACL entries separated by commas, and is used for input. Comments are not supported. Entry tag type keywords may either appear in their full unabbreviated form, or in their single letter abbreviated form. The abbreviation for user is ``u``, the abbreviation for group is ``g``, the abbreviation for mask is ``m``, and the abbreviation for other is ``o``.

The permissions may contain at most one each of the following characters in any order: ``r``, ``w``, ``x``.  These are examples of the short text form:

.. code-block:: bash

	u::rw-,u:lisa:rw-,g::r--,g:toolies:rw-,m::r--,o::r--
	g:toolies:rw,u:lisa:rw,u::wr,g::r,o::r,m::r

For more information, check out the POSIX Access Control Lists documentation by running ``man acl`` command.

**Examples**

List out all CDT objects in a tree structure view:

.. code-block:: bash

	$ s9s tree \
		--tree --all
	
Add an access control list to allow a group called "users" to have read (access the cluster) and write (make modification to the cluster) to a cluster named "galera_001":

.. code-block:: bash

	$ s9s tree \
		--add-acl \
		--acl="group:users:rw-" \
		galera_001
									
Tag a cluster with a string "Production" for a cluster named PostgreSQL_Cluster_001 which located at ``/PostgreSQL Cluster 1`` (as a CDT object):

.. code-block:: bash

	$ s9s tree \
		--add-tag \
		--tag="Production" \
		"/PostgreSQL Cluster 001"
									
Change the ownership of a cluster called "MariaDB 2 QA" (located at ``/MariaDB 2 QA`` as an CDT object) to user john and group DBA:
									
.. code-block:: bash

	$ s9s tree \
		--chown \
		--owner=john:DBA \
		--recursive \
		"/MariaDB 2 QA"

Create a new directory inside the CDT tree:

.. code-block:: bash

	$	s9s tree \
		--mkdir \
		/home/kirk
		
List out all objects under ``/MariaDB_Cluster_1`` including the hidden objects:

.. code-block:: bash

	$ s9s tree \
		--list \
		--long \
		--recursive \
		--all \
		--full-path \
		/MariaDB_Cluster_1
											
											
