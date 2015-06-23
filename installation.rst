.. _installation:

Installation
============

This section provides detailed information on how to get ClusterControl installed on your environment. If you are looking for a simpler way to install ClusterControl, please have a look at `Getting Started <getting-started.html>`_ section.


Severalnines Repository
-----------------------

Severalnines provides YUM/APT repositories at http://repo.severalnines.com with instructions provided on the landing page. The Automatic Installation described in the next section is automatically configured with these repositories.

YUM repository
``````````````

1. Manually import the Severalnines repository public key into your RPM keyring:

.. code-block:: bash

	$ rpm --import http://repo.severalnines.com/severalnines-repos.asc

2. You can download the repository definition from `Severalnines download page <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

	$ wget http://www.severalnines.com/downloads/cmon/s9s-repo.repo -P /etc/yum.repos.d/

Or, you can create the repository definition manually:

.. code-block:: bash

	$ cat - > /etc/yum.repos.d/s9s-repo.repo <<REPOEND
	[s9s-repo]
	name = Severalnines Release Repository
	baseurl = http://repo.severalnines.com/rpm/os/x86_64
	enabled = 1
	gpgkey = http://repo.severalnines.com/severalnines-repos.asc
	gpgcheck = 1
	REPOEND

3. Look for ClusterControl packages:

.. code-block:: bash

	$ yum search clustercontrol

APT repository
``````````````

1. Manually add Severalnines repository public key into your APT keyring:

.. code-block:: bash

	$ wget http://repo.severalnines.com/severalnines-repos.asc -O- | sudo apt-key add -

2. You can download the repository definition from `Severalnines download page <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

	$ sudo wget http://www.severalnines.com/downloads/cmon/s9s-repo.list -P /etc/apt/sources.list.d/

Or, add the Severalnines APT source list manually:

.. code-block:: bash

  $ echo 'deb [arch=amd64] http://repo.severalnines.com/deb ubuntu main' | sudo tee /etc/apt/sources.list.d/s9s-repo.list

3. Update package list:

.. code-block:: bash

	$ sudo apt-get update

4. Look for ClusterControl packages:

.. code-block:: bash

	$ sudo apt-cache search clustercontrol


Automatic Installation
----------------------

We have a bunch of scripts and tools to automate and simplify the installation process of ClusterControl in various environments:

* Severalnines Configurator
* Installation script (install-cc)
* Post-installation script (setup-cc.sh)
* Bootstrap script
* Puppet module
* Chef cookbooks
* Docker image

Severalnines Configurator
`````````````````````````

`Severalnines Configurator <http://www.severalnines.com/configurator>`_ is a free online tool to deploy a database cluster on your servers. It generates a deployment package based on the values you input in the wizard, the deployment package then automates the installation on your servers. Users need to prepare a group of servers for the database cluster plus one dedicated host for ClusterControl.

The Configurator is able to deploy different types of database clusters as stated in the `Supported Database Server/Cluster <intro.html#supported-database-server-cluster>`_ section.

Deployment package
''''''''''''''''''

The deployment package will generate a deployment script with ``cluster_id=1``, defined inside ``[package name]/[cluster type]/scripts/install/.s9s/config``. The scripts are indempotent, you can execute it as many time as you like and you still get the same outcome. You can change the cluster ID value to bigger than 1, which will cause the deployment scripts to assume that you are deploying a new cluster and you want to add it into an existing ClusterControl running on the same host. This will skip the ClusterControl installation part.

Deployment steps
''''''''''''''''

1. Go to the `Severalnines Configurator <http://www.severalnines.com/configurator>`_, choose your database cluster and enter the details of your server environment and configuration.

2. SSH into the ClusterControl host and download the deployment script

.. code-block:: bash

	$ wget [deployment package from online configurator page]

3. Extract the deployment package:

.. code-block:: bash

	$ tar -xzf [deployment package]

4. Navigate to the installation directory:

.. code-block:: bash

	$ cd [package name]/[cluster type]/scripts/install

5. Execute the deployment script:

.. code-block:: bash

	$ bash ./deploy.sh 2>&1 | tee cc.log

Answer a few questions (mostly setting up the SSH environment and firewall) and your cluster will be automatically installed. The deployment usually takes about 15 minutes. Firewall and SElinux will be disabled during the installation.

Once the installation is complete, open a web browser and go https://[ClusterControl_host]/clustercontrol . Setup the default admin account by specifying a valid email address and password on the welcome page.


Installer Script (install-cc)
`````````````````````````````

Installer script is the recommended way to install ClusterControl if you already have existing database server or cluster, though ClusterControl UI is also capable deploying a new database cluster just like `Severalnines Configurator <installation.html#severalnines-configurator>`_.

On ClusterControl server, run following commands:

.. code-block:: bash

  $ wget http://www.severalnines.com/downloads/cmon/install-cc
  $ chmod +x install-cc
  # omit sudo if you run as root
  $ sudo ./install-cc

.. Note:: The installation script will always install an Apache server on the host. An existing MySQL server can be used or a new MySQL server install is configured for minimum system requirements. If you have a larger server please make the necessary changes to the ``my.cnf`` file and restart the MySQL server after the installation.

After the installation completes, open your web browser to http://[ClusterControl_host]/clustercontrol and create the default admin user by specifying a valid email address and password on the welcome page.

Post-installation script (setup-cc.sh)
``````````````````````````````````````

In order to use post-installation script, you have to install ClusterControl UI package via package manager beforehand.

1. On ClusterControl node, setup `Severalnines repository <installation.html#severalnines-repository>`_

2. Install ClusterControl UI by using following command:

.. code-block:: bash

  # omit sudo if you run as root
  $ yum install -y clustercontrol # Redhat/CentOS
  $ sudo apt-get update && sudo apt-get install -y clustercontrol # Debian/Ubuntu

.. Note:: ClusterControl requires extra post-installation setup steps, such as generating an API token, configuring cmon/dcps database schema, grant privileges on cmon schema, setting up SSL and so on. We provide a post-installation script for this purpose at ``[wwwroot]/clustercontrol/app/tools/setup-cc.sh``. If you are installing for the first time, you are required to run this script to ensure ClusterControl is properly set up.

3. Execute the post installation script as below:

.. code-block:: bash

  # omit sudo if you run as root
  $ sudo /var/www/html/clustercontrol/app/tools/setup-cc.sh # Redhat/CentOS/Ubuntu =>14.04
  $ sudo /var/www/clustercontrol/app/tools/setup-cc.sh # Debian/Ubuntu <14.04
 
Basically, the post-installation script will attempt to automate the following tasks:

* Install and configure a MySQL server (used by ClusterControl to store monitoring data)
* Configure ClusterControl UI and cmonapi packages
* Install and configure the ClusterControl Controller package via package manager
* Install ClusterControl dependencies via package manager
* Configure Apache and SSL
* Configure ClusterControl API token
* Configure ClusterControl Controller with minimal configuration options
* Enable the CMON service on boot and start it

Once the installation completes, login to the ClusterControl UI at http://[ClusterControl_host]/clustercontrol and create the default admin user by entering a valid email address and password.

Bootstrap Script
````````````````

Bootstrap script is a legacy way to install ClusterControl on top of existing database cluster. Common use case when you want to install ClusterControl on top of MongoDB Sharded Cluster or MySQL Cluster. The reason behind this is only these database cluster types are not supported to be added through ClusterControl UI. You can also use bootstrap script to install ClusterControl standby server for high availability, as described `in this blog post <http://www.severalnines.com/blog/installing-clustercontrol-standby-server>`_.

You will need to prepare a host for ClusterControl, download the ``cc-bootstrap`` package on that host and execute it. It will install ClusterControl on the host and add your existing database cluster to ClusterControl.

.. Attention:: Even though it supports adding the other type of database cluster (particularly Galera, MySQL replication and MongoDB Replica Set), we do recommend that you install ClusterControl first and then use *Add Existing Server/Cluster* feature.

Requirements
''''''''''''

Make sure the following is ready prior to the installation:

* A new host for ClusterControl server is ready.
* You already have a database cluster up and running.
* Verify that sudo is working properly if you are using a non-root user.
* ClusterControl node must able to connect to all of the database nodes.

Installation
''''''''''''

The following steps should be performed on the ClusterControl host:

.. code-block:: bash

  $ wget http://www.severalnines.com/downloads/cmon/cc-bootstrap.tar.gz
  $ tar zxvf cc-bootstrap.tar.gz
  $ cd cc-bootstrap-*
  $ ./s9s_bootstrap --install

Answer all questions during the configuration stage.

Once finished, you should see following success banner:

.. code::

  =================================================
  ### CLUSTERCONTROL INSTALLATION COMPLETED. ###
  Use following details to access ClusterControl:
  URL      : https://192.168.1.80/clustercontrol
  Username : myemail@domain.com
  Password : admin
  
  ClusterControl API Token : d98316c4ab3340259b483defcf4655fda6215899
  ClusterControl API URL   : https://192.168.1.80/cmonapi
  =================================================

Open a web browser and go to mentioned URL. Setup the super admin account by specifying a valid email address and password on the welcome page.

Puppet module
`````````````

If you are automating your infrastructure using :term:`Puppet`, we have created a module for this purpose and it is available at `Puppet Forge <https://forge.puppetlabs.com/severalnines/clustercontrol>`_. Installing the module is as easy as:

.. code-block:: bash

	$ puppet module install severalnines-clustercontrol

Requirements
''''''''''''

If you haven’t change the default ``$modulepath``, this module will be installed under ``/etc/puppet/modules/clustercontrol`` on your Puppet master host. This module requires the following criteria to be met:

* The node for ClusterControl must be a clean/dedicated host.
* ClusterControl node must be running on 64bit OS platform and together with the same OS distribution with the monitored DB hosts. Mixing Debian with Ubuntu and CentOS with Red Hat is acceptable.
* ClusterControl node must have an internet connection during the deployment. After the deployment, ClusterControl does not need internet access.
* Make sure your database cluster is up and running before performing this deployment.

Pre-installation
''''''''''''''''

ClusterControl requires proper SSH key configuration and a ClusterControl API token. Use the helper script located at ``$modulepath/clustercontrol/files/s9s_helper.sh`` to generate them.

Generate SSH key to be used by ClusterControl to manage your database nodes. Run following command in Puppet master:

.. code-block:: bash

	$ bash /etc/puppet/modules/clustercontrol/files/s9s_helper.sh --generate-key

Then, generate an API token:

.. code-block:: bash

	$ bash /etc/puppet/modules/clustercontrol/files/s9s_helper.sh --generate-token
	b7e515255db703c659677a66c4a17952515dbaf5

.. Attention:: These two steps are mandatory and just need to run once (unless if you want to intentionally regenerate them). The first command will generate a RSA key (if not exists) to be used by the module and the key must exist in the Puppet master module's directory before the deployment begins.

Installation
''''''''''''
Specify the generated token in the node definition similar to example below.

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
      email_address => 'admin@localhost.xyz',
      mysql_server_addresses => '192.168.1.11,192.168.1.12,192.168.1.13',
      api_token => 'b7e515255db703c659677a66c4a17952515dbaf5'
    }
  }
  
  # Monitored DB hosts
  node "galera1.local", "galera2.local", "galera3.local" {
    class {'clustercontrol':
      is_controller => false,
      mysql_root_password => 'r00tpassword',
      clustercontrol_host => '192.168.1.10'
    }
  }


You can either instruct the agent to pull the configuration from the Puppet master and apply it immediately:

.. code-block:: bash

	$ puppet agent -t

Or, wait for the Puppet agent service to apply the catalog automatically (depending on the runinterval value, default is 30 minutes). Once completed, open the ClusterControl UI page at http://[ClusterControl_host]/clustercontrol and create the default admin user and password.

For more example on deployments using Puppet, please refer to `this blog post <http://www.severalnines.com/blog/clustercontrol-module-puppet>`_. For more details on configuration options, please refer to `ClusterControl Puppet Module <https://forge.puppetlabs.com/severalnines/clustercontrol>`_ page.

Chef cookbooks
``````````````

If you are automating your infrastructure using :term:`Chef`, we have created a cookbook for this purpose and it is available at `Chef Supermarket <https://supermarket.chef.io/cookbooks/clustercontrol>`_. Getting the cookbook is as easy as:

.. code-block:: bash

	$ knife cookbook site download clustercontrol

Requirements
''''''''''''

This cookbook requires the following criteria to be met:

* The node for ClusterControl must be a clean/dedicated host.
* ClusterControl node must be running on 64bit OS platform and together with the same OS distribution with the monitored DB hosts. Mixing Debian with Ubuntu and CentOS with Red Hat is acceptable.
* ClusterControl node must have an internet connection during the deployment. After the deployment, ClusterControl does not need internet access.
* Make sure your database cluster is up and running before performing this deployment.

Data items are used by the ClusterControl controller recipe to configure SSH public key on database hosts, grants cmon database user and setting up CMON configuration file. We provide a helper script located under ``clustercontrol/files/default/s9s_helper.sh``. Please run this script prior to the deployment.

Answer all the questions and at the end of the wizard, it will generate a data bag file called ``config.json`` and a set of command that you can use to create and upload the data bag. If you run the script for the first time, it will ask to re-upload the cookbook since it contains a newly generated SSH key: 

.. code-block:: bash

	$ knife cookbook upload clustercontrol
	

Chef Workstation
''''''''''''''''

This section shows example ClusterControl installation with Chef and requires you to use :term:`knife`. Please ensure it has been configured correctly and is able to communicate with the Chef Server before you proceed with the following steps. The steps in this section should be performed on the Chef Workstation node.

1. Get the ClusterControl cookbook using knife:

.. code-block:: bash

	$ cd ~/chef-repo/cookbooks
	$ knife cookbook site download clustercontrol
	$ tar -xzf clustercontrol-*
	$ rm -Rf *.tar.gz

2. Run ``s9s_helper.sh`` to auto generate SSH key files, ClusterControl API token, and data bag items:

.. code-block:: bash

  $ cd ~/chef-repo/cookbooks/clustercontrol/files/default
  $ ./s9s_helper.sh
  ==============================================
  Helper script for ClusterControl Chef cookbook
  ==============================================
  ClusterControl requires an email address to be configured as super admin user.
  What is your email address? [admin@localhost.xyz]: admin@domain.com
  
  What is the IP address for ClusterControl host?: 192.168.50.100
  
  ClusterControl will create a MySQL user called 'cmon' for automation tasks.
  Enter the user cmon password [cmon] : cmonP4ss2014
  
  What is your database cluster type? 
  (galera|mysqlcluster|mysql_single|replication|mongodb) [galera]: 
  
  What is your Galera provider?
  (codership|percona|mariadb) [percona]: codership
  
  ClusterControl requires an OS user for passwordless SSH. If user is not root, the user must be in sudoer list.
  What is the OS user? [root]: ubuntu
  
  Please enter the sudo password (if any). Just press enter if you are using sudo without password: 
  What is your SSH port? [22]: 
  
  List of your MySQL nodes (comma-separated list): 192.168.50.101,192.168.50.102,192.168.50.103
  ClusterControl needs to have your database nodes' MySQL root password to perform installation and grant privileges.
  
  Enter the MySQL root password on the database nodes [password]: myR00tP4ssword
  We presume all database nodes are using the same MySQL root password.
  
  Database data path [/var/lib/mysql]: 
  
  Generating config.json..
  {
   "id" : "config",
   "cluster_type" : "galera",
   "vendor" : "codership",
   "email_address" : "admin@domain.com",
   "ssh_user" : "ubuntu",
   "cmon_password" : "cmonP4ss2014",
   "mysql_root_password" : "myR00tP4ssword",
   "mysql_server_addresses" : "192.168.50.101,192.168.50.102,192.168.50.103",
   "datadir" : "/var/lib/mysql",
   "clustercontrol_host" : "192.168.50.100",
   "clustercontrol_api_token" : "1913b540993842ed14f621bba22272b2d9471d57"
  }
  
  Data bag file generated at /home/ubuntu/chef-repo/cookbooks/clustercontrol/files/default/config.json
  To upload the data bag, you can use the following command:
  $ knife data bag create clustercontrol
  $ knife data bag from file clustercontrol /home/ubuntu/chef-repo/cookbooks/clustercontrol/files/default/config.json
  
  Re-upload the cookbook since it contains a newly generated SSH key: 
  $ knife cookbook upload clustercontrol
  ** We highly recommend you to use encrypted data bag since it contains confidential information **

3. As per instructions above, on Chef Workstation host, do:

.. code-block:: bash

	$ knife data bag create clustercontrol
	Created data_bag[clustercontrol]

	$ knife data bag from file clustercontrol /home/ubuntu/chef-repo/cookbooks/clustercontrol/files/default/config.json
	Updated data_bag_item[clustercontrol::config]
	
	$ knife cookbook upload clustercontrol
	Uploading clustercontrol [0.1.0]
	Uploaded 1 cookbook.

4. Create two roles, ``cc_controller`` and ``cc_db_hosts``:

.. code-block:: bash

	$ cat cc_controller.rb 
	name "cc_controller"
	description "ClusterControl Controller"
	run_list ["recipe[clustercontrol]"]

The DB host role:

.. code-block:: bash

  $ cat cc_db_hosts.rb
  name "cc_db_hosts"
  description "Database hosts monitored by ClusterControl"
  run_list ["recipe[clustercontrol::db_hosts]"]
  override_attributes({ 
    "mysql" => {
       "basedir" => "/usr/local/mysql"
     }
  })


.. Note:: In above example, we set an override attribute because the MySQL server is installed under ``/usr/local/mysql``. For more details on attributes, please refer to ``attributes/default.rb`` in the cookbook.

5. Add the defined roles into Chef Server:

.. code-block:: bash

	$ knife role from file cc_controller.rb
	Updated Role cc_controller!
	 
	$ knife role from file cc_db_hosts.rb
	Updated Role cc_db_hosts!

6. Assign the roles to the relevant nodes:

.. code-block:: bash

	$ knife node run_list add clustercontrol.domain.com "role[cc_controller]"
	$ knife node run_list add galera1.domain.com "role[cc_db_hosts]"
	$ knife node run_list add galera2.domain.com "role[cc_db_hosts]"
	$ knife node run_list add galera3.domain.com "role[cc_db_hosts]"


Chef Client
'''''''''''

Let :term:`chef-client` run on each Chef client node and apply the cookbook:

.. code-block:: bash

	$ sudo chef-client

Once completed, open the ClusterControl UI page at http://[ClusterControl_host]/clustercontrol and create the default admin user and password. 

For more example on deployments using Chef, please refer to `this blog post <http://www.severalnines.com/blog/chef-cookbooks-clustercontrol-management-and-monitoring-your-database-clusters>`_. For more details on configuration options, please refer to `ClusterControl Chef Cookbooks <https://supermarket.chef.io/cookbooks/clustercontrol>`_ page.

Docker image
````````````

The :term:`Docker` image comes with ClusterControl installed and configured with all of its components, so you can immediately use it to manage and monitor your existing databases. 

Having a Docker image for ClusterControl at the moment is convenient in terms of how quickly it is to get it up and running and it's 100% reproducible. Docker users can now start testing ClusterControl, since we have images that everyone can pull down and then launch the tool.

It is a start and our plan is to add better integration with the Docker API in future releases in order to transparently manage Docker containers/images within ClusterControl, e.g., to launch/manage and deploy database clusters using Docker images.

Build the image
'''''''''''''''

The Dockerfiles are available from `our Github repository <https://github.com/severalnines/docker>`_. You can build it manually by cloning the repository:

.. code-block:: bash

	$ git clone https://github.com/severalnines/docker
	$ cd docker/[operating system] 
	$ docker build -t severalnines/clustercontrol:[operating system] .

.. note:: Replace [operating system] with your choice of OS distribution; redhat6, redhat7, centos6, centos7, debian-wheezy, ubuntu-trusty.

Running container
'''''''''''''''''

Please refer to the `Docker Hub page <https://registry.hub.docker.com/u/severalnines/clustercontrol/>`_ for the latest instructions. Pick the operating system distribution images that you would like to deploy, and use the ``docker pull`` command to download the image. To pull all images:

.. code-block:: bash

	$ docker pull severalnines/clustercontrol

You can pull the ClusterControl image that you want based on your target cluster’s operating system.

.. code-block:: bash

	$ docker pull severalnines/clustercontrol:[ubuntu-trusty|debian-wheezy|redhat6|redhat7]

So, if you want to pull the ClusterControl image for CentOS 6/Redhat 6, just run:

.. code-block:: bash

	$ docker pull severalnines/clustercontrol:redhat6 #or
	$ docker pull severalnines/clustercontrol:centos6

.. note:: Image tagged with ‘centos6’ or ‘centos7’ is aliased to redhat’s respectively.

Use the following command to run:

.. code-block:: bash

	$ docker run -d --name clustercontrol -p 5000:80 severalnines/clustercontrol:redhat7

Once started, ClusterControl is accessible at http://[Docker_host]:5000/clustercontrol. You should see the welcome page to create a default admin user. Use your email address and specify passwords for that user. By default MySQL users root and cmon will be using 'password' and 'cmon' as default password respectively. You can override this value with -e flag, as example below:

.. code-block:: bash

	$ docker run -d --name clustercontrol -e CMON_PASSWORD=MyCM0n22 -e MYSQL_ROOT_PASSWORD=SuP3rMan -p 5000:80 severalnines/clustercontrol:debian
	
Optionally, you can map the HTTPS port using -p by appending the forwarding as below:

.. code-block:: bash

	$ docker run -d --name clustercontrol -p 5000:80 -p 5443:443 severalnines/clustercontrol:redhat7

Verify the container is running by using the ps command:

.. code-block:: bash

	$ docker ps

For more example on deployments with Docker images, please refer to `this blog post <http://www.severalnines.com/blog/clustercontrol-docker>`_. For more details on configuration options, please refer to `ClusterControl's Docker Hub <https://registry.hub.docker.com/u/severalnines/clustercontrol/>`_ page.

Manual Installation
-------------------

If you want to have more control on the installation process, you may perform manual installation.

.. note:: Installing and uninstalling ClusterControl shall not bring any downtime to your running database cluster.

The main installation steps are:

1. Install Severalnines yum/apt repository
2. Install ClusterControl packages
3. Execute the post-installation script (recommended) or perform manual installation

.. note:: On step #3, performing installation using the post-installation script is highly recommended. Manual installation instructions are provided in this guide for advanced users and reference.

ClusterControl requires three packages to be installed and configured:

* clustercontrol - ClusterControl web user interface
* clustercontrol-cmonapi - ClusterControl REST API
* clustercontrol-controller - ClusterControl CMON controller

Steps described in following sections should be perform on ClusterControl node unless specified otherwise.

Requirements
````````````

Make sure the following is ready prior to this installation:

* You already have a database cluster up and running
* Verify that sudo is working properly if you are using a non-root user
* ClusterControl node must able to connect to all database nodes
* Passwordless SSH from ClusterControl node to all nodes (including the ClusterControl node itself) has been configured correctly
* You must have internet connection on ClusterControl node during the installation process

Redhat/CentOS
``````````````

1. Setup `Severalnines YUM Repository <installation.html#yum-repository>`_.

2. Disable SElinux and open required ports (or stop iptables):

.. code-block:: bash

	sed -i 's|SELINUX=enforcing|SELINUX=disabled|g' /etc/selinux/config
	setenforce 0
	service iptables stop # RedHat/CentOS 6
	systemctl stop firewalld # RedHat/CentOS 7

3. Install required packages via package manager:

.. code-block:: bash

	yum -y install curl mailx cronie nc bind-utils mysql mysql-server

4. Install ClusterControl packages:

.. code-block:: bash

	yum -y install clustecontrol clustercontrol-cmonapi clustercontrol-controller

5. Start MySQL server (MariaDB for Redhat/CentOS 7), enable it on boot and set a MySQL root password:

.. code-block:: bash

	service mysqld start # Redhat/CentOS 6
	systemctl start mariadb.service # Redhat/CentOS 7
	chkconfig mysqld on # Redhat/CentOS 6
	systemctl enable mariadb.service # Redhat/CentOS 7
	mysqladmin -uroot password 'themysqlrootpassword'
	
6. Create two databases called cmon and dcps and grant the cmon user:

.. code-block:: bash

	mysql -uroot -p -e 'DROP SCHEMA IF EXISTS cmon; CREATE SCHEMA cmon'
	mysql -uroot -p -e 'DROP SCHEMA IF EXISTS dcps; CREATE SCHEMA dcps'
	mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"localhost" IDENTIFIED BY "[cmonpassword]" WITH GRANT OPTION'
	mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"127.0.0.1" IDENTIFIED BY "[cmonpassword]" WITH GRANT OPTION'
	mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"[ClusterControl main IP address]" IDENTIFIED BY "[cmonpassword]" WITH GRANT OPTION'
	mysql -uroot -p -e 'FLUSH PRIVILEGES'

.. note:: Replace ``[ClusterControl main IP address]`` and ``[cmonpassword]`` with respective values.

7. Import cmon and dcps schema structure and data:

.. code-block:: bash

	mysql -uroot -p cmon < /usr/share/cmon/cmon_db.sql
	mysql -uroot -p cmon < /usr/share/cmon/cmon_data.sql
	mysql -uroot -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql
	
8. Clear the default CMON configuration file at ``/etc/cmon.cnf`` so we can setup a minimal options:

.. code-block:: bash

	sudo cat /dev/null > /etc/cmon.cnf

And add following lines for minimal configuration options:

.. code-block:: bash

	mysql_port=3306
	mysql_hostname=[ClusterControl main IP address]
	mysql_password=[cmonpassword]
	hostname=[ClusterControl main IP address]

Example is as follow:

.. code-block:: bash

	mysql_port=3306
	mysql_hostname=192.168.1.85
	mysql_password=cmon
	hostname=192.168.1.85

.. Attention:: The value of ``mysql_hostname`` and ``hostname`` must be the same that you used to grant user ``cmon@[ClusterControl main IP address]`` in step #6 above.

9. Enable CMON daemon on boot and start it:

.. code-block:: bash

	chkconfig cmon on # Redhat/CentOS 6
	service cmon start # Redhat/CentOS 6
	systemctl enable cmon # Redhat/CentOS 7
	systemctl start cmon # Redhat/CentOS 7

10. Configure Apache to use ``AllowOverride=All`` and set up SSL key and certificate:

.. code-block:: bash

	cp -f /var/www/html/cmonapi/ssl/server.crt /etc/pki/tls/certs/s9server.crt
	cp -f /var/www/html/cmonapi/ssl/server.key /etc/pki/tls/private/s9server.key
	rm -rf /var/www/html/cmonapi/ssl
	sed -i 's|AllowOverride None|AllowOverride All|g' /etc/httpd/conf/httpd.conf
	sed -i 's|AllowOverride None|AllowOverride All|g' /etc/httpd/conf.d/ssl.conf
	sed -i 's|^SSLCertificateFile.*|SSLCertificateFile /etc/pki/tls/certs/s9server.crt|g' /etc/httpd/conf.d/ssl.conf
	sed -i 's|^SSLCertificateKeyFile.*|SSLCertificateKeyFile /etc/pki/tls/private/s9server.key|g' /etc/httpd/conf.d/ssl.conf

11. Copy the ClusterControl UI and CMONAPI default files:

.. code-block:: bash

	cp -f /var/www/html/clustercontrol/bootstrap.php.default /var/www/html/clustercontrol/bootstrap.php
	cp -f /var/www/html/cmonapi/config/bootstrap.php.default /var/www/html/cmonapi/config/bootstrap.php
	cp -f /var/www/html/cmonapi/config/database.php.default /var/www/html/cmonapi/config/database.php

12. Assign correct ownership and permission:

.. code-block:: bash

	chmod -R 777 /var/www/html/clustercontrol/app/tmp
	chmod -R 777 /var/www/html/clustercontrol/app/upload
	chown -Rf apache.apache /var/www/html/cmonapi/
	chown -Rf apache.apache /var/www/html/clustercontrol/

13. Configure MySQL credentials for the ClusterControl UI at ``/var/www/html/clustercontrol/bootstrap.php``. In most cases, you just need to update the ``DB_PASS`` parameter with the cmon user password:

.. code-block:: php

	define('DB_PASS', '[cmonpassword]');

.. Note:: Replace ``[cmonpassword]`` with a relevant value.

14. Generate a ClusterControl API token to be used by cmonapi:

.. code-block:: bash

	python -c 'import uuid; print uuid.uuid4()' | sha1sum | cut -f1 -d' '
	6856d96a19d049aa8a7f4a5ba57a34740b3faf57

15. Use the generated value from the previous command and specify it in ``/var/www/html/cmonapi/bootstrap.php`` under the ``CMON_TOKEN`` parameter. Also, update the ``CC_URL`` value to be equivalent to ClusterControl URL in your environment:

.. code-block:: php

	define('CMON_TOKEN', '[Generated ClusterControl API token]');
	define('CC_URL', 'https://[ClusterControl_host]/clustercontrol');

.. Note:: Replace ``[Generated ClusterControl API token]`` and ``[ClusterControl_host]`` with appropriate values.

16. Configure MySQL credential for ClusterControl CMONAPI at ``/var/www/html/cmonapi/database.php``. In most cases, you just need to update the ``DB_PASS`` parameter with the cmon user password:

.. code-block:: bash

	define('DB_PASS', '[cmonpasword]');

.. Note:: Replace ``[cmonpassword]`` with a relevant value.

17. Start the Apache web server and configure it to auto start on boot:

.. code-block:: bash

	service httpd start # Redhat/CentOS 6
	chkconfig httpd on # Redhat/CentOS 6
	systemctl start httpd # Redhat/CentOS 7
	systemctl enable httpd # Redhat/CentOS 6

18. Generate a SSH key to be used by ClusterControl so it can perform passwordless SSH to database hosts. If you are running as sudoer, the SSH key should be located under ``/home/$USER/.ssh/id_rsa``:

.. code-block:: bash

	ssh-keygen -t rsa # Press enter for all prompt

19. Before importing a database cluster or single-server to ClusterControl, ensure the ClusterControl node is able to do passwordless SSH to the database host(s). Use the following command to copy the SSH key to the target hosts:

.. code-block:: bash

	ssh-copy-id -i ~/.ssh/id_rsa [ssh user]@[IP address of the target node]

.. Note:: Replace ``[ssh user]`` and ``[IP address of the target node]`` with appropriate values.

20. Open ClusterControl Ui and create the default admin password by providing a valid email address and password. You will be redirted to ClusterControl default page. Go to `Cluster Registrations` and enter the generated ClusterControl API token (step #14) and URL, similar to example below:

.. image:: img/cc_register_token.png
   :alt: Register ClusterControl API token
   :align: center

You will then be redirected to the ClusterControl landing page and the installation is now complete. You can now start to manage your database cluster. Please review the User Guide for details.

Debian/Ubuntu
``````````````

The following steps should be performed on the ClusterControl node, unless specified otherwise. Ensure you have Severalnines repository and ClusterControl UI installed. Please refer to Severalnines Repository section for details. Omit sudo if you are installing as root user. Take note that for Ubuntu 14.04 and later, replace all occurrences of ``/var/www`` with ``/var/www/html`` in the following instructions.

1. Setup `Severalnines APT Repository <installation.html#apt-repository>`_.

2. If you have AppArmor running, disable it and open the required ports (or stop iptables):

.. code-block:: bash

	sudo /etc/init.d/apparmor stop
	sudo /etc/init.d/apparmor teardown
	sudo update-rc.d -f apparmor remove
	sudo service iptables stop

3. Install ClusterControl dependencies:

.. code-block:: bash

	sudo apt-get update
	sudo apt-get install -y curl mailutils dnsutils mysql-client mysql-server

4. Install the ClusterControl controller package:

.. code-block:: bash

	sudo apt-get install -y clustercontrol-controller

5. Comment the following line inside ``/etc/mysql/my.cnf`` to allow MySQL to listen on all interfaces:

.. code-block:: bash

	#bind-address=127.0.0.1

Restart the MySQL service to apply the change:

.. code-block:: bash

	service mysql restart

6. Create two databases called cmon and dcps and grant user cmon:

.. code-block:: bash

	mysql -uroot -p -e 'DROP SCHEMA IF EXISTS cmon; CREATE SCHEMA cmon'
	mysql -uroot -p -e 'DROP SCHEMA IF EXISTS dcps; CREATE SCHEMA dcps'
	mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"localhost" IDENTIFIED BY "[cmonpassword]" WITH GRANT OPTION'
	mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"127.0.0.1" IDENTIFIED BY "[cmonpassword]" WITH GRANT OPTION'
	mysql -uroot -p -e 'GRANT ALL PRIVILEGES ON *.* TO "cmon"@"[ClusterControl main IP address]" IDENTIFIED BY "[cmonpassword]" WITH GRANT OPTION'
	mysql -uroot -p -e 'FLUSH PRIVILEGES'

.. Note:: Replace [ClusterControl main IP address] and [cmonpassword] with respective values.

7. Import cmon and dcps schema:

.. code-block:: bash

	mysql -uroot -p cmon < /usr/share/cmon/cmon_db.sql
	mysql -uroot -p cmon < /usr/share/cmon/cmon_data.sql
	mysql -uroot -p dcps < /var/www/clustercontrol/sql/dc-schema.sql

8. Clear the default CMON configuration file at ``/etc/cmon.cnf`` so we can setup minimal configuration:

.. code-block:: bash

	sudo cat /dev/null > /etc/cmon.cnf

And add the following lines for minimal configuration options:

.. code-block:: bash

	mysql_port=3306
	mysql_hostname=[ClusterControl main IP address]
	mysql_password=[cmonpassword]
	hostname=[ClusterControl main IP address]

A sample configuration will be something like this:

.. code-block:: bash

	mysql_port=3306
	mysql_hostname=192.168.1.85
	mysql_password=cmon
	hostname=192.168.1.85

.. Note:: The value of ``mysql_hostname`` and ``hostname`` must be the same that you used to grant user ``cmon@[ClusterControl main IP address]`` in step #6.

9. Enable CMON daemon on boot and start it:

.. code-block:: bash

	sudo update-rc.d cmon defaults
	sudo service cmon start

10. Configure Apache ``AllowOverride`` and setting up SSL:

.. code-block:: bash

	cp -f /var/www/cmonapi/ssl/server.crt /etc/ssl/certs/s9server.crt
	cp -f /var/www/cmonapi/ssl/server.key /etc/ssl/private/s9server.key
	rm -rf /var/www/cmonapi/ssl
	sed -i 's|AllowOverride None|AllowOverride All|g' /etc/apache2/sites-available/default
	sed -i 's|AllowOverride None|AllowOverride All|g' /etc/apache2/sites-available/default-ssl
	sed -i 's|^[ \t]*SSLCertificateFile.*|SSLCertificateFile /etc/ssl/certs/s9server.crt|g' /etc/apache2/sites-available/default-ssl
	sed -i 's|^[ \t]*SSLCertificateKeyFile.*|SSLCertificateKeyFile /etc/ssl/private/s9server.key|g' /etc/apache2/sites-available/default-ssl

For Ubuntu 14.04, it runs on Apache 2.4 which has a slightly different configuration than above:

.. code-block:: bash

	cp -f /var/www/cmonapi/ssl/server.crt /etc/ssl/certs/s9server.crt
	cp -f /var/www/cmonapi/ssl/server.key /etc/ssl/private/s9server.key
	rm -rf /var/www/cmonapi/ssl
	cp -f /var/www/clustercontrol/app/tools/apache2/s9s.conf /etc/apache2/sites-available/
	cp -f /var/www/clustercontrol/app/tools/apache2/s9s-ssl.conf /etc/apache2/sites-available/
	rm -f /etc/apache2/sites-enabled/000-default.conf
	rm -f /etc/apache2/sites-enabled/default-ssl.conf
	rm -f /etc/apache2/sites-enabled/001-default-ssl.conf
	ln -sfn /etc/apache2/sites-available/s9s.conf /etc/apache2/sites-enabled/001-s9s.conf
	ln -sfn /etc/apache2/sites-available/s9s-ssl.conf /etc/apache2/sites-enabled/001-s9s-ssl.conf
	sed -i 's|^[ \t]*SSLCertificateFile.*|SSLCertificateFile /etc/ssl/certs/s9server.crt|g' /etc/apache2/sites-available/s9s-ssl.conf
	sed -i 's|^[ \t]*SSLCertificateKeyFile.*|SSLCertificateKeyFile /etc/ssl/private/s9server.key|g' /etc/apache2/sites-available/s9s-ssl.conf

11. Enable Apache’s SSL and rewrite module and create a symlink to sites-enabled for default HTTPS virtual host:

.. code-block:: bash

	a2enmod ssl
	a2enmod rewrite
	a2ensite default-ssl

12. Copy the ClusterControl UI and CMONAPI default files:

.. code-block:: bash

	cp -f /var/www/clustercontrol/bootstrap.php.default /var/www/clustercontrol/bootstrap.php
	cp -f /var/www/cmonapi/config/bootstrap.php.default /var/www/cmonapi/config/bootstrap.php
	cp -f /var/www/cmonapi/config/database.php.default /var/www/cmonapi/config/database.php

13. Assign correct ownership and permissions:

.. code-block:: bash

	chmod -R 777 /var/www/clustercontrol/app/tmp
	chmod -R 777 /var/www/clustercontrol/app/upload
	chown -Rf www-data.www-data /var/www/cmonapi/
	chown -Rf www-data.www-data /var/www/clustercontrol/

14. Configure MySQL credentials for ClusterControl UI at ``/var/www/clustercontrol/bootstrap.php``. In most cases, you just need to update the ``DB_PASS`` parameter with the cmon user password:

.. code-block:: php

	define('DB_PASS', '[cmonpassword]');

.. Note:: Replace [cmonpassword] with the relevant value.

15. Generate a ClusterControl API token to be used by CMONAPI:

.. code-block:: bash

	python -c 'import uuid; print uuid.uuid4()' | sha1sum | cut -f1 -d' '
	6856d96a19d049aa8a7f4a5ba57a34740b3faf57

16. Use the generated value from previous command and specify it in ``/var/www/cmonapi/config/bootstrap.php`` under ``CMON_TOKEN`` parameter. Also, update the ``CC_URL`` value to be equivalent to ClusterControl URL in your environment:

.. code-block:: php

	define('CMON_TOKEN', '[Generated ClusterControl API token]');
	define('CC_URL', 'https://[ClusterControl_host]/clustercontrol');

.. Note:: Replace ``[Generated ClusterControl API token]`` and ``[ClusterControl_host]`` with appropriate values.

17. Configure MySQL credentials for ClusterControl CMONAPI at ``/var/www/cmonapi/config/database.php``. In most cases, you just need to update the ``DB_PASS`` parameter with the cmon user password:

.. code-block:: php

	define('DB_PASS', '[cmonpasword]');

.. Note:: Replace ``[cmonpassword]`` with the relevant value.

18. Restart Apache web server to apply the changes:

.. code-block:: bash

	sudo service apache2 restart

19. Generate an SSH key to be used by ClusterControl so it can perform passwordless SSH to the database hosts. If you are running as sudoer, the SSH key should be located under ``/home/$USER/.ssh/id_rsa``:

.. code-block:: bash

	ssh-keygen -t rsa # Press enter for all prompt

20. Before importing a database cluster or single-server to ClusterControl, ensure the ClusterControl host is able to do passwordless SSH to the database host(s). Use following command to copy the SSH key to the target host:

.. code-block:: bash

	ssh-copy-id -i ~/.ssh/id_rsa [ssh user]@[IP address of the target node]

.. Note:: Replace ``[ssh user]`` and ``[IP address of the target node]`` with appropriate values.

21. Open ClusterControl Ui and create the default admin password by providing a valid email address and password. You will be redirected to ClusterControl default page. Go to `Cluster Registrations` and enter the generated ClusterControl API token (step #14) and URL, similar to example below:

.. image:: img/cc_register_token.png
   :alt: Register ClusterControl API token
   :align: center

You will then be redirected to the ClusterControl landing page and the installation is now complete. You can now start to manage your database cluster. Please review the User Guide for details.


Offline Installation
--------------------

The installer script (install-cc) also supports offline installations by specifying ``NO_INET=1`` as an environment variable. Note that the following ClusterControl features will not work without Internet connection:

* `Backup > Online Storage` - requires connection to AWS.
* `Service Providers > AWS Instances` - requires connection to AWS.
* `Service Providers > AWS VPC` - requires connection to AWS.
* `Manage > Load Balancer` - requires connection to EPEL repository/HAproxy download site.
* `Manage > Upgrades` - requires connection to Percona repository.

Prior to the offline install, make sure you meet the following requirements for the ClusterControl node:

* Ensure the offline repository is ready. We assume that you already configured an offline repository for this guide. Details on how to setup offline repository is explained on the next section.
* Firewall, SElinux or AppArmor must be turned off. You can turn on the firewall once the installation has completed. Make sure to allow ports as defined on this page.
* MySQL server must be installed on the ClusterControl host.
* ClusterControl packages for the selected version must exist under ``s9s_tmp`` directory from the script’s execution path.

Setting up Offline Repository
`````````````````````````````

The installer script requires an offline repository so it can automate the dependencies installation process. In this documentation, we provide steps to configure offline repository on Redhat/CentOS 6 and Debian 7. 

Redhat/CentOS 6
'''''''''''''''

1. Insert the DVD installation disc into the DVD drive.

2. Mount the DVD installation disc into the default media location at ``/media/CentOS``:

.. code-block:: bash

	mount /dev/cdrom /media/CentOS

3. Disable the default repository by adding ``enabled=0`` to "base", "updates" and "extras" directives. You should have something like this inside ``/etc/yum.repos.d/CentOS-Base.repo``:

.. code-block:: bash

  [base]
  name=CentOS-$releasever - Base
  mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
  #baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
  gpgcheck=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
  enabled=0
  
  #released updates
  [updates]
  name=CentOS-$releasever - Updates
  mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
  #baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
  gpgcheck=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
  enabled=0
  
  #additional packages that may be useful
  [extras]
  name=CentOS-$releasever - Extras
  mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
  #baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
  gpgcheck=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
  enabled=0
  …

4. Update the "enabled" value under the ``c6-media`` directive in ``/etc/yum.repos.d/CentOS-Media.repo``, as shown below:

.. code-block:: bash

  [c6-media]
  name=CentOS-$releasever - Media
  baseurl=file:///media/CentOS/
          file:///media/cdrom/
          file:///media/cdrecorder/
  gpgcheck=1
  enabled=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

5. Get the list of available packages:

.. code-block:: bash

  $ yum list

Make sure the last step does not produce any error.

Debian 7
''''''''

1. Download the ISO images from the respective vendor site and upload them onto the ClusterControl host. You should have something like this on Debian 7.6:

.. code-block:: bash

	$ ls -1 | grep debian
	debian-7.6.0-amd64-DVD-1.iso
	debian-7.6.0-amd64-DVD-2.iso
	debian-7.6.0-amd64-DVD-3.iso

2. Create mount points and mount each of the ISO images accordingly:

.. code-block:: bash

	mkdir /mnt/debian-dvd1 /mnt/debian-dvd2 /mnt/debian-dvd3
	mount debian-7.6.0-amd64-DVD-1.iso /mnt/debian-dvd1
	mount debian-7.6.0-amd64-DVD-2.iso /mnt/debian-dvd2
	mount debian-7.6.0-amd64-DVD-3.iso /mnt/debian-dvd3

3. Add the following lines into /etc/apt/sources.list and comment other lines:

.. code-block:: bash

	deb file:/mnt/debian-dvd1/ wheezy main contrib
	deb file:/mnt/debian-dvd2/ wheezy main contrib
	deb file:/mnt/debian-dvd3/ wheezy main contrib

4. Retrieve the new list of packages:

.. code-block:: bash

	$ apt-get update

Make sure the last step does not produce any error.

Pre-installation
````````````````

Redhat/CentOS
'''''''''''''

1. The offline installation script will need a running MySQL server on the host. Install MySQL server and client, enable it starts on boot and start the service:

.. code-block:: bash

	yum install -y mysql mysql-server
	chkconfig mysqld on
	service mysqld start

2. Configure MySQL root password for the newly installed MySQL server:

.. code-block:: bash

	mysqladmin -uroot password yourR00tP4ssw0rd

3. Create the staging directory called ``s9s_tmp`` and download/upload the latest version of clustercontrol, clustercontrol-cmonapi and clustercontrol-controller RPM packages from the `Severalnines download page <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

	mkdir ~/s9s_tmp
	cd ~/s9s_tmp
	wget http://www.severalnines.com/downloads/cmon/clustercontrol-1.2.10-418-x86_64.rpm
	wget http://www.severalnines.com/downloads/cmon/clustercontrol-cmonapi-1.2.10-61-x86_64.rpm
	wget http://www.severalnines.com/downloads/cmon/clustercontrol-controller-1.2.10-764-x86_64.rpm

.. Attention:: In this example, we downloaded the package directly to simplify the package preparation step. If the ClusterControl server does not have internet connections, you should upload the packages manually to the mentioned staging path.

4. Download and prepare the installation script with correct permission:

.. code-block:: bash

	cd ~
	wget http://www.severalnines.com/downloads/cmon/install-cc.sh
	chmod 755 install-cc.sh

Debian/Ubuntu
'''''''''''''

1. Install MySQL on the host:

.. code-block:: bash

	sudo apt-get install -y --force-yes mysql-client mysql-server

2. Create the staging directory called ``s9s_tmp`` and download the latest version of clustercontrol, clustercontrol-cmonapi and clustercontrol-controller DEB packages from `Severalnines download page <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

	mkdir ~/s9s_tmp
	cd ~/s9s_tmp
	wget http://www.severalnines.com/downloads/cmon/clustercontrol_1.2.10-418_x86_64.deb
	wget http://www.severalnines.com/downloads/cmon/clustercontrol-cmonapi_1.2.10-61_x86_64.deb
	wget http://www.severalnines.com/downloads/cmon/clustercontrol-controller-1.2.10-764-x86_64.deb

.. Attention:: In this example, we downloaded the package directly to simplify the package preparation step. If the ClusterControl server does not have internet connections, you should upload the packages manually to the mentioned staging path.

3. Download and prepare the installation script with correct permission:

.. code-block:: bash

	cd ~
	wget http://www.severalnines.com/downloads/cmon/install-cc.sh
	sudo chmod 755 install-cc.sh

Installing ClusterControl
`````````````````````````

1. Define ``NO_INET`` variable to 1 to tell the installation script to perform an offline installation and execute the installation script:

.. code-block:: bash

  $ export NO_INET=1
  $ ./install-cc.sh
  This script will install the ClusterControl UI and the Controller (optional).
  An Apache and MySQL server will also be installed. An existing MySQL Server on this host can be used.
   
  => Detected NO_INET is set, i.e., NO INTERNET enabled install.
  => Make sure you have an existing MySQL and Apache Server installed and running on this host!
  => Download these ClusterControl packages before continuing:
   
  => cd s9s_tmp
  => wget http://severalnines.com/downloads/cmon/s9s-clustercontrol-1.2.6.tar.gz
  => wget http://severalnines.com/downloads/cmon/cmon-controller-1.2.6-357-x86_64.rpm
   
  => The UI/Controller hostname is set to 192.168.253.133. Do you want to change it? (y/N):
  ...

Follow the installation wizard.

2. Open the browser and navigate to https://[ClusterControl_host]/clustercontrol . Setup the super admin account by specifying a valid email address and password on the welcome page.

Post-installation
`````````````````

Once ClusterControl is up and running, you can point it to your existing clusters and/or single-instance MySQL/MariaDB instances and start managing them from one place. Make sure passwordless SSH is configured from ClusterControl node to your database nodes.

1. Generate a SSH key on ClusterControl node:

.. code-block:: bash

	ssh-keygen -t rsa # press Enter on all prompts

2. Setup passwordless SSH to ClusterControl and database nodes:

.. code-block:: bash

	ssh-copy-id -i ~/.ssh/id_rsa [os_user]@[IP address/hostname]

Repeat step 2 for all database hosts that you are going to manage (including the ClusterControl node itself).