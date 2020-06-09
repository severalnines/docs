.. include:: <isotech.txt>

s9s container
-------------

Manage cloud and container virtualization. Multiple technologies (multiple virtualization backends) are supported (e.g. Linux LXC and AWS) providing various levels of virtualization. Throughout this documentation (and in fact in the command line options) s9s uses the word "container" to identify virtualized servers. The actual virtualization backend might use the term "virtual machine" or "Linux container" but s9s provides a high level generic interface to interact with them, so the generic "container" term is used. So please note, the term "container" does not necessarily mean "Linux container", it means "a server that is running in some kind of virtualized environment".

In order to utilize the s9s command line tool and the CMON Controller to manage virtualization, a virtualization host (container server) has to be installed first. The installation of such a container environment is documented in the `s9s server`_.

**Usage**

.. code-block:: bash

	s9s container {command} {options}

**Command**

========================================== ===========
Name, shorthand                            Description
========================================== ===========
|minus|\ |minus|\ create                   Creates and start a new container or virtual machine. If this option is provided, the controller will create a new job that creates a container. By default the container will also be started, an account will be created, passwordless sudo granted and the controller  will  wait  the  controller to obtain an IP address.
|minus|\ |minus|\ delete                   Stop and delete the container or virtual machine.
|minus|\ |minus|\ list, -L                 Lists the containers. See `Container List`_.
|minus|\ |minus|\ start                    Starts an existing container.
|minus|\ |minus|\ stat                     Prints the details of a container.
|minus|\ |minus|\ stop                     Stops the container. This will not remove the container by default, but it will stop it. If the container is set to be deleted on stop (temporary) it will be deleted.
========================================== ===========

**Options**

============================================ ===========
Name, shorthand                              Description
============================================ ===========
|minus|\ |minus|\ log                        Waits until the job is executed. While waiting the job logs will be shown unless the silent mode is set.
|minus|\ |minus|\ recurrence=CRONTABSTRING   This option can be used to create recurring jobs, jobs that are repeated over and over again until they are manually deleted. Every time the job is repeated a new job will be instantiated by copying the original recurring job and starting the copy. The option argument is a crontab style string defining the recurrence of the job. See `Crontab`_.
|minus|\ |minus|\ schedule=DATETIME          The job will not be executed now but it is scheduled to execute later. The datetime string is sent to the backend, so all the formats are supported that is supported by the controller.
|minus|\ |minus|\ timeout=SECONDS            Sets the timeout for the created job. If the execution of the job is not done before the timeout counted from the start time of the job expires the job will fail. Some jobs might not support the timeout feature, the controller might ignore this value.
|minus|\ |minus|\ wait                       Waits until the job is executed. While waiting a progress bar will be shown unless the silent mode is set.
|minus|\ |minus|\ cloud=PROVIDER             This option can be used when new container(s) created. The name of the cloud provider where the new container will be created. This command line option can also be used to filter the list of the containers when used together with one of the ``--list`` or ``--stat`` options.
|minus|\ |minus|\ containers=LIST            A list of containers to be created or managed. The containers can be passed as command line options (suitable for simple commands) or as an option argument for this command line option. The ``s9s container --stop node01`` and the ``s9s container --stop --containers=node01`` commands for example are equivalent. See `Create Container List`_.
|minus|\ |minus|\ image=NAME                 The name of the image from which the new container will be created. This option is not mandatory, when a new container is created the controller can choose an image if it is needed. To find out what images are supported by the registered container severs please issue the s9s server ``--list-images`` command.
|minus|\ |minus|\ os-key-file=PATH           The path of the SSH key to install on a new container to allow the user to log in. This command line option can be passed when a new container is created, the argument of the option should be the path of the private key stored on the controller. Although the path of the private key file is passed only the public key will be uploaded to the new container.
|minus|\ |minus|\ os-password=PASSWORD       This command line option can be passed when creating new containers to set the password for the user that will be created on the container. Please note that some virtualization backend might not support passwords, only keys.
|minus|\ |minus|\ os-user=USERNAME           This option may be used when creating new containers to pass the name of the user that will be created on the new container. Please note that this option is not mandatory, because the controller will create an account whose name is the same as the name of the cmon user creating the container. The public key of the cmon user will also be registered (if the user has an associated public key) so the user can actually log in.
|minus|\ |minus|\ servers=LIST               A list of servers to work with.
|minus|\ |minus|\ subnet-id=ID               This option can be used when new containers are created to set the subnet ID for the container. To find out what subnets are supported by the registered container severs please issue the s9s server ``--list-subnets`` command.
|minus|\ |minus|\ template=NAME              The name of the container template. See `Container Template`_.
|minus|\ |minus|\ volumes=LIST               When a new container is created this command line option can be used to pass a list of volumes that will be created for the container. See `Volume List`_.
|minus|\ |minus|\ vpc-id=ID                  This option can be used when new containers are created to set the VPC ID for the container. To find out what VPCs are supported by the registered container severs please issue the ``s9s server --list-subnets --long`` command.
============================================ ===========

Container List
++++++++++++++

Using the ``--list`` and ``--long`` command line options a detailed list of the containers can be printed. Here is an example of such a list:

.. code-block:: bash

	$ s9s container --list --long
	S TYPE TEMPLATE OWNER GROUP     NAME                IP ADDRESS    SERVER
	- lxc  -        pipas testgroup bestw_controller    -             core1
	u lxc  -        pipas testgroup dns1                192.168.0.2   core1
	u lxc  ubuntu   pipas testgroup ft_containers_35698 192.168.0.228 core1
	u lxc  -        pipas testgroup mqtt                192.168.0.5   core1
	- lxc  -        pipas testgroup ubuntu              -             core1
	u lxc  -        pipas testgroup www                 192.168.0.19  core1
	Total: 6 containers, 4 running.

The list contains the following fields:

=============== ===========
Field           Description
=============== ===========
S               The abbreviated status information. This is ``u`` for a container that is up and running and ``-`` otherwise.
TYPE            Shows what kind of container or virtual machine shown in this line, the type of the software that provides the virtualization.
TEMPLATE        The name of the template that is used to create the container.
OWNER           The owner of the server object.
GROUP           The group owner of the server object.
NAME            The name of the container. This is not necessarily the hostname, this is a unique name to identify the container on the host.
IP ADDRESS      The IP address of the container or the ``-`` character if the container has no IP address.
SERVER          The server on which the container can be found.
=============== ===========

Create Container List
+++++++++++++++++++++

The command line option argument is one or more containers separated by the ``;`` character. Each container is an URL defining the container name (an alias for the container) and zero or more properties. The string ``container05?parent_server=core1;container06?parent_server=core2`` for example defines two containers one on one server and the other is on an other server.

To see what properties are supported in the controller for the containers, one may use the following command:

.. code-block:: bash

	$ s9s metatype --list-properties --type=CmonContainer --long
	ST NAME            UNIT DESCRIPTION
	r- acl             -    The access control list.
	r- alias           -    The name of the container.
	r- architecture    -    The processor architecture.
	
See `Property List`_ for details.

Container Template
++++++++++++++++++

Defining a template is an easy way to set a number of complex properties without actually enumerating them in the command line one by one. The actual interpretation of the template name is up to the virtualization backend that is the protocol of the container server. The lxc backend  for example considers the template to be an already created container, it simply creates the new container by copying the template container so the new container inherits everything.

The template name can also be provided as a property name for the container, so the command ``s9s container --create --containers="node02?template=ubuntu;node03" --log`` for example will create two containers, one using a template, the other using the default settings.

Note that the ``--template`` command line option is not mandatory, if emitted suitable default values will be chosen, but if the template is provided and the template is not found the creation of the new container will fail.

Volume List
+++++++++++

The list can contain one or more volumes separated by the ``;`` character. Every volume consists three properties separated by the ``:`` character, a volume name, the volume size in gigabytes and a volume type that is either "hdd" or "ssd". The string ``vol1:5:hdd;vol2:10:hdd`` for example defines two hard-disk volumes, one 5GByte and one 10GByte.

For convenience, the volume name and the type can be omitted, so that automatically generated volume names are used.

**Examples**

Create a container no special information needed, every settings will use the default values. For this of course, at least one container server has to be pre-registered and properly working:

.. code-block:: bash

	$ s9s container --create --wait

Using the default, automatically chosen container names might not be the easiest way, so here is an example that provides a container name:

.. code-block:: bash

	$ s9s container --create --wait node01

This is equivalent with the following example that provides the container name through a command line option:

.. code-block:: bash

	$ s9s container --create --wait --containers="node01"
	
