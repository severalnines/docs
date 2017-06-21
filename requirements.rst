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
* Network: conventional network interface (eth, en, em)
* Hardware platform:
	* Bare-metal
	* Virtualization
		* VMware
		* VirtualBox
		* Xen
		* Docker
* Tested cloud platform:
	* AWS EC2
	* RackSpace Cloud
	* Digital Ocean
* Internet connection (for selected cluster deployment)

Operating system
----------------

ClusterControl has been tested on the following operating systems:

* Redhat/CentOS/Oracle Linux 6 and later
* Ubuntu 12.04/14.04/16.04 LTS
* Debian 7.0 and later

The following do not work:

* Centos 5.4 and earlier
* Fedora Core 16 and earlier

Software Dependencies
---------------------

The following software is required by ClusterControl:

- MySQL server (5.1 or later, preferably 5.5 or later)
- MySQL client
- Apache web server (2.2 or later)
	- mod_rewrite
	- mod_ssl
	- allow .htaccess override
- PHP (5 or later)
	- RHEL: php, php-mysql, php-gd, php-ldap, php-curl
	- Debian: php5-common, php5-mysql, php5-gd, php5-ldap, php5-curl, php5-json
- Linux Kernel Security (SElinux or AppArmor) - must be disabled or set to permissive mode
- OpenSSH server/client
- BASH (recommended: version 4 or later)
- NTP server - All servers’ time must be synced under one time zone
- netcat - for streaming backups

.. Note:: If ClusterControl is installed via installation script (install-cc) or package manager (yum/apt), all dependencies will be automatically satisfied.

Supported Browsers
------------------

We highly recommend user to use the following web browsers when accessing ClusterControl UI:
	- Google Chrome
	- Mozilla Firefox
	
Ensure to keep up-to-date of these browsers as we are very likely taking advatange of the new features available in the latest version.

.. Note:: ClusterControl is built and tested only on the mentioned web browsers. Some major web browsers like Safari, Opera and Internet Explorer could also work.

Supported Databases
-------------------

The following table shows supported database clusters with recommended minimum nodes:

+----------------+----------------------------+--------------------------------+-----------------------------------------------------------------------------+
| Database type  | Cluster type               | Version                        | Minimum recommended nodes                                                   |
+================+============================+================================+=============================================================================+
| MySQL/MariaDB  | MySQL Cluster (NDB)        | 7.1 and later                  | 5 hosts (2 data nodes + 2 API/mgmd nodes + 1 ClusterControl node)           |
|                +----------------------------+--------------------------------+-----------------------------------------------------------------------------+
|                | MySQL replication          | 5.5 and later                  | 3 hosts (1 master node + 1 standby master/slave + 1 ClusterControl node)    |
|                +----------------------------+--------------------------------+-----------------------------------------------------------------------------+
|                | * MySQL Galera Cluster     | * 5.5/5.6/5.7 (MySQL/Percona)  | 4 hosts (3 master nodes + 1 ClusterControl node)                            |
|                | * Percona XtraDB Cluster   | * 5.5/10.0/10.1 (MariaDB)      |                                                                             |
|                | * MariaDB Galera Cluster   |                                |                                                                             |
|                +----------------------------+--------------------------------+-----------------------------------------------------------------------------+
|                | Single instance            | 5.5 and later                  | 2 hosts (1 MySQL node + 1 ClusterControl node)                              |
+----------------+----------------------------+--------------------------------+-----------------------------------------------------------------------------+
| MongoDB        | Replicated sharded cluster | 3.2 and later                  | 4 hosts (3 config servers/2 mongos/2 shard servers + 1 ClusterControl node) |
|                +----------------------------+                                +-----------------------------------------------------------------------------+
|                | Sharded cluster            |                                | 4 hosts (3 config servers/2 mongos/2 shard servers + 1 ClusterControl node) |
|                +----------------------------+                                +-----------------------------------------------------------------------------+
|                | Replica set                |                                | 3 hosts (2 replica servers + 1 ClusterControl node)                         |
+----------------+----------------------------+--------------------------------+-----------------------------------------------------------------------------+
| PostgreSQL     | Single instance            | 9.x                            | 2 hosts (1 PostgreSQL node + 1 ClusterControl node)                         |
|                +----------------------------+                                +-----------------------------------------------------------------------------+
|                | Replication                |                                | 3 hosts (1 master node + 1 slave node + 1 ClusterControl node)              |
+----------------+----------------------------+--------------------------------+-----------------------------------------------------------------------------+

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
* HAProxy stats (if installed on ClusterControl node - default is 9600)
* MySQL load balance (if HAProxy is installed on ClusterControl node - default is 3307)
* Streaming port for mysqldump through netcat (default is 9999)

ClusterControl supports various database and application vendors and each has its own set of standard ports that need to be reachable. Following ports and services need to be reachable by ClusterControl on the managed database nodes:

+-------------------------------------------------+----------------------------------------+
| Database Cluster (Vendor)                       | Port/Service                           |
+=================================================+========================================+
| MySQL/MariaDB (Single instance and replication) | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 3306 (MySQL)                         |
+-------------------------------------------------+----------------------------------------+
| * MySQL Galera Cluster                          | * 22 (SSH)                             |
| * Percona XtraDB Cluster                        | * ICMP (echo reply/request)            |
| * MariaDB Galera Cluster                        | * 3306 (MySQL)                         |
|                                                 | * 4444 (SST)                           |
|                                                 | * 4567 TCP/UDP (Galera)                |
|                                                 | * 4568 (Galera IST)                    |
|                                                 | * 9200 (HAproxy health check)          |
+-------------------------------------------------+----------------------------------------+
| MySQL Cluster (NDB)                             | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 1186 (MySQL Cluster)                 |
|                                                 | * 2200 (MySQL Cluster)                 |
|                                                 | * 3306 (MySQL)                         |
+-------------------------------------------------+----------------------------------------+
| MongoDB replica set                             | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 27017 (mongod)                       |
+-------------------------------------------------+----------------------------------------+
| MongoDB sharded cluster                         | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 27018 (mongod)                       |
|                                                 | * 27017 (mongos)                       |
|                                                 | * 27019 (config server)                |
+-------------------------------------------------+----------------------------------------+
| PostgreSQL                                      | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 5432 (postgres)                      |
+-------------------------------------------------+----------------------------------------+
| HAproxy                                         | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 9600 (HAproxy stats)                 |
|                                                 | * 3307 (MySQL load-balanced)           |
|                                                 | * 3308 (MySQL load-balanced read-only) |
+-------------------------------------------------+----------------------------------------+
| MaxScale                                        | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 6033 (MaxAdmin - CLI)                |
|                                                 | * 4006 (Round robin listener)          |
|                                                 | * 4008 (Read/Write split listener)     |
|                                                 | * 4442 (Debug information)             |
+-------------------------------------------------+----------------------------------------+
| Keepalived                                      | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 224.0.0.0/8 (multicast request)      |
|                                                 | * IP protocol 112 (VRRP)               |
+-------------------------------------------------+----------------------------------------+
| Galera Arbitrator (garbd)                       | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 4567 (Galera)                        |
+-------------------------------------------------+----------------------------------------+
| ProxySQL                                        | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 6032 (ProxySQL Admin)                |
|                                                 | * 6033 (MySQL load-balanced)           |
+-------------------------------------------------+----------------------------------------+

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

ClusterControl controller (cmon) process requires a dedicated operating system user to perform various management and monitoring commands on the managed nodes. This user which is defined as ``os_user`` or ``sshuser`` in CMON configuration file, must exist on all managed nodes and it should have the ability to perform super-user commands.

You are recommended to install ClusterControl as 'root', and running as root is the easiest option. If you perform the installation using another user other than 'root', the following must be true:

* The OS user must exist on all nodes
* The OS user must not be 'mysql'
* 'sudo' program must be installed on all hosts
* The OS user must be allowed to do 'sudo', i.e, it must be in sudoers

For sudoers, using passwordless sudo is recommended. To setup a passwordless sudo user, add following line into ``/etc/sudoers``:

Edit the sudoers with the following command (as root):

.. code-block:: bash

  visudo

And add the following line at the end. Replace ``[OS user]`` with the sudo username of your choice:

.. code-block:: bash

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

Proper passwordless SSH setup from ClusterControl node to all nodes (including ClusterControl node) is mandatory. When adding a new node, the node must be accessible via passwordless SSH from ClusterControl beforehand.

Setting up passwordless SSH
+++++++++++++++++++++++++++

To setup a passwordless SSH, make sure you generate a SSH key and copy it from the ClusterControl host as the designated user to the target host. Take note that ClusterControl also requires passwordless SSH to itself, so do not forget to set this up as described in the example below. 

Most of the sampling tasks for controller are done locally but there are some tasks that require a working self-passwordless SSH e.g: starting netcat when performing backup (to stream created backup to the other node). There are also various places where ClusterControl performs the execution "uniformly" regardless of the node's role or type. So, setting this up is required and failing to do so will result ClusterControl to raise an alarm.

.. Note:: It is *NOT* neccessary to setup two-way passwordless SSH, e.g: from the managed database node to the ClusterControl.

Examples below show how a root user on the ClusterControl host generates and copies a SSH key to a database host, 192.168.0.10:

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

Sudoers with or without password is possible with sudo configuration option. If undefined, CMON will escalate to sudoer without password. To specify the sudo password, add the following option inside the CMON configuration file:

.. code-block:: bash

  sudo="echo 'thesudopassword' | sudo -S 2>/dev/null"

.. Attention::  Having ``2>/dev/null`` in the sudo command is compulsory to exclude stderr from the response.

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

UTC is however recommended. Configure NTP client for each host with a working time server to avoid time drifting between hosts which could cause inaccurate reporting or incorrect graphs plotting. To immediately sync a server’s time with a time server, use following command:

.. code-block:: bash

	$ ntpdate -u [NTP server, e.g europe.pool.ntp.org]

License
-------

ClusterControl comes in 4 versions - Community, Standalone, Advanced and Enterprise editions, within the same binary. Please review the `ClusterControl product page <http://www.severalnines.com/pricing>`_ for features comparison between these editions. To upgrade from Community to Standalone, Advanced or Enterprise, you would need a valid software license. When the license expires, ClusterControl defaults back to the Community Edition.

All installation methods automatically configures ClusterControl with a 30-days fully functional trial license. For commercial information, please `contact us <http://www.severalnines.com/contact>`_.
