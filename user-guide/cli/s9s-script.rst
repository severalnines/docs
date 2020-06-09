.. include:: <isotech.txt>

s9s script
----------

Manage and execute Advisor scripts available in the :ref:`MySQL - Manage - Developer Studio`.

**Usage**

.. code-block:: bash

	s9s script {command} {options}

**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ tree                 Prints available scripts on the controller in tree-like format.
|minus|\ |minus|\ execute              Executes a script from a local file.
|minus|\ |minus|\ system               Executes a shell command or an entire shell script on the nodes of the cluster showing the output of the command as job messages. See `Shell Command`_.
|minus|\ |minus|\ tree                 Print the names of the scripts stored on the controller in tree format.
====================================== ===========

**Options**

======================================== ===========
Name, shorthand                          Description
======================================== ===========
|minus|\ |minus|\ cluster-id=ID          The target cluster ID.
|minus|\ |minus|\ cluster-name=NAME, -n  The target cluster name.
======================================== ===========


Shell Command
+++++++++++++

If the ``--shell-command`` command line option is provided, the option argument will be executed. If a file name is provided as command line argument that content of the file will be executed as shell script. If neither found in the command line the s9s will read its standard input and execute the lines found as shell script on the nodes (this is the way to implement self contained remote executed shell scripts using a shebang).

Please note that this will be a job and it can be blocked by other jobs running on the same cluster.

.. code-block:: bash

	$ s9s script \
		--system \
		--log \
		--cluster-id=1 \
		--shell-command="df -h"

The command/script will be executed using the pre-configured OS user of the cluster. If that user has no superuser privileges the sudo utility can be used in the command or the script to gain superuser privileges.

By default the command/script will be executed on all nodes, all members of the cluster except the controller. This can be changed by providing a node list using the ``--nodes=`` command line option.

To execute shell commands the authenticated user has to have execute privileges on the nodes. If the execute privilege is not granted the credentials of an other Cmon User can be passed in the command line or the privileges can  be changed (see `s9s-tree`_ for details about owners, privileges and ACLs).

.. code-block:: bash

	$ s9s script \
		--system \
		--cmon-user=system \
		--password=mysecret \
		--log \
		--cluster-id=1 \
		--timeout=2 \
		--nodes='192.168.0.127;192.168.0.44' \
		--shell-command="df -h"

Please note that the  job will by default has a 10 seconds timeout, so if the command/script keeps running the job will be failed and the execution of the command(s) aborted. The timeout value can be set using the ``--timeout`` command line option.


**Examples**

Print the scripts available for cluster ID 1:

.. code-block:: bash

	$ s9s script --tree --cluster-id=1
	
Execute a script called ``test_script.sh`` on all nodes in the cluster with ID 1:

.. code-block:: bash

	$ s9s script \
		--system \
		--log \
		--log-format="%M\n" \
		--timeout=20 \
		--cluster-id=1 \
		test_script.sh