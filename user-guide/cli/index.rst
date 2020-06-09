Also known as **s9s CLI**, **cc CLI** or **s9s-tools**, **s9s** is a command line tool binary introduced in ClusterControl version 1.4.1 to interact, control and manage database clusters using the ClusterControl Database Platform. Starting from version 1.4.1, the installer script will automatically install this package on the ClusterControl node. You can also install it on another computer or workstation to manage the database cluster remotely.

ClusterControl CLI opens a new door for cluster automation where you can easily integrate it with existing deployment automation tools like Ansible, Puppet, Chef or Salt. The command line tool is invoked by executing a binary called ``s9s``. The commands are basically JSON messages being sent over to the ClusterControl Controller (CMON) RPC interface. Communication between the s9s (the command line tool) and the cmon process (ClusterControl Controller) is encrypted using TLS and requires the port 9501 to be opened on controller and the client host.

The command line client installs manual pages and can be viewed by entering the command:

.. code-block:: bash

	$ man s9s-{command group}

For example:

.. code-block:: bash

	$ man s9s
	$ man s9s-cluster
	$ man s9s-alarm
	$ man s9s-backup

The general synopsis to execute commands using ``s9s`` is:

.. code-block:: bash

	s9s {command group} {options}

Supported command list:

===================== ===========
Command               Description
===================== ===========
`s9s account`_        Manage accounts on clusters.
`s9s alarm`_          Manage alarms.
`s9s backup`_         View, create and restore database backups.
`s9s cluster`_        List and manipulate clusters.
`s9s container`_      Manage virtual machines.
`s9s controller`_     Manage Cmon controllers.
`s9s job`_            View jobs.
`s9s maintenance`_    View and manipulate maintenance periods.
`s9s metatype`_       Print metatype information.
`s9s node`_           Handle nodes.
`s9s process`_        View processes running on nodes.
`s9s replication`_    Monitor and control data replication.
`s9s report`_         Manage reports.
`s9s script`_         Manage and execute scripts.
`s9s server`_         Manage hardware resources.
`s9s user`_           Manage users.
===================== ===========