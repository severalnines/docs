.. _Requirements:

Requirements
============

This section provides detailed information on ClusterControl requirement on hardware, software, system environment and security configurations.

.. _Requirements - Hardware:

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

.. _Requirements - Operating System:

Operating system
----------------

ClusterControl has been tested on the following operating systems:

* RedHat/CentOS 6.x/7.x
* Ubuntu 12.04/14.04/16.04/18.04 LTS
* Debian 7.x/8.x/9.x

For the managed nodes, the deployment feature has been tested on the following operating systems:

* RedHat/CentOS 6.x/7.x
* Ubuntu 12.04/14.04/16.04/18.04 LTS
* Debian 7.x/8.x/9.x

The following do not work:

* CentOS 5.4 and earlier
* Fedora Core 16 and earlier

.. _Requirements - Software Dependencies:

Software Dependencies
---------------------

The following softwares are required by ClusterControl:

- MySQL server (5.1 or later, preferably 5.5 or later. MySQL 8.0 is not supported yet)
- MySQL client
- Apache web server (2.2 or later)
	- mod_rewrite
	- mod_ssl
	- allow .htaccess override
- PHP (5.4 or later)
	- RHEL: php, php-mysql, php-gd, php-ldap, php-curl
	- Debian: php5-common, php5-mysql, php5-gd, php5-ldap, php5-curl, php5-json
- Linux Kernel Security (SElinux or AppArmor) - must be disabled or set to permissive mode
- OpenSSH server/client
- BASH (recommended: version 4 or later)
- NTP server - All servers’ time must be synced under one time zone
- socat or netcat - for streaming backups

.. Note:: If ClusterControl is installed via installation script (install-cc) or package manager (yum/apt), all dependencies will be automatically satisfied.

.. _Requirements - Supported Browsers:

Supported Browsers
------------------

We highly recommend users to use the following web browsers when accessing ClusterControl UI:
	- Google Chrome
	- Mozilla Firefox
	
Ensure to keep up-to-date of these browsers as we are very likely taking advantage of the new features available in the latest version.

.. Note:: ClusterControl is built and tested only on the mentioned web browsers. Some major web browsers like Safari, Opera and Microsoft Edge could also work.

.. _Requirements - Supported Databases:

Supported Databases
-------------------

The following table shows supported database clusters with recommended minimum nodes:

+-----------------+----------------------------+-------------------------------------+---------------------------------------------------------------------------------+
| Database type   | Cluster type               | Version                             | Minimum recommended nodes                                                       |
+=================+============================+=====================================+=================================================================================+
| MySQL/MariaDB   | MySQL Cluster (NDB)        | 7.1 and later                       | 5 hosts (2 data nodes + 2 API/mgmd nodes + 1 ClusterControl node)               |
|                 +----------------------------+-------------------------------------+---------------------------------------------------------------------------------+
|                 | MySQL replication          | 5.1/5.5/5.6/5.7                     | 3 hosts (1 master node + 1 standby master/slave + 1 ClusterControl node)        |
|                 +----------------------------+-------------------------------------+---------------------------------------------------------------------------------+
|                 | * Percona XtraDB Cluster   | * 5.5/5.6/5.7 (MySQL/Percona)       | 4 hosts (3 nodes + 1 ClusterControl node)                                       |
|                 | * MariaDB Galera Cluster   | * 5.5/10.0/10.1/10.2/10.3 (MariaDB) |                                                                                 |
|                 +----------------------------+-------------------------------------+---------------------------------------------------------------------------------+
|                 | Single instance            | 5.5/5.6/5.7                         | 2 hosts (1 MySQL node + 1 ClusterControl node)                                  |
+-----------------+----------------------------+-------------------------------------+---------------------------------------------------------------------------------+
| MongoDB/Percona | Sharded cluster            | 3.2/3.4/3.6/4.0 (MongoDB only)      | 4 hosts (3 config servers / 3 shard servers / 2 mongos + 1 ClusterControl node) |
| Server for      +----------------------------+                                     +---------------------------------------------------------------------------------+
| MongoDB         | Replica set                |                                     | 4 hosts (3 replica servers + 1 ClusterControl node)                             |
+-----------------+----------------------------+-------------------------------------+---------------------------------------------------------------------------------+
| PostgreSQL      | Single instance            | >9.6/10.x/11.x                      | 2 hosts (1 PostgreSQL node + 1 ClusterControl node)                             |
|                 +----------------------------+                                     +---------------------------------------------------------------------------------+
|                 | Streaming Replication      |                                     | 3 hosts (1 master node + 1 slave node + 1 ClusterControl node)                  |
+-----------------+----------------------------+-------------------------------------+---------------------------------------------------------------------------------+

.. _Requirements - Firewall and Security Groups:

Firewall and Security Groups
----------------------------

It is important to secure the ClusterControl host and the database cluster. It is recommended for users to isolate the database infrastructure from public Internet and just whitelist the known hosts or networks to reach the database cluster.

ClusterControl requires ports used by the following services to be opened/enabled:

* ICMP (echo reply/request)
* SSH (default is 22)
* HTTP (default is 80)
* HTTPS (default is 443)
* MySQL (default is 3306)
* CMON RPC (default is 9500)
* CMON RPC TLS (default is 9501)
* CMON Events (default is 9510)
* CMON SSH (default is 9511)
* CMON Cloud (default is 9518)
* Streaming port for backups through socat/netcat (default is 9999)

ClusterControl supports various database and application vendors and each has its own set of standard ports that need to be reachable. Following ports and services need to be reachable by ClusterControl on the managed database nodes:

+-------------------------------------------------+----------------------------------------+
| Database Cluster (Vendor)                       | Port/Service                           |
+=================================================+========================================+
| MySQL/MariaDB (single instance and replication) | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 3306 (MySQL)                         |
+-------------------------------------------------+----------------------------------------+
| * MariaDB Galera Cluster                        | * 22 (SSH)                             |
| * Percona XtraDB Cluster                        | * ICMP (echo reply/request)            |
|                                                 | * 3306 (MySQL)                         |
|                                                 | * 4444 (SST)                           |
|                                                 | * 4567 TCP/UDP (Galera)                |
|                                                 | * 4568 (Galera IST)                    |
|                                                 | * 9200 (HAProxy health check)          |
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
| HAProxy                                         | * 22 (SSH)                             |
|                                                 | * ICMP (echo reply/request)            |
|                                                 | * 9600 (HAProxy stats)                 |
|                                                 | * 3307 (MySQL load-balanced)           |
|                                                 | * 3308 (MySQL load-balanced read-only) |
+-------------------------------------------------+----------------------------------------+
| MariaDB MaxScale                                | * 22 (SSH)                             |
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

.. _Requirements - Hostnames and IP Addresses:

Hostnames and IP Addresses
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

.. _Requirements - Operating System User:

Operating System User
---------------------

ClusterControl controller (cmon) process requires a dedicated operating system user to perform various management and monitoring commands on the managed nodes. This user which is defined as ``os_user`` or ``sshuser`` in CMON configuration file, must exist on all managed nodes and it should have the ability to perform super-user commands.

You are recommended to install ClusterControl as 'root', and running as root is the easiest option. If you perform the installation using another user other than 'root', the following must be true:

* The OS user must exist on all nodes
* The OS user must not be 'mysql'
* 'sudo' program must be installed on all hosts
* The OS user must be allowed to do 'sudo', i.e, it must be in sudoers
* The OS user must be configured with proper PATH environment variable. The following PATH are expected for user ``myuser``: ``PATH=/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/myuser/.local/bin:/home/myuser/bin``

.. Attention:: ClusterControl requires full access of sudo (all commands) for full functionality. Restricting the commands would cause some of the operations to fail (cluster recovery, failover, backup restoration, service control and cluster deployment).

For sudoers, using passwordless sudo is recommended. To setup a passwordless sudo user, open ``/etc/sudoers`` via text editor and add the following line at the end. Replace ``[OS user]`` with the sudo username of your choice:

.. code-block:: bash

  [OS user] ALL=(ALL) NOPASSWD: ALL

Open a new terminal to verify if it works. You should now be able to run the following command without entering a password:

.. code-block:: bash

  $ sudo ls /usr

You can also verify this with SSH command line used by CMON (assuming passwordless SSH has been setup correctly):

.. code-block:: bash

  $ ssh -qt [OS user]@[IP address/hostname] "sudo ls /usr"

where ``[OS user]`` is the name of the user you intend to use during the installation, and ``[IP address/hostname]`` is the IP address or hostname of a node in your cluster.

.. _Requirements - Passwordless SSH:

Passwordless SSH
----------------

Proper passwordless SSH setup from ClusterControl node to all nodes (including ClusterControl node) is mandatory. Before performing any operation on the managed node, the node must be accessible via SSH without using password but using key-based authentication instead.

ClusterControl uses :term:`libssh` which supports the following public key algorithms:

* ssh-rsa
* rsa-sha2-512
* rsa-sha2-256
* ssh-dss
* ssh-ed25519
* ecdsa-sha2-nistp256
* ecdsa-sha2-nistp384
* ecdsa-sha2-nistp521

.. Note:: Take note that ClusterControl is fully tested with RSA public key. Other supported key types should work on most cases.

.. _Requirements - Passwordless SSH - Setting up Passwordless SSH:

Setting up Passwordless SSH
+++++++++++++++++++++++++++

To setup a passwordless SSH, make sure you generate SSH keys (private and public keys) and copy the public key from the ClusterControl host as the designated user to the target host. Take note that ClusterControl also requires passwordless SSH to itself, so do not forget to set this up as described in the example below. 

Most of the sampling tasks for controller are done locally but there are some tasks that require a working self-passwordless SSH e.g: starting :term:`netcat` when performing backup (to stream backup to the other node). There are also various places where ClusterControl performs the execution "uniformly" regardless of the node's role or type. Hence, setting this up is compulsory and failing to do so will result ClusterControl to raise an alarm.

.. Note:: It is *NOT* necessary to setup two-way passwordless SSH, e.g: from the managed database node to the ClusterControl.

To generate a SSH key, use ``ssh-keygen`` command which is available with OpenSSH-client package. On ClusterControl node:

.. code-block:: bash

	$ whoami
	root
	$ ssh-keygen -t rsa # press Enter on all prompts

The above command will generate SSH RSA private and public key under user's home directory, ``/root/.ssh/``. The private key, ``id_rsa`` has to be kept secure on the node. The public key, ``id_rsa.pub`` should be copied over to all nodes that want to be accessed by ClusterControl passwordlessly.

The next step is to copy the SSH public key to all nodes. You may use ``ssh-copy-id`` command to achieve this if the destination node support password authentication:

.. code-block:: bash

  $ whoami
  root
  $ ls -1 ~/.ssh/id*
  /root/.ssh/id_rsa
  /root/.ssh/id_rsa.pub
  $ ssh-copy-id 192.168.0.10 # specify the root password of 192.168.0.10 if prompted

The command ``ssh-copy-id`` will simply copy the public key from the source server and add it into the destination server's authorized key list, default to ``~/.ssh/autohorized_keys`` of the authenticated SSH user. If password authentication is disabled, then manual copy is required. On ClusterControl node, copy the content of SSH public key located at ``~/.ssh/id_rsa.pub`` and paste it into ``~/.ssh/authorized_keys`` on all managed nodes (including ClusterControl server).

The following example shows how a root user on the ClusterControl host (192.168.0.10) generates and copies a SSH key to databases hosts (192.168.0.11, 192.168.0.12, 192.168.0.13) and to itself (192.168.0.10):

.. code-block:: bash

  $ whoami
  root
  $ ssh-keygen -t rsa # press Enter on all prompts
  $ ls -1 ~/.ssh/id*
  /root/.ssh/id_rsa
  /root/.ssh/id_rsa.pub
  $ ssh-copy-id 192.168.0.10 # specify the root password of 192.168.0.10 if prompted
  $ ssh-copy-id 192.168.0.11 # specify the root password of 192.168.0.11 if prompted
  $ ssh-copy-id 192.168.0.12 # specify the root password of 192.168.0.12 if prompted
  $ ssh-copy-id 192.168.0.13 # specify the root password of 192.168.0.13 if prompted

If you are running as a sudo user e.g "sysadmin", here is an example:

.. code-block:: bash

	$ whoami
	sysadmin
	$ ssh-keygen -t rsa # press Enter on all prompts
	$ ls -1 ~/.ssh/id*
	/home/sysadmin/.ssh/id_rsa
	/home/sysadmin/.ssh/id_rsa.pub
	$ ssh-copy-id 192.168.0.10 # specify the sysadmin password of 192.168.0.10 if prompted
	$ ssh-copy-id 192.168.0.11 # specify the sysadmin password of 192.168.0.11 if prompted
	$ ssh-copy-id 192.168.0.12 # specify the sysadmin password of 192.168.0.12 if prompted
	$ ssh-copy-id 192.168.0.13 # specify the sysadmin password of 192.168.0.13 if prompted

You should be able to SSH from ClusterControl to the other server(s) without password:

.. code-block:: bash

  $ ssh [username]@[server IP address]

For cloud users, you can use the corresponding key pair generated by the cloud provider by uploading it onto ClusterControl host and specify the physical path when configuring the SSH-related parameters in the ClusterControl UI (deploy cluster, import nodes, etc). ClusterControl will then use this key to perform tasks that require passwordless SSH and store the path via ``ssh_identity`` variable inside CMON configuration file:

.. code-block:: bash

  ssh_identity=/path/to/keypair/cloud.pem

If you use other public key algorithm (CMON defaults to RSA), make sure the public key generated on ClusterControl node is copied and allowed on all managed nodes under ``~/.ssh/autohorized_keys``. You can use ``ssh-copy-id`` command (as shown in the example above), or simply copying the public key to all managed nodes manually.

.. _Requirements - Passwordless SSH - Sudo Password:

Sudo password
+++++++++++++

Sudoers with or without password is possible with sudo configuration option. If undefined, CMON will escalate to sudoer without password. To specify the sudo password, add the following option inside the CMON configuration file:

.. code-block:: bash

  sudo="echo 'thesudopassword' | sudo -S 2>/dev/null"

.. Attention::  Having ``2>/dev/null`` in the sudo command is compulsory to strip out stderr from the response.

Don't forget to restart cmon service to load the option.

.. _Requirements - Passwordless SSH - Encrypted Home Directory:

Encrypted home directory
++++++++++++++++++++++++

If the sudo user's home directory is encrypted, you might be facing following scenarios:

* First SSH login will required password, even though you have copied the public key to the remote host's ``authorized_keys``.
* If you run another SSH session, while the first SSH session is still active, you will be able to authenticate without password and the key authentication is successful.

Encrypted home directories are not decrypted until the login is successful, and your SSH keys are stored in your home directory. The first SSH connection you make will require a password. While the subsequent connections will no longer need password since the SSH service is able to read the ``authorized_key`` (inside user's homedir) in decrypted environment.

To solve this, you need to follow the instructions in this page, `Passwordless SSH in Encrypted Home Directory <http://support.severalnines.com/entries/23490521-Passwordless-SSH-in-Encrypted-Home-Directory>`_.

.. _Requirements - Timezone:

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

.. _Requirements - License:

License
-------

ClusterControl comes in 4 versions - Community, Standalone, Advanced and Enterprise editions, within the same binary. Please review the `ClusterControl product page <http://www.severalnines.com/pricing>`_ for features comparison between these editions. To upgrade from Community to Standalone, Advanced or Enterprise, you would need a valid software license. When the license expires, ClusterControl defaults back to the Community Edition.

All installation methods automatically configures ClusterControl with a 30-day fully functional trial license. For commercial information, please `contact us <http://www.severalnines.com/contact>`_.