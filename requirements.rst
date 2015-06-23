.. _requirements:

Requirements
============

This section provides detailed information on ClusterControl requirement on hardware, system environment and security policy.

Hardware
--------

Following is the expected system environment for the ClusterControl host:

* Architecture: x86_64 only
* RAM: >2 GB
* CPU: >2 cores
* Disk space: >20 GB
* Network: conventional network interface
* Hardware platform:
	* Bare-metal
	* Paravirtualization (VMware, VirtualBox, Xen)
	* HVM
* Cloud platform:
	* AWS EC2
	* RackSpace Cloud
	* Microsoft Azure
	* Google Cloud
* Internet connection (for selected cluster deployment purposes)

Operating system
----------------

ClusterControl has been tested on the following operating systems:

* Redhat/CentOS/Amazon Linux/Oracle Linux 5.8 and later
* Ubuntu 12.04/14.04 LTS
* Debian 6.0 and later

The following do not work:

* Centos 5.4 and earlier
* Fedora Core 16 and earlier

.. Important:: ClusterControl node must run under the same operating system distribution running on the database nodes. For instance, if you have a set of MySQL Cluster nodes running on CentOS 6.5, it is possible to have ClusterControl node run on Redhat 6.5. Mixing up Debian-based hosts with Redhat-based hosts is not possible. On top of that, mixing Ubuntu 14.04 with 12.04 (and lower) is not recommended since they both are running different Apache version with distinct configuration.

Software Dependencies
---------------------

The following software is required by ClusterControl:

- MySQL server (5.1 or later, preferably 5.5 or later)
- MySQL client
- Apache web server (2.2 or later)
- PHP (5 or later)
- Linux Kernel Security (SElinux or AppArmor) - must be disabled or set to permissive mode
- OpenSSH server/client
- BASH (recommended: version 4 or later)
- NTP server - All servers’ time must synced under a same time zone
- netcat - for streaming backups

.. Note:: If ClusterControl is installed via deployment package (generated with Severalnines Configurator), installation script (install-cc.sh), bootstrap script (s9s_bootstrap) or package manager (yum/apt), all dependencies will be automatically satisfied.*


Supported Clusters
------------------

The following table shows supported database clusters with recommended minimum nodes:

+----------------+----------------------------+-----------------------------------------------------------------------------+
| Database type  | Cluster type               | Minimum recommended nodes                                                   |
+================+============================+=============================================================================+
| MySQL/MariaDB  | MySQL Cluster (NDB)        | 5 hosts (2 data nodes + 2 API/mgmd nodes + 1 ClusterControl node)           |
|                +----------------------------+-----------------------------------------------------------------------------+
|                | MySQL replication          | 3 hosts (1 master node + 1 standby master/slave + 1 ClusterControl node)    |
|                +----------------------------+-----------------------------------------------------------------------------+
|                | * MYSQL Galera Cluster     | 4 hosts (3 master nodes + 1 ClusterControl node)                            |
|                | * Percona XtraDB Cluster   |                                                                             |
|                | * MariaDB Galera Cluster   |                                                                             |
|                +----------------------------+-----------------------------------------------------------------------------+
|                | Single instance            | 2 hosts (1 MySQL node + 1 ClusterControl node)                              |
+----------------+----------------------------+-----------------------------------------------------------------------------+
| MongoDB/TokuMX | Replicated sharded cluster | 4 hosts (3 config servers/2 mongos/2 shard servers + 1 ClusterControl node) |
|                +----------------------------+-----------------------------------------------------------------------------+
|                | Sharded cluster            | 4 hosts (3 config servers/2 mongos/2 shard servers + 1 ClusterControl node) |
|                +----------------------------+-----------------------------------------------------------------------------+
|                | Replica set                | 3 hosts (2 replica servers + 1 ClusterControl node)                         |
+----------------+----------------------------+-----------------------------------------------------------------------------+
| PostgreSQL     | Single instance            | 2 hosts (1 PostgreSQL node + 1 ClusterControl node)                         |
+----------------+----------------------------+-----------------------------------------------------------------------------+

Firewall and Security Groups
----------------------------

If you used Severalnines Configurator to deploy a cluster, the deployment script disables firewalls by default to minimize the possibilities of failure during the cluster deployment. Once it is completed, it is important to secure the ClusterControl node and the database cluster. We recommend user to isolate their database infrastructure from the public Internet and just whitelist the known hosts or networks to connect to the database cluster.

ClusterControl requires ports used by the following services to be opened/enabled:

* ICMP (echo reply/request)
* SSH (default is 22)
* HTTP (default is 80)
* HTTPS (default is 443)
* MySQL (default is 3306)
* CMON RPC (default is 9500)
* HAproxy stats (if installed on ClusterControl node - default is 9600)
* MySQL load balance (if HAproxy installed on ClusterControl node - default is 3307)
* Streaming port for mysqldump through netcat (default is 9999)


ClusterControl supports various database and application vendors and each has its own set of standard ports that need to be reachable. Following ports and services need to be reachable by ClusterControl on the managed database nodes:

+-------------------------------------------------+--------------------------------------+
| Database Cluster (Vendor)                       | Port/Service                         |
+=================================================+======================================+
| MySQL/MariaDB (Single instance and replication) | * 22 (SSH)                           |
|                                                 | * ICMP (echo reply/request)          |
|                                                 | * 3306 (MySQL)                       |
+-------------------------------------------------+--------------------------------------+
| * MySQL Galera Cluster                          | * 22 (SSH)                           |
| * Percona XtraDB Cluster                        | * ICMP (echo reply/request)          |
| * MariaDB Galera Cluster                        | * 3306 (MySQL)                       |
|                                                 | * 4444 (SST)                         |
|                                                 | * 4567 TCP/UDP (Galera)              |
|                                                 | * 4568 (Galera IST)                  |
|                                                 | * 9200 (HAproxy health check)        |
+-------------------------------------------------+--------------------------------------+
| MySQL Cluster                                   | * 22 (SSH)                           |
|                                                 | * ICMP (echo reply/request)          |
|                                                 | * 1186 (MySQL Cluster)               |
|                                                 | * 2200 (MySQL Cluster)               |
|                                                 | * 3306 (MySQL)                       |
+-------------------------------------------------+--------------------------------------+
| MongoDB/TokuMX replica set                      | * 22 (SSH)                           |
|                                                 | * ICMP (echo reply/request)          |
|                                                 | * 27017 (mongod)                     |
+-------------------------------------------------+--------------------------------------+
| MongoDB/TokuMX sharded cluster                  | * 22 (SSH)                           |
|                                                 | * ICMP (echo reply/request)          |
|                                                 | * 27018 (mongod)                     |
|                                                 | * 27017 (mongos)                     |
|                                                 | * 27019 (config server)              |
+-------------------------------------------------+--------------------------------------+
| HAproxy                                         | * 22 (SSH)                           |
|                                                 | * ICMP (echo reply/request)          |
|                                                 | * 9600 (HAproxy stats)               |
|                                                 | * 3307 or 33306 (MySQL load-balanced)|
+-------------------------------------------------+--------------------------------------+
| Keepalived                                      | * 22 (SSH)                           |
|                                                 | * ICMP (echo reply/request)          |
|                                                 | * 224.0.0.0/8 (multicast request)    |
|                                                 | * IP protocol 112 (VRRP)             |
+-------------------------------------------------+--------------------------------------+
| Galera Arbitrator (garbd)                       | * 22 (SSH)                           |
|                                                 | * ICMP (echo reply/request)          |
|                                                 | * 4567 (Galera)                      |
+-------------------------------------------------+--------------------------------------+

Hostnames and IP addresses
--------------------------

It is recommended for users to setup a proper host definition file in ``/etc/hosts`` file. The file should be identical on all servers in your cluster. Otherwise, your database cluster might not work as expected with ClusterControl. Below is an example of a host definition file:

.. code-block:: bash

  127.0.0.1 	localhost.localdomain localhost
  10.0.1.10 	clustercontrol clustercontrol.example.com
  10.0.1.11 	server1 server1.example.com
  10.0.1.12 	server2 server2.example.com

You need to separate the 127.0.0.1 entry from your real hostname, specifying it only to ``localhost`` or ``localhost.localdomain``. To verify whether you have set up the hostname correctly, ensure the following command returns the primary IP address:

.. code-block:: bash

  $ hostname -I
  10.0.1.10 # This is good. IP address returned is neither 127.0.0.1 nor 127.0.1.1

Operating System User
---------------------

ClusterControl controller (cmon) process requires a dedicated operating system user to perform various management and monitoring commands on the managed nodes. This user which is defined as ``os_user`` or ``sshuser`` in CMON configuration file, must exist on all managed nodes and it should have ability to perform super-user commands.

You are recommended to install ClusterControl as 'root', and running as root is the easiest option. If you perform the install using another user other than 'root', the following must be true:

* The OS user must exist on all nodes
* The OS user must not be 'mysql'
* 'sudo' program must be installed on all hosts
* The OS user must be allowed to do 'sudo', i.e, it must be in sudoers

For sudoers, using passwordless sudo is recommended. To setup a passwordless sudo user, add following line into ``/etc/sudoers``:

.. code-block:: bash

  # add the following line at the end. Replace [OS user] with the sudo username of your choice.
  [OS user] ALL=(ALL) NOPASSWD: ALL

Open a new terminal to verify it works. You should now be able to run the command below without entering a password:

.. code-block:: bash

  $ sudo ls /usr

You can also verify this with SSH command line used by CMON (assuming passwordless SSH has been setup correctly):

.. code-block:: bash

  $ ssh -qt [OS user]@[IP address/hostname] "sudo ls /usr"

where ``[OS user]`` is the name of the user you intend to use during the installation, and ``[IP address/hostname]`` is the IP address or hostname of a node in your cluster.

Passwordless SSH
----------------

Proper passwordless SSH setup from ClusterControl node to all nodes (including ClusterControl node) is mandatory. If ClusterControl is installed using the deployment package generated from the Severalnines Configurator or using one of our bootstrap scripts, the deployment script will guide users on setting up SSH keys before proceeding with the install.

Setting up passwordless SSH
+++++++++++++++++++++++++++

To setup a passwordless SSH, make sure you generate a SSH key and copy it from the ClusterControl host as the designated user to the target host. Take note that ClusterControl also requires passwordless SSH to itself, so do not forget to set this up as described in the example below. It is NOT neccessary to setup two-way passwordless SSH, e.g: from the managed database node to the ClusterControl.

Examples below shows how a root user on the ClusterControl host generates and copies a SSH key to a database host, 192.168.0.10:

.. code-block:: bash

  $ whoami
  root
  $ ssh-keygen -t rsa # press Enter on all prompts
  $ ssh-copy-id 192.168.0.10 # insert the root password of 192.168.0.10 if prompted

.. Attention::  Repeat the ``ssh-copy-id`` command to all nodes (including ClusterControl node)

If you are running as a sudo user e.g sysadmin, here is an example:

.. code-block:: bash

  $ whoami
  sysadmin
  $ ssh-keygen -t rsa # press Enter on all prompts
  $ ssh-copy-id 192.168.0.10 # insert the sysadmin password of 192.168.0.10 if prompted

.. Attention::  Repeat the ``ssh-copy-id`` command to all nodes (including ClusterControl node)

You should now able to SSH from ClusterControl to the other server(s) without password:

.. code-block:: bash
  $ ssh [username]@[server IP address]

If it does not work, check permissions of the ``.ssh`` directory and the files in it. Some users need to set the following in their ``/etc/ssh/sshd_config`` file:

.. code-block:: bash

  RSAAuthentication=Yes

Do not forget to restart SSH daemon if you make changes in the ``sshd_config`` file.

In order to prevent a long running SSH connection to be terminated by the firewall or switch, you may also want to set in ``/etc/ssh/ssh_config`` on the ClusterControl node:

.. code-block:: bash

  ServerAliveInterval 30
  ServerAliveCountMax 10

For AWS cloud users, you can use the corresponding key pair by uploading it onto the ClusterControl host and specifying the physical location under ``ssh_identity`` in CMON configuration file:

.. code-block:: bash

  ssh_identity=/path/to/keypair/aws.pem

If you use DSA (CMON defaults to RSA), then you need to follow `these instructions <http://support.severalnines.com/entries/23498833-Using-DSA-keys-instead-of-RSA-key-based-authentitication>`_.


Sudo password
+++++++++++++

Sudoers with or without password is possible with sudo configuration option (though it is not recommended since CMON supports alphanumeric characters only). If undefined, CMON will escalate sudo user without password. To specify the sudo password, add the following option inside the CMON configuration file:

.. code-block:: bash

  sudo="echo 'thesudopassword' | sudo -S 2>/dev/null"

Don't forget to restart cmon service to load the option.

Encrypted home directory
++++++++++++++++++++++++

If the sudo user's home directory is encrypted, you might be facing following scenarios:

* First SSH login will required password, even though you have copied the public key to the remote host ``authorized_keys``
* If you run another SSH session, while the first SSH session still active, you will able to authenticate without password and the key authentication is successful.

Encrypted home directories aren’t decrypted until the login is successful, and your SSH keys are stored in your home directory. The first SSH connection you make will require a password. While the subsequent connections will no longer need password since the SSH service is able to read the ``authorized_key`` (inside user's homedir) in decrypted environment.

To solve this, you need to follow `these instructions <http://support.severalnines.com/entries/23490521-Passwordless-SSH-in-Encrypted-Home-Directory>`_.

Timezone
--------

ClusterControl requires all servers' time to be synchronized and to run within a same time zone. Verify this by using following command:

.. code-block:: bash

  $ date
  Mon Sep 17 22:59:24 UTC 2013

To change time zone, e.g from UTC to Pacific time:

.. code-block:: bash

	$ rm /etc/localtime
	$ ln -sf /usr/share/zoneinfo/US/Pacific localtime

UTC is however recommended. Configure NTP client for each host with a working time server to avoid time drifting between hosts which could cause inaccurate reporting or graphs not being plotted properly. To immediately sync a server’s time with a time server, use following command:

.. code-block:: bash

	$ ntpdate -u [NTP server, e.g europe.pool.ntp.org]

License
-------

ClusterControl comes in three versions, Community, Pro and Enterprise editions, within the same binary. Please review the `ClusterControl product page <http://www.severalnines.com/pricing>`_ for a feature comparison between these editions. To upgrade from Community to Pro or Enterprise, you would need a valid software license. When the license expires, ClusterControl defaults back to the Community Edition.

All installation methods automatically configures ClusterControl with a 14-days fully functional trial license. For commercial information, please `contact us <http://www.severalnines.com/contact>`_.
