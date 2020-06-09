.. include:: <isotech.txt>

s9s server
----------

Provisions and manages virtualization host to be used to host database containers.

.. Note:: The supported virtualization platforms are LXC/LXD and Amazon Web Service EC2 instances. Docker is not supported at the moment.

LXC Containers
++++++++++++++

Handling lxc containers is a new feature added to the CMON Controller and the s9s command line tool. The basic functionality is available and tested, containers can created, started, stopped, deleted, even creating containers on the fly while installing clusters or cluster nodes is possible.

For the lxc containers one needs a container server, a computer that has the lxc software installed and configured and of course needs a proper account to access the container server from the CMON Controller. One can set up an lxc container server in two easy and one not so easy steps:

1. Install a Linux server and set it up so that the root user can ssh in from the Cmon Controller with a key, without a password.  Creating  such an access for the superuser is of course not the only way, it is just the easiest.
2. Register the server as a container server on the CMON Controller by issuing the s9s server ``--register --servers="lxc://IP_ADDRESS"`` command. This will install the necessary software and register the server as a container server to be used later.
3. The hard part is the network configuration on the container server. Most of the distributions by default have a network configuration that provides local (host only) IP address for the newly created containers. In order to provide public IP address for the containers, the container server must have some sort of bridging or NAT configured.

A possible way to configure the network for public IP is described in this blog, `Converting eth0 to br0 and getting all your LXC or LXD onto your LAN <https://insights.ubuntu.com/2015/11/10/converting-eth0-to-br0-and-getting-all-your-lxc-or-lxd-onto-your-lan>`_.

CMON-cloud Virtualization
+++++++++++++++++++++++++

The cmon-cloud containers are an experimental virtualization backend currently added to the CMON Controller as a brand new feature.

**Usage**

.. code-block:: bash

	s9s server {command} {options}

**Command**

========================================== ===========
Name, shorthand                            Description
========================================== ===========
|minus|\ |minus|\ add-acl                  Adds a new ACL entry to the server or modifies an existing ACL entry.
|minus|\ |minus|\ create                   Creates a new server. If this option is provided the controller will use SSH to discover the server and install the necessary software packages, modify the configuration if needed so that the server can host containers.
|minus|\ |minus|\ get-acl                  List the ACL of a server.
|minus|\ |minus|\ list-disks               List disks found in one or more servers.
|minus|\ |minus|\ list-images              List the images available on one or more servers. With the ``--long`` command line option a more detailed list is available.
|minus|\ |minus|\ list-memory              List memory modules from one or more servers.
|minus|\ |minus|\ list-nics                List network controllers from one or more servers.
|minus|\ |minus|\ list-partitions          List partitions from multiple servers.
|minus|\ |minus|\ list-processors          List processors from one or more servers.
|minus|\ |minus|\ list-subnets             List all the subnets exists on one or more servers.
|minus|\ |minus|\ register                 Registers an existing container server. If this command line option is provided the controller will register the server to be used as container server later. No software packages are installed or configuration changed.
|minus|\ |minus|\ start                    Boot up a server. This option will try to start up a server that is physically turned off (using e.g. the wake-on-lan feature).
|minus|\ |minus|\ stat                     Prints details about one or more servers.
|minus|\ |minus|\ stop                     Shuts down and power off a server. When this command line option is provided the controller will run the shutdown program on the server.
|minus|\ |minus|\ unregister               Unregisters a container server, simply removes it from the controller.
========================================== ===========

**Options**

=========================================== ===========
Name, shorthand                             Description
=========================================== ===========
|minus|\ |minus|\ log                       If the s9s application created a job and this command line option is provided it will wait until the job is executed. While waiting the job logs will be shown unless the silent mode is set.
|minus|\ |minus|\ recurrence=CRONTABSTRING  This option can be used to create recurring jobs, jobs that are repeated over and over again until they are manually deleted. Every time the job is repeated a new job will be instantiated by copying the original recurring job and starting the copy. The option argument is a crontab style string defining the recurrence of the job. See `Crontab`_.
|minus|\ |minus|\ schedule=DATETIME         The job will not be executed now but it is scheduled to execute later. The datetime string is sent to the backend, so all the formats are supported that is supported by the controller.
|minus|\ |minus|\ timeout=SECONDS           Sets the timeout for the created job. If the execution of the job is not done before the timeout counted from the start time of the job expires the job will fail. Some jobs might not support the timeout feature, the controller might ignore this value.
|minus|\ |minus|\ wait                      If the application created a job (e.g. to create a new cluster) and this command line option is provided the s9s program will wait until the job is executed. While waiting a progress bar will be shown unless the silent mode is set.
|minus|\ |minus|\ acl=ACLSTRING             The ACL entry to set.
|minus|\ |minus|\ os-key-file=PATH          The SSH key file to authenticate on the server. If none of the operating system authentication options are provided (``--os-key-file``, ``--os-password``, ``--os-user``) the controller will try top log in with the default settings.
|minus|\ |minus|\ os-password=PASSWORD      The SSH password to authenticate on the server. If none of the operating system authentication options are provided (``--os-key-file``, ``--os-password``, ``--os-user``) the controller will try top log in with the default settings.
|minus|\ |minus|\ os-user=USERNAME          The SSH username to authenticate on the server. If none of the operating system authentication options are provided (``--os-key-file``, ``--os-password``, ``--os-user``) the controller will try top log in with the default settings.
|minus|\ |minus|\ refresh                   Do not use cached data, collect information.
|minus|\ |minus|\ servers=LIST              List of servers.
=========================================== ===========

**Examples**

Register a virtualization host:

.. code-block:: bash

	$ s9s server --register --servers=lxc://storage01

Check the list of virtualization hosts:

.. code-block:: bash

	$ s9s server --list --long

Create a virtualization server with operating system username and password to be used to host containers. The controller will try to access the server using the specified credentials:

.. code-block:: bash

	$ s9s server \
		--create \
		--os-user=testuser \
		--os-password=passw0rd \
		--servers=lxc://192.168.0.250 \
		--log

