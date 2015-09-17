.. _getting-started:

Getting Started
===============

*Step #1*

Install the latest ClusterControl:

.. code-block:: bash
  
  $ wget http://www.severalnines.com/downloads/cmon/install-cc
  $ chmod +x install-cc
  $ ./install-cc   # as root or sudo user

If you have multiple network interface cards, assign one IP address as per below:

.. code-block:: bash

  $ HOST=ip_address ./install-cc # as root or sudo user  

.. Note:: The installation script will always install an Apache server on the host. An existing MySQL server can be used or a new MySQL server install is configured for minimum system requirements. If you have a larger server please make the necessary changes to the ``my.cnf`` file and restart the MySQL server after the installation.

*Step #2*

Open your web browser to http://[ClusterControl_host]/clustercontrol and create the default Admin user by entering a valid email address and password.

*Step #3*

Setup passwordless SSH to all target nodes (ClusterControl and all database nodes). Run following commands on ClusterControl:

.. code-block:: bash

	$ ssh-keygen -t rsa # press enter on all prompts
	$ ssh-copy-id -i ~/.ssh/id_rsa [ClusterControl IP address]
	$ ssh-copy-id -i ~/.ssh/id_rsa [Database nodes IP address] # repeat this to all target database nodes

.. Attention:: If you use a non-root user, then it must be in sudoers on all nodes. Read `here <requirements.html#operating-system-user>`_ for details.

*Step #4*

Deploy a new server/cluster or add an existing database server/cluster into ClusterControl via:

`Add Existing Servers/Clusters <user-guide/index.html#add-existing-server-cluster>`_ | `Create Database Cluster <user-guide/index.html#create-database-cluster>`_ | `Create Database Node <user-guide/index.html#create-database-node>`_

*Step #5*

Start managing and monitor your database instances:

`ClusterControl User Guide <user-guide/index.html>`_

*Step #6*

`Follow our blog <http://severalnines.com/blog/>`_ | `Exchange with other users <http://support.severalnines.com/forums/20303393-Community-Help>`_ | `Contact Severalnines Support <http://support.severalnines.com/home>`_ | `Contact Sales <http://www.severalnines.com/contact-us>`_
