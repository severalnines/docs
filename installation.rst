Installation
============

This section provides detailed information on how to get ClusterControl installed on your environment. If you are looking for a simpler way to install ClusterControl, please have a look at `Getting Started <getting-started.html>`_.

Severalnines Repository
-----------------------

Severalnines provides YUM/APT repositories accessible at http://repo.severalnines.com . Automatic installation procedure (as described in `Automatic Installation`_) automatically configures the ClusterControl node with respective package repository.

YUM repository
++++++++++++++

1. Manually import the Severalnines repository public key into your RPM keyring:

.. code-block:: bash

	$ rpm --import http://repo.severalnines.com/severalnines-repos.asc

2 (a). You can download the repository definition from `Severalnines download page <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

  $ wget http://www.severalnines.com/downloads/cmon/s9s-repo.repo -P /etc/yum.repos.d/

For nightly build (development release):

.. code-block:: bash

  $ wget http://www.severalnines.com/downloads/cmon/s9s-repo-nightly.repo -P /etc/yum.repos.d/

2 (b). Or, you can create the repository definition manually:

.. code-block:: bash

	$ cat - > /etc/yum.repos.d/s9s-repo.repo <<REPOEND
	[s9s-repo]
	name = Severalnines Release Repository
	baseurl = http://repo.severalnines.com/rpm/os/x86_64
	enabled = 1
	gpgkey = http://repo.severalnines.com/severalnines-repos.asc
	gpgcheck = 1
	REPOEND

For nightly build (development release):

.. code-block:: bash

  $ cat - > /etc/yum.repos.d/s9s-repo-nightly.repo <<REPOEND
  [s9s-repo-nightly]
  name = Severalnines Nightly Repository
  baseurl = http://repo.severalnines.com/repos-nightly/rpm/os/x86_64
  enabled = 1
  gpgkey = http://repo.severalnines.com/repos-nightly/severalnines-repos.asc
  gpgcheck = 1
  REPOEND

3. Look for ClusterControl packages:

.. code-block:: bash

	$ yum search clustercontrol

APT repository
++++++++++++++

1. Manually add Severalnines repository public key into your APT keyring:

.. code-block:: bash

	$ wget http://repo.severalnines.com/severalnines-repos.asc -O- | sudo apt-key add -

2 (a). You can download the repository definition from `Severalnines download page <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

  $ sudo wget http://www.severalnines.com/downloads/cmon/s9s-repo.list -P /etc/apt/sources.list.d/

For nightly build (development release):

.. code-block:: bash

  $ sudo wget http://www.severalnines.com/downloads/cmon/s9s-repo-nightly.list -P /etc/apt/sources.list.d/

2 (b). Or, add the Severalnines APT source list manually:

.. code-block:: bash

  $ echo 'deb [arch=amd64] http://repo.severalnines.com/deb ubuntu main' | sudo tee /etc/apt/sources.list.d/s9s-repo.list

For nightly build (development release):

.. code-block:: bash

  $ echo 'deb [arch=amd64] http://repo.severalnines.com/repos-nightly/deb ubuntu main' | sudo tee /etc/apt/sources.list.d/s9s-repo-nightly.list

3. Update package list:

.. code-block:: bash

	$ sudo apt-get update

4. Look for ClusterControl packages:

.. code-block:: bash

	$ sudo apt-cache search clustercontrol


Automatic Installation
----------------------

We have a bunch of scripts and tools to automate and simplify the installation process of ClusterControl in various environments:

* Installation Script (install-cc)
* Puppet module
* Chef cookbooks
* Ansible role
* Docker image


Installer Script (install-cc)
++++++++++++++++++++++++++++++

Installer script is the recommended way to install ClusterControl. The script must be downloaded and executed on ClusterControl node, which performs all necessary steps to install and configure ClusterControl's packages and dependencies on that particular host. It also supports offline installation with ``NO_INET=1`` variable exported, however you need to have mirrored repository enabled or MySQL and Apache installed and running on that host beforehand. See `Offline Installation`_ for details. The script assumes that the host can install all dependencies via operating system repository.

On ClusterControl server, run the following commands:

.. code-block:: bash

  $ wget http://www.severalnines.com/downloads/cmon/install-cc
  $ chmod +x install-cc
  $ sudo ./install-cc   # omit sudo if you run as root

If you have multiple network interface cards, assign primary IP address for ``HOST`` variable as per example below:

.. code-block:: bash

  $ HOST=192.168.1.10 ./install-cc # as root or sudo user

By default, the script will allocate 50% of the host's RAM to InnoDB buffer pool. You can change this by assigning a value in MB for ``INNODB_BUFFER_POOL_SIZE`` variable as per example below:

.. code-block:: bash

	$ INNODB_BUFFER_POOL_SIZE=512 ./install-cc # as root or sudo user

.. Note:: ClusterControl relies on a MySQL server as a data repository for the clusters it manages and an Apache server for the User Interface. The installation script will always install an Apache server on the host. An existing MySQL server can be used or a new MySQL server install is configured for minimum system requirements. If you have a larger server please make the necessary changes to the my.cnf file and restart the MySQL server after the installation.

Basically, the installation script will attempt to automate the following tasks:

1. Install and configure a MySQL server (used by ClusterControl to store monitoring data).
2. Install and configure the ClusterControl controller package via package manager.
3. Install ClusterControl dependencies via package manager.
4. Configure Apache and SSL.
5. Configure ClusterControl API URL and token.
6. Configure ClusterControl Controller with minimal configuration options.
7. Enable the CMON service on boot and start it up.

After the installation completes, open your web browser to :samp:`http://{ClusterControl_host}/clustercontrol` and create the default admin user by specifying a valid email address and password in the welcome page.

Puppet Module
++++++++++++++

If you are automating your infrastructure using :term:`Puppet`, we have created a module for this purpose and it is available at `Puppet Forge <https://forge.puppetlabs.com/severalnines/clustercontrol>`_. Installing the module is as easy as:

.. code-block:: bash

	$ puppet module install severalnines-clustercontrol

Requirements
````````````

If you haven’t changed the default ``$modulepath``, this module will be installed under ``/etc/puppet/modules/clustercontrol`` on your Puppet master host. This module requires the following criteria to be met:

* The node for ClusterControl must be a clean/dedicated host.
* ClusterControl node must have an internet connection during the deployment. After the deployment completes, ClusterControl does not need internet access to work.


Pre-installation
``````````````````

ClusterControl requires proper SSH key configuration and a ClusterControl API token. Use the helper script located at ``$modulepath/clustercontrol/files/s9s_helper.sh`` to generate them.

Generate SSH key to be used by ClusterControl to manage your database nodes. Run the following command in Puppet master:

.. code-block:: bash

	$ bash /etc/puppet/modules/clustercontrol/files/s9s_helper.sh --generate-key

Then, generate an API token:

.. code-block:: bash

	$ bash /etc/puppet/modules/clustercontrol/files/s9s_helper.sh --generate-token
	b7e515255db703c659677a66c4a17952515dbaf5

.. Attention:: These two steps are mandatory and just need to run once (unless if you want to intentionally regenerate them). The first command will generate a RSA key (if not exists) to be used by the module and the key must exist in the Puppet master module's directory before the deployment begins.

Installation
````````````

Specify the generated token in the node definition similar to the example below.

Example hosts:

.. code-block:: bash

  clustercontrol.local    192.168.1.10
  galera1.local           192.168.1.11
  galera2.local           192.168.1.12
  galera3.local           192.168.1.13

Example node definition:

.. code-block:: ruby

  # ClusterControl host
  node "clustercontrol.local" {
    class { 'clustercontrol':
      is_controller => true,
	  ssh_user => root,
      api_token => 'b7e515255db703c659677a66c4a17952515dbaf5'
    }
  }

After the deployment completes, open ClusterControl UI at :samp:`https://{ClusterControl_host}/clustercontrol` and create a default admin login. You can now start to add existing database node/cluster, or deploy a new one. Ensure that passwordless SSH is configured properly from ClusterControl node to all database nodes beforehand.

To setup passwordless SSH on target database nodes, you can use following definition:

.. code-block:: ruby

  # Monitored DB hosts
  node "galera1.local", "galera2.local", "galera3.local" {
    class {'clustercontrol':
      is_controller => false,
	  ssh_user => root,
      mysql_root_password => 'r00tpassword',
      clustercontrol_host => '192.168.1.10'
    }
  }


You can either instruct the agent to pull the configuration from the Puppet master and apply it immediately:

.. code-block:: bash

	$ puppet agent -t

Or, wait for the Puppet agent service to apply the catalog automatically (depending on the ``runinterval`` value, default is 30 minutes). Once completed, open the ClusterControl UI page at :samp:`http://{ClusterControl_host}/clustercontrol` and create the default admin user and password.

For more example on deployments using Puppet, please refer to `Puppet Module for ClusterControl - Adding Management and Monitoring to your Existing Database Clusters <https://severalnines.com/blog/puppet-module-clustercontrol-adding-management-and-monitoring-your-existing-database-clusters>`_. For more info on configuration options, please refer to `ClusterControl Puppet Module <https://forge.puppetlabs.com/severalnines/clustercontrol>`_ page.

Chef Cookbooks
++++++++++++++

If you are automating your infrastructure using :term:`Chef`, we have created a cookbook for this purpose and it is available at `Chef Supermarket <https://supermarket.chef.io/cookbooks/clustercontrol>`_. Getting the cookbook is as easy as:

.. code-block:: bash

	$ knife cookbook site download clustercontrol

Requirements
``````````````

This cookbook requires the following criterias to be met:

* The node for ClusterControl must be a clean/dedicated host.
* ClusterControl node must be running on 64bit OS platform and together with the same OS distribution with the monitored DB hosts. Mixing Debian with Ubuntu and CentOS with Red Hat is acceptable.
* ClusterControl node must have an internet connection during the deployment. After the deployment, ClusterControl does not need internet access.
* Make sure your database cluster is up and running before performing this deployment.

Data items are used by the ClusterControl controller recipe to configure SSH public key on database hosts, grants cmon database user and setting up CMON configuration file. We provide a helper script located under ``clustercontrol/files/default/s9s_helper.sh``. Please run this script prior to the deployment.

Answer all the questions and at the end of the wizard, it will generate a data bag file called ``config.json`` and a set of commands that you can use to create and upload the data bag. If you run the script for the first time, it will ask to re-upload the cookbook since it contains a newly generated SSH key: 

.. code-block:: bash

	$ knife cookbook upload clustercontrol
	

Chef Workstation
````````````````

This section shows example ClusterControl installation with Chef and requires you to use :term:`knife`. Please ensure it has been configured correctly and is able to communicate with the Chef Server before you proceed with the following steps. The steps in this section should be performed on the Chef Workstation node.

1. Get the ClusterControl cookbook using knife:

.. code-block:: bash

	$ cd ~/chef-repo/cookbooks
	$ knife cookbook site download clustercontrol
	$ tar -xzf clustercontrol-*
	$ rm -Rf *.tar.gz

2. Run ``s9s_helper.sh`` to auto generate SSH key file, ClusterControl API token and data bag items:

.. code-block:: bash

  $ cd ~/chef-repo/cookbooks/clustercontrol/files/default
  $ ./s9s_helper.sh
	==============================================
	Helper script for ClusterControl Chef cookbook
	==============================================
	
	ClusterControl will install a MySQL server and setup the MySQL root user.
	Enter the password for MySQL root user [password] : R00tP4ssw0rd
	
	ClusterControl will create a MySQL user called 'cmon' for automation tasks.
	Enter the password for user cmon [cmon] : Bj990sPkj
	
	Generating config.json..
	{
	    "id" : "config",
	    "mysql_root_password" : "R00tP4ssw0rd",
	    "cmon_password" : "Bj990sPkj",
	    "clustercontrol_api_token" : "662894d3e854ed779babd895a82dc0f8eed86ccc"
	}
	
	Data bag file generated at /root/cookbooks/clustercontrol/files/default/config.json
	To upload the data bag, you can use following command:
	$ knife data bag create clustercontrol
	$ knife data bag from file clustercontrol /root/cookbooks/clustercontrol/files/default/config.json
	
	** We highly recommend you to use encrypted data bag since it contains confidential information **

3. As per instructions above, on Chef Workstation host, do:

.. code-block:: bash

	$ knife data bag create clustercontrol
	Created data_bag[clustercontrol]

	$ knife data bag from file clustercontrol /home/ubuntu/chef-repo/cookbooks/clustercontrol/files/default/config.json
	Updated data_bag_item[clustercontrol::config]
	
	$ knife cookbook upload clustercontrol
	Uploading clustercontrol [0.1.6]
	Uploaded 1 cookbook.

4. Create a role, ``cc_controller``:

.. code-block:: bash

	$ cat cc_controller.rb 
	name "cc_controller"
	description "ClusterControl Controller"
	run_list ["recipe[clustercontrol]"]

5. Add the defined roles into Chef Server:

.. code-block:: bash

	$ knife role from file cc_controller.rb
	Updated Role cc_controller!

6. Assign the roles to the relevant nodes:

.. code-block:: bash

	$ knife node run_list add clustercontrol.domain.com "role[cc_controller]"


Chef Client
````````````

Let :term:`chef-client` run on each Chef client node and apply the cookbook:

.. code-block:: bash

	$ sudo chef-client

Once completed, open the ClusterControl UI at :samp:`http://{ClusterControl_host}/clustercontrol` and create the default admin user and password. 

For more example on deployments using Chef, please refer to `Chef Cookbooks for ClusterControl - Management and Monitoring for your Database Clusters <http://www.severalnines.com/blog/chef-cookbooks-clustercontrol-management-and-monitoring-your-database-clusters>`_. For more info on the configuration options, please refer to `ClusterControl Chef Cookbooks <https://supermarket.chef.io/cookbooks/clustercontrol>`_ page.

Ansible Role
++++++++++++++

If you are automating your infrastructure using :term:`Ansible`, we have created a role for this purpose and it is available at `Ansible Galaxy <https://galaxy.ansible.com/severalnines/clustercontrol>`_. This role also supports deploy a new cluster and import existing cluster into ClusterControl automatically, as shown under `Example Playbook`_.

.. seealso:: `ClusterControl Ansible Github <https://github.com/severalnines/ansible-clustercontrol>`_ page.

Getting the role is as easy as:

.. code-block:: bash

	$ ansible-galaxy install severalnines.clustercontrol

Usage
``````

1. Get the ClusterControl Ansible role from Ansible Galaxy or Github.

Ansible Galaxy:

.. code-block:: bash

	$ ansible-galaxy install severalnines.clustercontrol

Or through Github:

.. code-block:: bash

	$ git clone https://github.com/severalnines/ansible-clustercontrol
	$ cp -rf ansible-clustercontrol /etc/ansible/roles/severalnines.clustercontrol

2. Create a playbook. See `Example Playbook`_ section.

3. Run the playbook.

.. code-block:: bash

	$ ansible-playbook cc.playbook

4) Once ClusterControl is installed, go to :samp:`http://{ClusterControl_host}/clustercontrol` and create the default admin user/password.

5) On ClusterControl node, setup passwordless SSH key to all target DB nodes. For example, if ClusterControl node is 192.168.0.10 and DB nodes are 192.168.0.11, 192.168.0.12 and 192.168.0.13:

.. code-block:: bash

	$ ssh-copy-id 192.168.0.11 # DB1
	$ ssh-copy-id 192.168.0.12 # DB2
	$ ssh-copy-id 192.168.0.13 # DB3

.. Note:: Enter the password to complete the passwordless SSH setup.

6) Start to deploy a new database cluster or add an existing one.

Example Playbook
``````````````````

The simplest playbook would be:

.. code-block:: yaml

    - hosts: clustercontrol-server
      roles:
        - { role: severalnines.clustercontrol }

If you would like to specify custom configuration values as explained above, create a file called ``vars/main.yml`` and include it inside the playbook:

.. code-block:: yaml

    - hosts: 192.168.10.15
      vars:
        - vars/main.yml
        roles:
        - { role: severalnines.clustercontrol, tags: controller }

Inside ``vars/main.yml``:

.. code-block:: yaml

	controller: true
	mysql_root_username: admin
	mysql_root_password: super-user-password
	cmon_mysql_password: super-cmon-password
	cmon_mysql_port: 3307

If you are running as another user, ensure the user has ability to escalate as super user via sudo. Example playbook for Ubuntu 12.04 with sudo password enabled:

.. code-block:: yaml

    - hosts: ubuntu@192.168.10.100
      become: yes
      become_user: root
      roles:
        - { role: severalnines.clustercontrol, tags: controller }

Then, execute the command with ``--ask-become-pass`` flag, for example:

.. code-block:: bash

    $ ansible-playbook cc.playbook --ask-become-pass

Install ClusterControl with automatic deployment

The role also supports automatic database deployment by leveraging the CMON RPC interface. This will minimize the deployment time to get your database cluster up and running. Example playbook for automatic deployment in AWS EC2 can be found here.

Consider the following inside /etc/ansible/hosts:

.. code-block:: bash

	[clustercontrol]
	192.168.55.100
	
	[galera]
	192.168.55.171
	192.168.55.172
	192.168.55.173
	
	[mysql-replication]
	192.168.55.204
	192.168.55.205

The following playbook will install ClusterControl on 192.168.55.100, setup passwordless SSH on Galera and MySQL replication nodes, then post create/add job into ClusterControl for the deployment:

.. code-block:: yaml

  - hosts: clustercontrol
    roles:
      - { role: severalnines.clustercontrol, tags: controller }
	
  - hosts:
      - mysql-replication
      - galera
    roles:
      - { role: severalnines.clustercontrol, tags: dbnodes }
    vars:
      clustercontrol_ip_address: 192.168.55.100
      ssh_user: root
	
  - hosts: clustercontrol
    roles:
      - { role: severalnines.clustercontrol, tags: deploy-database }
    vars:
      cc_cluster:
        # create new mysql replication. first node is the master
        - deployment: true
          operation: "create"
          cluster_type: "replication"
          mysql_hostnames:
            - '192.168.55.204'
            - '192.168.55.205'
          mysql_cnf_template: "my.cnf.repl57"
          mysql_datadir: "/var/lib/mysql"
          mysql_password: "password"
          mysql_port: 3306
          mysql_version: "5.7"
          ssh_keyfile: "/root/.ssh/id_rsa"
          ssh_port: "22"
          ssh_user: "root"
          sudo_password: ""
          type: "mysql"
          vendor: "oracle"
      # add existing galera.
        - deployment: true
          operation: "add"
          cluster_type: "galera"
          mysql_password: "password"
          mysql_hostnames:
            - '192.168.55.171'
            - '192.168.55.172'
            - '192.168.55.173'
          ssh_keyfile: "/root/.ssh/id_rsa"
          ssh_port: 22
          ssh_user: root
          vendor: percona
          sudo_password: ""
          galera_version: "3.x"
          enable_node_autorecovery: true
          enable_cluster_autorecovery: true
      # minimal create new galera
        - deployment: true
          operation: "create"
          cluster_type: "galera"
          mysql_cnf_template: "my.cnf.galera"
          mysql_datadir: "/var/lib/mysql"
          mysql_hostnames:
            - '192.168.55.191'
            - '192.168.55.192'
            - '192.168.55.193'
          mysql_password: "password"
          mysql_port: 3306
          mysql_version: "5.6"
          ssh_keyfile: "/root/.ssh/id_rsa"
          ssh_user: "root"
          sudo_password: ""
          vendor: "percona"

Take note the following tags in the role lines:

* no tag (default) - Install ClusterControl
* dbnodes - For all managed nodes to setup passwordless SSH
* deploy-database - To deploy database after ClusterControl is installed

Variables are mostly similar to keys in JSON job command created in ClusterControl's Cluster Job. If a key:value is not specified, the default value is used. For more details, check out `ClusterControl Ansible Github <https://github.com/severalnines/ansible-clustercontrol>`_ page.

Docker Image
++++++++++++++

The :term:`Docker` image comes with ClusterControl installed and configured with all of its components, so you can immediately use it to manage and monitor your existing databases. 

Having a Docker image for ClusterControl at the moment is convenient in terms of how quickly it is to get it up and running and it's 100% reproducible. Docker users can now start testing ClusterControl, since we have the Docker image that everyone can pull down from Docker Hub and then launch the tool.

It is a start and our plan is to add better integration with the Docker API in future releases in order to transparently manage Docker containers/images within ClusterControl, e.g., to launch/manage and deploy database clusters using Docker images.

Build the image
````````````````

The Dockerfiles are available from `our Github repository <https://github.com/severalnines/docker>`_. You can build it manually by cloning the repository:

.. code-block:: bash

	$ git clone https://github.com/severalnines/docker
	$ cd docker/
	$ docker build -t severalnines/clustercontrol .

Running container
``````````````````

Please refer to the `Docker Hub page <https://registry.hub.docker.com/u/severalnines/clustercontrol/>`_ for the latest instructions. Use the ``docker pull`` command to download the image:

.. code-block:: bash

	$ docker pull severalnines/clustercontrol

Use the following command to run:

.. code-block:: bash

	$ docker run -d --name clustercontrol -p 5000:80 severalnines/clustercontrol

Once started, ClusterControl is accessible at :samp:`http://{Docker_host}:5000/clustercontrol`. You should see the welcome page to create a default admin user. Use your email address and specify passwords for that user. By default MySQL users root and cmon will be using 'password' and 'cmon' as default password respectively. You can override this value with -e flag, as example below:

.. code-block:: bash

	$ docker run -d --name clustercontrol -e CMON_PASSWORD=MyCM0n22 -e MYSQL_ROOT_PASSWORD=SuP3rMan -p 5000:80 severalnines/clustercontrol
	
Optionally, you can map the HTTPS port using -p by appending the forwarding as below:

.. code-block:: bash

	$ docker run -d --name clustercontrol -p 5000:80 -p 5443:443 severalnines/clustercontrol

Verify the container is running by using the ps command:

.. code-block:: bash

	$ docker ps

For more example on deployments with Docker images, please refer to `ClusterControl on Docker <http://www.severalnines.com/blog/clustercontrol-docker>`_ and `the Docker image Github page <https://github.com/severalnines/docker/>`_. For more info on the configuration options, please refer to `ClusterControl's Docker Hub <https://registry.hub.docker.com/u/severalnines/clustercontrol/>`_ page.

Manual Installation
-------------------

If you want to have more control on the installation process, you may perform manual installation.

.. note:: Installing and uninstalling ClusterControl should not bring any downtime to the managed database cluster.

The main installation steps are:

1. Install Severalnines yum/apt repository.
2. Install ClusterControl packages.
3. Execute the post-installation script (recommended) or perform manual installation.

.. note:: On step #3, performing installation using the post-installation script is highly recommended. Manual installation instructions are provided in this guide for advanced users and reference purpose.

ClusterControl requires a number of packages to be installed and configured, as described in the following list:

* *clustercontrol* - ClusterControl web user interface.
* *clustercontrol-cmonapi* - ClusterControl REST API.
* *clustercontrol-controller* - ClusterControl CMON controller.
* *clustercontrol-notifications* - ClusterControl notification module, if you would like to integrate with third-party tools like PagerDuty and Slack.
* *clustercontrol-ssh* - ClusterControl web-based SSH module, if you would like to access the host via SSH directly from ClusterControl UI.
* *clustercontrol-cloud* - ClusterControl cloud module, if you would like to manage your cloud instances directly from ClusterControl UI.
* *clustercontrol-clud* - ClusterControl cloud file manager module, if you would like to upload and download backups from cloud storage. It requires ``clustercontrol-cloud``.

Steps described in the following sections should be performed on ClusterControl node unless specified otherwise.

Requirements
++++++++++++

Make sure the following is ready prior to this installation:

* Verify that sudo is working properly if you are using a non-root user.
* ClusterControl node must able to connect to all database nodes.
* Passwordless SSH from ClusterControl node to all nodes (including the ClusterControl node itself) has been configured correctly.
* You must have internet connection on ClusterControl node during the installation process. Otherwise, see `Offline Installation`_.

Redhat/CentOS
+++++++++++++

1. Setup `Severalnines YUM Repository <installation.html#yum-repository>`_.

2. Disable SElinux and open required ports (or stop iptables):

.. code-block:: bash

	$ sed -i 's|SELINUX=enforcing|SELINUX=disabled|g' /etc/selinux/config
	$ setenforce 0
	$ service iptables stop # RedHat/CentOS 6
	$ systemctl stop firewalld # RedHat/CentOS 7

3. Install required packages via package manager:

.. code-block:: bash

	$ yum -y install curl mailx cronie nc bind-utils mysql mariadb-server httpd mod_ssl php php-pdo php-mysql # RHEL/CentOS 7
	$ yum -y install curl mailx cronie nc bind-utils mysql mysql-server httpd mod_ssl php php-pdo php-mysql # RHEL/CentOS 6

4. Install ClusterControl packages:

.. code-block:: bash

	$ yum -y install clustercontrol clustercontrol-cmonapi clustercontrol-controller clustercontrol-ssh clustercontrol-notifications clustercontrol-cloud clustercontrol-clud

5. Start MySQL server (MariaDB for Redhat/CentOS 7), enable it on boot and set a MySQL root password:

.. code-block:: bash

	$ service mysqld start # Redhat/CentOS 6
	$ systemctl start mariadb.service # Redhat/CentOS 7
	$ chkconfig mysqld on # Redhat/CentOS 6
	$ systemctl enable mariadb.service # Redhat/CentOS 7
	$ mysqladmin -uroot password 'themysqlrootpassword'
	
6. Create two databases called cmon and dcps and grant the cmon user:

.. code-block:: bash

	$ mysql -uroot -p -e 'DROP SCHEMA IF EXISTS cmon; CREATE SCHEMA cmon'
	$ mysql -uroot -p -e 'DROP SCHEMA IF EXISTS dcps; CREATE SCHEMA dcps'
	$ mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"localhost" IDENTIFIED BY "{cmonpassword}" WITH GRANT OPTION'
	$ mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"127.0.0.1" IDENTIFIED BY "{cmonpassword}" WITH GRANT OPTION'
	$ mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"{ClusterControl primary IP address}" IDENTIFIED BY "{cmonpassword}" WITH GRANT OPTION'
	$ mysql -uroot -p -e 'FLUSH PRIVILEGES'

.. note:: Replace ``{ClusterControl primary IP address}`` and ``{cmonpassword}`` with respective values.

7. Import cmon and dcps schema structure and data:

.. code-block:: bash

	$ mysql -uroot -p cmon < /usr/share/cmon/cmon_db.sql
	$ mysql -uroot -p cmon < /usr/share/cmon/cmon_data.sql
	$ mysql -uroot -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql
	
8. Generate a ClusterControl key to be used by ``CMON_TOKEN``, ``RPC_TOKEN`` and ``rpc_key``:

.. code-block:: bash

	$ python -c 'import uuid; print uuid.uuid4()' | sha1sum | cut -f1 -d' '
	6856d96a19d049aa8a7f4a5ba57a34740b3faf57

And create the ClusterControl Controller (cmon) configuration file at ``/etc/cmon.cnf`` with the following configuration options:

.. code-block:: bash

	mysql_port=3306
	mysql_hostname=127.0.0.1
	mysql_password={cmonpassword}
	hostname={ClusterControl primary IP address}
	rpc_key={ClusterControl API key as generated above}

Example as follows:

.. code-block:: bash

	$ cat /etc/cmon.cnf
	mysql_port=3306
	mysql_hostname=127.0.0.1
	mysql_password=cmon
	hostname=192.168.1.85
	rpc_key=6856d96a19d049aa8a7f4a5ba57a34740b3faf57

.. Attention:: The value of ``hostname`` must be either a valid FQDN or IP address of ClusterControl node. If the host has multiple IP addresses, use the one that granted in step #6.

9. ClusterControl's event and cloud modules require ``/etc/default/cmon`` for service definition. Create the file and add the following lines:

.. code-block:: bash

	EVENTS_CLIENT="http://127.0.0.1:9510"
	CLOUD_SERVICE="http://127.0.0.1:9518"

10. Configure Apache to use ``AllowOverride=All`` and set up SSL key and certificate:

.. code-block:: bash

	$ cp -f /var/www/html/cmonapi/ssl/server.crt /etc/pki/tls/certs/s9server.crt
	$ cp -f /var/www/html/cmonapi/ssl/server.key /etc/pki/tls/private/s9server.key
	$ rm -rf /var/www/html/cmonapi/ssl
	$ sed -i 's|AllowOverride None|AllowOverride All|g' /etc/httpd/conf/httpd.conf
	$ sed -i 's|AllowOverride None|AllowOverride All|g' /etc/httpd/conf.d/ssl.conf
	$ sed -i 's|^SSLCertificateFile.*|SSLCertificateFile /etc/pki/tls/certs/s9server.crt|g' /etc/httpd/conf.d/ssl.conf
	$ sed -i 's|^SSLCertificateKeyFile.*|SSLCertificateKeyFile /etc/pki/tls/private/s9server.key|g' /etc/httpd/conf.d/ssl.conf

11. Copy the ClusterControl UI and CMONAPI default files and assign correct permission:

.. code-block:: bash

	$ cp -f /var/www/html/clustercontrol/bootstrap.php.default /var/www/html/clustercontrol/bootstrap.php
	$ cp -f /var/www/html/cmonapi/config/bootstrap.php.default /var/www/html/cmonapi/config/bootstrap.php
	$ cp -f /var/www/html/cmonapi/config/database.php.default /var/www/html/cmonapi/config/database.php
	$ chmod 644 /var/www/html/clustercontrol/bootstrap.php
	$ chmod 644 /var/www/html/cmonapi/config/bootstrap.php
	$ chmod 644 /var/www/html/cmonapi/config/database.php

12. Assign correct ownership and permission:

.. code-block:: bash

	$ chmod -R 777 /var/www/html/clustercontrol/app/tmp
	$ chmod -R 777 /var/www/html/clustercontrol/app/upload
	$ chown -Rf apache.apache /var/www/html/cmonapi/
	$ chown -Rf apache.apache /var/www/html/clustercontrol/

13. Use the generated value from step #8 and specify it in ``/var/www/html/clustercontrol/bootstrap.php`` under the ``RPC_TOKEN`` constant and configure MySQL credentials for the ClusterControl UI by updating the ``DB_PASS`` and ``DB_PORT`` constants with the cmon user password and MySQL port for ``dcps`` database:

.. code-block:: php

	define('DB_PASS', '{cmonpassword}');
	define('DB_PORT', '3306');
	define('RPC_TOKEN', '{Generated ClusterControl API token}');

.. Note:: Replace ``{cmonpassword}`` and ``{Generated ClusterControl API token}`` with appropriate values.

14. Use the generated value from step #8 and specify it in ``/var/www/html/cmonapi/config/bootstrap.php`` under the ``CMON_TOKEN`` constant. It is expected for the ``CMON_TOKEN``, ``RPC_TOKEN`` (step #13) and ``rpc_key`` (in cmon.cnf) are holding the same value. Also, update the ``CC_URL`` value to be equivalent to ClusterControl URL in your environment:

.. code-block:: php

	define('CMON_TOKEN', '{Generated ClusterControl API token}');
	define('CC_URL', 'https://{ClusterControl_host}/clustercontrol');

.. Note:: Replace ``{Generated ClusterControl API token}`` and ``{ClusterControl_host}`` with appropriate values.

15. Configure MySQL credential for ClusterControl CMONAPI at ``/var/www/html/cmonapi/config/database.php``. In most cases, you just need to update the ``DB_PASS`` and ``DB_PORT`` constants with the cmon user password and MySQL port for ``cmon`` database:

.. code-block:: bash

	define('DB_PASS', '{cmonpasword}');
	define('DB_PORT', '3306');

.. Note:: Replace ``{cmonpassword}`` with a relevant value.

16. Enable ClusterControl and Apache daemons on boot and start them:

For sysvinit:

.. code-block:: bash

	$ chkconfig --levels 235 cmon on
	$ chkconfig --levels 235 cmon-ssh on
	$ chkconfig --levels 235 cmon-events on
	$ chkconfig --levels 235 cmon-cloud on
	$ chkconfig --levels 235 httpd on
	$ service cmon start
	$ service cmon-ssh start
	$ service cmon-events start
	$ service cmon-cloud start
	$ service httpd start

For systemd:

.. code-block:: bash

	$ systemctl enable cmon cmon-ssh cmon-events cmon-cloud httpd
	$ systemctl start cmon cmon-ssh cmon-events cmon-cloud httpd

17. Generate a SSH key to be used by ClusterControl when connecting to all managed hosts. In this example, we are using 'root' user to connect to the managed hosts. To generate a SSH key for the root user, do:

.. code-block:: bash

	$ whoami
	root
	$ ssh-keygen -t rsa # Press enter for all prompts

.. Note:: If you are running as sudoer, the default SSH key will be located under ``/home/$USER/.ssh/id_rsa``. See `Operating System User <requirements.html#operating-system-user>`_.


18. Open ClusterControl UI at :samp:`http://{ClusterControl_host}/clustercontrol` and create the default admin password by providing a valid email address and password. You will be redirected to ClusterControl default page. Go to *Global Settings* > *Cluster Registrations* and enter the generated ClusterControl API token (step #14) and CMONAPI URL :samp:`http://127.0.0.1/cmonapi`, similar to example below:

.. image:: img/cc_register_token.png
   :alt: Register ClusterControl API token
   :align: center

You will then be redirected to the ClusterControl landing page. 

19. Before importing a database server/cluster into ClusterControl, set up passwordless SSH from ClusterControl host to the database host(s). Use the following command to copy the SSH key to the target hosts:

.. code-block:: bash

	$ ssh-copy-id -i ~/.ssh/id_rsa {SSH user}@{IP address of the target node}

.. Note:: Replace ``{SSH user}`` and ``{IP address of the target node}`` with appropriate values. Repeat the command for all target hosts.

The installation is complete and you can start to import existing or deploy a new database cluster. Please review the `User Guide <user-guide/>`_ for details.

Debian/Ubuntu
+++++++++++++

The following steps should be performed on the ClusterControl node, unless specified otherwise. Ensure you have Severalnines repository and ClusterControl UI installed. Please refer to Severalnines Repository section for details. Omit sudo if you are installing as root user. Take note that for Ubuntu 12.04/Debian 7 and earlier, replace all occurrences of ``/var/www/html`` with ``/var/www`` in the following instructions.

1. Setup `Severalnines APT Repository <installation.html#apt-repository>`_.

2. If you have AppArmor running, disable it and open the required ports (or stop iptables):

.. code-block:: bash

	$ sudo /etc/init.d/apparmor stop
	$ sudo /etc/init.d/apparmor teardown
	$ sudo update-rc.d -f apparmor remove
	$ sudo service iptables stop

3. Install ClusterControl dependencies:

.. code-block:: bash

	$ sudo apt-get update
	$ sudo apt-get install -y curl mailutils dnsutils mysql-client mysql-server

4. Install the ClusterControl controller package:

.. code-block:: bash

	$ sudo apt-get install -y clustercontrol-controller clustercontrol clustercontrol-cmonapi clustercontrol-ssh clustercontrol-notifications clustercontrol-cloud clustercontrol-clud

5. Comment the following line inside ``/etc/mysql/my.cnf`` to allow MySQL to listen on all interfaces:

.. code-block:: bash

	#bind-address=127.0.0.1

Restart the MySQL service to apply the change:

.. code-block:: bash

	$ service mysql restart

6. Create two databases called cmon and dcps and grant user cmon:

.. code-block:: bash

	$ mysql -uroot -p -e 'DROP SCHEMA IF EXISTS cmon; CREATE SCHEMA cmon'
	$ mysql -uroot -p -e 'DROP SCHEMA IF EXISTS dcps; CREATE SCHEMA dcps'
	$ mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"localhost" IDENTIFIED BY "{cmonpassword}" WITH GRANT OPTION'
	$ mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"127.0.0.1" IDENTIFIED BY "{cmonpassword}" WITH GRANT OPTION'
	$ mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"{ClusterControl primary IP address}" IDENTIFIED BY "{cmonpassword}" WITH GRANT OPTION'
	$ mysql -uroot -p -e 'FLUSH PRIVILEGES'

.. Note:: Replace ``{ClusterControl primary IP address}`` and ``{cmonpassword}`` with respective values.

7. Import cmon and dcps schema:

.. code-block:: bash

	$ mysql -uroot -p cmon < /usr/share/cmon/cmon_db.sql
	$ mysql -uroot -p cmon < /usr/share/cmon/cmon_data.sql
	$ mysql -uroot -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql

8. Generate a ClusterControl key to be used by ``CMON_TOKEN``, ``RPC_TOKEN`` and ``rpc_key``:

.. code-block:: bash

	$ python -c 'import uuid; print uuid.uuid4()' | sha1sum | cut -f1 -d' '
	6856d96a19d049aa8a7f4a5ba57a34740b3faf57

And create the ClusterControl Controller (cmon) configuration file at ``/etc/cmon.cnf`` with the following configuration options:

.. code-block:: bash

	mysql_port=3306
	mysql_hostname=127.0.0.1
	mysql_password={cmonpassword}
	hostname={ClusterControl primary IP address}
	rpc_key={ClusterControl API key as generated above}

Example as follows:

.. code-block:: bash

	$ cat /etc/cmon.cnf
	mysql_port=3306
	mysql_hostname=127.0.0.1
	mysql_password=cmon
	hostname=192.168.1.85
	rpc_key=6856d96a19d049aa8a7f4a5ba57a34740b3faf57

.. Attention:: The value of ``hostname`` must be either a valid FQDN or IP address of ClusterControl node. If the host has multiple IP addresses, use the one that granted in step #6.

9. ClusterControl's event and cloud modules require ``/etc/default/cmon`` for service definition. Create the file and add the following lines:

.. code-block:: bash

	EVENTS_CLIENT="http://127.0.0.1:9510"
	CLOUD_SERVICE="http://127.0.0.1:9518"

10. The default web root for Clustercontrol UI and CMONAPI on Debian-based distribution is ``/var/www``. For Apache 2.4 and later (Ubuntu 14.04/Debian 8 and later), the default document root is ``/var/www/html``. Create symbolic links for both components:

.. code-block:: bash

	$ ln -sfn /var/www/clustercontrol /var/www/html
	$ ln -sfn /var/www/cmonapi /var/www/html

11. Configure Apache ``AllowOverride`` and setting up SSL:

.. code-block:: bash

	$ cp -f /var/www/cmonapi/ssl/server.crt /etc/ssl/certs/s9server.crt
	$ cp -f /var/www/cmonapi/ssl/server.key /etc/ssl/certs/s9server.key
	$ rm -rf /var/www/cmonapi/ssl
	$ sed -i 's|AllowOverride None|AllowOverride All|g' /etc/apache2/sites-available/default
	$ sed -i 's|AllowOverride None|AllowOverride All|g' /etc/apache2/sites-available/default-ssl
	$ sed -i 's|^[ \t]*SSLCertificateFile.*|SSLCertificateFile /etc/ssl/certs/s9server.crt|g' /etc/apache2/sites-available/default-ssl
	$ sed -i 's|^[ \t]*SSLCertificateKeyFile.*|SSLCertificateKeyFile /etc/ssl/certs/s9server.key|g' /etc/apache2/sites-available/default-ssl

For Ubuntu 14.04, it runs on Apache 2.4 which has a slightly different configuration than above:

.. code-block:: bash

	$ cp -f /var/www/cmonapi/ssl/server.crt /etc/ssl/certs/s9server.crt
	$ cp -f /var/www/cmonapi/ssl/server.key /etc/ssl/certs/s9server.key
	$ rm -rf /var/www/cmonapi/ssl
	$ cp -f /var/www/clustercontrol/app/tools/apache2/s9s.conf /etc/apache2/sites-available/
	$ cp -f /var/www/clustercontrol/app/tools/apache2/s9s-ssl.conf /etc/apache2/sites-available/
	$ rm -f /etc/apache2/sites-enabled/000-default.conf
	$ rm -f /etc/apache2/sites-enabled/default-ssl.conf
	$ rm -f /etc/apache2/sites-enabled/001-default-ssl.conf
	$ ln -sfn /etc/apache2/sites-available/s9s.conf /etc/apache2/sites-enabled/001-s9s.conf
	$ ln -sfn /etc/apache2/sites-available/s9s-ssl.conf /etc/apache2/sites-enabled/001-s9s-ssl.conf
	$ sed -i 's|^[ \t]*SSLCertificateFile.*|SSLCertificateFile /etc/ssl/certs/s9server.crt|g' /etc/apache2/sites-available/s9s-ssl.conf
	$ sed -i 's|^[ \t]*SSLCertificateKeyFile.*|SSLCertificateKeyFile /etc/ssl/certs/s9server.key|g' /etc/apache2/sites-available/s9s-ssl.conf

12. Enable Apache’s SSL and rewrite module and create a symlink to sites-enabled for default HTTPS virtual host:

.. code-block:: bash

	$ a2enmod ssl
	$ a2enmod rewrite
	$ a2ensite default-ssl

13. Copy the ClusterControl UI and CMONAPI default files and also assign correct permission to those files:

.. code-block:: bash

	$ cp -f /var/www/clustercontrol/bootstrap.php.default /var/www/clustercontrol/bootstrap.php
	$ cp -f /var/www/cmonapi/config/bootstrap.php.default /var/www/cmonapi/config/bootstrap.php
	$ cp -f /var/www/cmonapi/config/database.php.default /var/www/cmonapi/config/database.php
	$ chmod 644 /var/www/clustercontrol/bootstrap.php
	$ chmod 644 /var/www/cmonapi/config/bootstrap.php
	$ chmod 644 /var/www/cmonapi/config/database.php

14. Assign correct ownership and permissions:

For Ubuntu 12.04/Debian 7 and earlier:

.. code-block:: bash

	$ chmod -R 777 /var/www/clustercontrol/app/tmp
	$ chmod -R 777 /var/www/clustercontrol/app/upload
	$ chown -Rf www-data.www-data /var/www/cmonapi/
	$ chown -Rf www-data.www-data /var/www/clustercontrol/

For Ubuntu 14.04/Debian 8 and later:

.. code-block:: bash

	$ chmod -R 777 /var/www/html/clustercontrol/app/tmp
	$ chmod -R 777 /var/www/html/clustercontrol/app/upload
	$ chown -Rf www-data.www-data /var/www/html/cmonapi/
	$ chown -Rf www-data.www-data /var/www/html/clustercontrol/
	
15. Use the generated value from step #8 and specify it in ``/var/www/clustercontrol/bootstrap.php`` under the ``RPC_TOKEN`` constant and configure MySQL credentials for the ClusterControl UI by updating the ``DB_PASS`` and ``DB_PORT`` constants with the cmon user password and MySQL port for ``dcps`` database:

.. code-block:: php

	define('DB_PASS', '{cmonpassword}');
	define('DB_PORT', '3306');
	define('RPC_TOKEN', '{Generated ClusterControl API token}');

.. Note:: Replace ``{cmonpassword}`` and ``{Generated ClusterControl API token}`` with appropriate values.

16. Use the generated value from step #8 and specify it in ``/var/www/cmonapi/config/bootstrap.php`` under the ``CMON_TOKEN`` constant. It is expected for the ``CMON_TOKEN``, ``RPC_TOKEN`` (step #14) and ``rpc_key`` (in cmon.cnf) are holding the same value. Also, update the ``CC_URL`` value to be equivalent to ClusterControl URL in your environment:

.. code-block:: php

	define('CMON_TOKEN', '{Generated ClusterControl API token}');
	define('CC_URL', 'https://{ClusterControl_host}/clustercontrol');

.. Note:: Replace ``{Generated ClusterControl API token}`` and ``{ClusterControl_host}`` with appropriate values.

17. Configure MySQL credentials for ClusterControl CMONAPI at ``/var/www/cmonapi/config/database.php``. In most cases, you just need to update the ``DB_PASS`` and ``DB_PORT`` constants with the cmon user password and MySQL port for the ``cmon`` database:

.. code-block:: php

	define('DB_PASS', '{cmonpasword}');
	define('DB_PORT', '3306');

.. Note:: Replace ``{cmonpassword}`` with the relevant value.

18. Restart Apache web server to apply the changes:

.. code-block:: bash

	$ sudo service apache2 restart

19. Enable ClusterControl on boot and start them:

For sysvinit/upstart:

.. code-block:: bash

	$ sudo update-rc.d cmon defaults
	$ sudo update-rc.d cmon-ssh defaults
	$ sudo update-rc.d cmon-events defaults
	$ sudo update-rc.d cmon-cloud defaults
	$ service cmon start
	$ service cmon-ssh start
	$ service cmon-events start
	$ service cmon-cloud start

For systemd:

.. code-block:: bash

	$ systemctl enable cmon cmon-ssh cmon-events cmon-cloud
	$ systemctl start cmon cmon-ssh cmon-events cmon-cloud

20. Generate a SSH key to be used by ClusterControl when connecting to all managed hosts. In this example, we are using 'root' user to connect to the managed hosts. To generate a SSH key for the root user, do:

.. code-block:: bash

	$ whoami
	root
	$ ssh-keygen -t rsa # Press enter for all prompts

.. Note:: If you are running as sudoer, the default SSH key will be located under ``/home/$USER/.ssh/id_rsa``. See `Operating System User <requirements.html#operating-system-user>`_.

21. Open ClusterControl UI at :samp:`http://{ClusterControl_host}/clustercontrol` and create the default admin password by providing a valid email address and password. You will be redirected to ClusterControl default page. Go to *Global Settings* > *Cluster Registrations* and enter the generated ClusterControl API token (step #14) and CMONAPI URL :samp:`http://127.0.0.1/cmonapi`, similar to example below:

.. image:: img/cc_register_token.png
   :alt: Register ClusterControl API token
   :align: center

You will then be redirected to the ClusterControl landing page. 

22. Before importing a database server/cluster into ClusterControl or deploy a new cluster, set up passwordless SSH from ClusterControl host to the database host(s). Use the following command to copy the SSH key to the target hosts:

.. code-block:: bash

	$ ssh-copy-id -i ~/.ssh/id_rsa {SSH user}@{IP address of the target node}

.. Note:: Replace ``{SSH user}`` and ``{IP address of the target node}`` with appropriate values. Repeat the command for all target hosts.

The installation is complete and you can start to import existing or deploy a new database cluster. Please review the `User Guide <user-guide/>`_ for details.


Offline Installation
--------------------

ClusterControl provides a helper script to install and configure ClusterControl packages in an Internetless environment, available at ``/var/www/clustercontrol/app/tools/setup-cc.sh``.

Take note that the following ClusterControl features will not work without Internet connection:

* `Backup > Create/Schedule Backup > Upload to Cloud` - requires connection to cloud providers.
* `Integrations > Cloud Providers` - requires connection to cloud providers.
* `Manage > Load Balancer` - requires connection to EPEL, ProxySQL, HAProxy, MariaDB repository.
* `Manage > Upgrades` - requires connection to provider's repository.
* `Deploy Database Cluster` - requires connection to database provider's repository.

Prior to the offline install, make sure you meet the following requirements for the ClusterControl node:

* Ensure the offline repository is ready. We assume that you already configured an offline repository. Details on how to setup offline repository is explained in the `Setting up Offline Repository`_ section.
* Firewall, SELinux or AppArmor must be turned off. You can turn on the firewall once the installation has completed. Make sure to allow ports as defined in `Firewall and Security Groups <requirements.html#firewall-and-security-groups>`_.
* MySQL server must be installed and running on the ClusterControl host.

Setting up Offline Repository
++++++++++++++++++++++++++++++

The installer script requires an offline repository to satisfy the dependencies. In this documentation, we provide steps to configure offline repository on CentOS 7, Debian 7 and Ubuntu 16.04 LTS. 

CentOS 7
`````````

1. Insert the CentOS 7 installation disc into the DVD drive.

2. Mount the DVD installation disc into the default media location at ``/media/CentOS``:

.. code-block:: bash

	$ mkdir /media/CentOS
	$ mount /dev/cdrom /media/CentOS

3. Disable the default repository by adding ``enabled=0`` to "base", "updates" and "extras" directives. You should have something like this inside ``/etc/yum.repos.d/CentOS-Base.repo``:

.. code-block:: bash

  [base]
  name=CentOS-$releasever - Base
  mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
  #baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
  gpgcheck=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
  enabled=0
  
  #released updates
  [updates]
  name=CentOS-$releasever - Updates
  mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
  #baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
  gpgcheck=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
  enabled=0
  
  #additional packages that may be useful
  [extras]
  name=CentOS-$releasever - Extras
  mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
  #baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
  gpgcheck=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
  enabled=0

4. Update the "enabled" value under the ``c6-media`` directive in ``/etc/yum.repos.d/CentOS-Media.repo``, as shown below:

.. code-block:: bash

  [c6-media]
  name=CentOS-$releasever - Media
  baseurl=file:///media/CentOS/
          file:///media/cdrom/
          file:///media/cdrecorder/
  gpgcheck=1
  enabled=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

5. Get the list of available packages:

.. code-block:: bash

  $ yum list

Make sure the last step does not produce any error.

Debian 7
`````````

1. Download the ISO images from the respective vendor site and upload them onto the ClusterControl host. You should have something like this on Debian 7.6:

.. code-block:: bash

	$ ls -1 | grep debian
	debian-7.6.0-amd64-DVD-1.iso
	debian-7.6.0-amd64-DVD-2.iso
	debian-7.6.0-amd64-DVD-3.iso

2. Create mount points and mount each of the ISO images accordingly:

.. code-block:: bash

	$ mkdir /mnt/debian-dvd1 /mnt/debian-dvd2 /mnt/debian-dvd3
	$ mount debian-7.6.0-amd64-DVD-1.iso /mnt/debian-dvd1
	$ mount debian-7.6.0-amd64-DVD-2.iso /mnt/debian-dvd2
	$ mount debian-7.6.0-amd64-DVD-3.iso /mnt/debian-dvd3

3. Add the following lines into /etc/apt/sources.list and comment the other lines:

.. code-block:: bash

	deb file:/mnt/debian-dvd1/ wheezy main contrib
	deb file:/mnt/debian-dvd2/ wheezy main contrib
	deb file:/mnt/debian-dvd3/ wheezy main contrib

4. Retrieve the new list of packages:

.. code-block:: bash

	$ apt-get update

Make sure the last step does not produce any error.

Ubuntu 16.04
`````````````

1. Insert Ubuntu 16.04 installation disc into the DVD drive.

2. Mount the disk as ``/media/cdrom``:

.. code-block:: bash

	$ sudo mkdir /media/cdrom
	$ sudo mount /dev/cdrom /media/cdrom/

3. Uncomment the following line (the first line) inside ``/etc/apt/sources.list`` and comment the other lines: 

.. code-block:: bash

	deb cdrom:[Ubuntu-Server 16.04.2 LTS _Xenial Xerus_ - Release amd64 (20170215.8)]/ xenial main restricted

4. Retrieve the new list of packages:

.. code-block:: bash

	$ sudo apt-get update

Make sure the last step does not produce any error.

Performing Offline Installation
++++++++++++++++++++++++++++++++

RedHat/CentOS
``````````````

1. The offline installation script will need a running MySQL server on the host. Install MySQL server and client, enable it to start on boot and start the service:

.. code-block:: bash

	$ yum install -y mariadb mariadb-server
	$ systemctl enable mariadb
	$ systemctl start mariadb

2. Configure MySQL root password for the newly installed MySQL server:

.. code-block:: bash

	$ mysqladmin -uroot password yourR00tP4ssw0rd

3. Create the staging directory called ``s9s_tmp`` and download/upload the latest version of ClusterControl RPM packages from the `Severalnines download page <http://www.severalnines.com/downloads/cmon/>`_, for example:

.. code-block:: bash

	$ mkdir ~/s9s_tmp
	$ cd ~/s9s_tmp
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-1.6.2-4942-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cmonapi-1.6.2-330-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-controller-1.6.2-2662-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-notifications-1.6.2-136-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-ssh-1.6.2-59-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cloud-1.6.2-141-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-clud-1.6.2-141-x86_64.rpm

.. Attention:: In this example, we downloaded the package directly to simplify the package preparation step. If the ClusterControl server does not have Internet connections, you have to upload the packages manually to the mentioned staging path.

4. Perform the package installation manually:

.. code-block:: bash

	$ yum localinstall clustercontrol-*


5. Execute the post-installation script to configure ClusterControl components and follow the installation wizard accordingly:

.. code-block:: bash

	$ /var/www/html/clustercontrol/app/tools/setup-cc.sh

6. Open the browser and navigate to :samp:`https://{ClusterControl_host}/clustercontrol`. Setup the super admin account by specifying a valid email address and password on the welcome page.

.. Note:: You would see this error: "Sorry we are not able to retrieve your license information. Please register your license under Settings - Subscription". This is expected because the demo license is automatically retrieved from our license server automatically via Internet. Please contact our Sales or Support team for a free 30-day demo license.


Debian/Ubuntu
``````````````

1. Install MySQL on the host and enable it on boot:

.. code-block:: bash

	$ sudo apt-get install -y --force-yes mysql-client mysql-server
	$ sudo systemctl enable mysql

2. Create the staging directory called ``s9s_tmp`` and download the latest version of ClusterControl DEB packages from `Severalnines download page <http://www.severalnines.com/downloads/cmon/>`_, for example:

.. code-block:: bash

	$ mkdir ~/s9s_tmp
	$ cd ~/s9s_tmp
	$ wget https://severalnines.com/downloads/cmon/clustercontrol_1.6.2-4942_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cmonapi_1.6.2-330_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-controller-1.6.2-2662-x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-notifications_1.6.2-136_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-ssh_1.6.2-59_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cloud_1.6.2-141_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-clud_1.6.2-141_x86_64.deb

.. Attention:: In this example, we downloaded the package directly to simplify the package preparation step. If the ClusterControl server does not have internet connections, you have to upload the packages manually to the mentioned staging path.

3. Perform the package installation and ClusterControl dependencies manually:

.. code-block:: bash

	$ sudo apt-get -f install ntp gnuplot
	$ sudo dpkg -i clustercontrol*.deb

4. Execute the post-installation script to configure ClusterControl components and follow the installation wizard accordingly:

.. code-block:: bash

	$ sudo /var/www/clustercontrol/app/tools/setup-cc.sh

5. Open the browser and navigate to :samp:`https://{ClusterControl_host}/clustercontrol`. Setup the super admin account by specifying a valid email address and password on the welcome page.

.. Note:: You would see this error: "Sorry we are not able to retrieve your license information. Please register your license under Settings - Subscription". This is expected because ClusterControl was trying to pull and configure a demo license from the license server via Internet. Please contact our Sales or Support team for a free 30-day demo license.

Post-installation
+++++++++++++++++

Once ClusterControl is up and running, you can import your existing cluster or deploy a new database cluster and start managing them from one place. Make sure passwordless SSH is configured from ClusterControl node to your database nodes.

1. Generate a SSH key on ClusterControl node:

.. code-block:: bash

	$ ssh-keygen -t rsa # press Enter on all prompts

2. Setup passwordless SSH to ClusterControl and database nodes:

.. code-block:: bash

	$ ssh-copy-id -i ~/.ssh/id_rsa {os_user}@{IP address/hostname}

Repeat step 2 for all database hosts that you are going to manage (including the ClusterControl node itself).
