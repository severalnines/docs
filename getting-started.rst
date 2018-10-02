.. _Getting Started:

Getting Started
===============

*Step #1*

Install the latest ClusterControl:

.. code-block:: bash
  
  $ wget --no-check-certificate https://severalnines.com/downloads/cmon/install-cc
  $ chmod +x install-cc
  $ ./install-cc   # as root or sudo user

If you have multiple network interface cards, assign one IP address for HOST variable as per example below:

.. code-block:: bash

  $ HOST=192.168.1.10 ./install-cc # as root or sudo user  

.. Note:: ClusterControl relies on a MySQL server as a data repository for the clusters it manages and an Apache server for the User Interface. The installation script will always install an Apache server on the host. An existing MySQL server can be used or a new MySQL server install is configured for minimum system requirements. If you have a larger server please make the necessary changes to the my.cnf file and restart the MySQL server after the installation.

If you want to perform a non-interactive installation, you can assign each variable with its value beforehand, similar to example below:

.. code-block:: bash

	$ S9S_CMON_PASSWORD=cmon S9S_ROOT_PASSWORD=root123 S9S_DB_PORT=3306 HOST=10.10.10.10 ./install-cc

*Step #2*

Open your web browser to :samp:`https://{ClusterControl IP address or hostname}/clustercontrol` and create the default Admin user by entering a valid email address and password.

*Step #3*

Setup passwordless SSH to all target nodes (ClusterControl and all database nodes). Run following commands on ClusterControl server:

.. code-block:: bash

	$ ssh-keygen -t rsa # press enter on all prompts
	$ ssh-copy-id -i ~/.ssh/id_rsa [ClusterControl IP address]
	$ ssh-copy-id -i ~/.ssh/id_rsa [Database nodes IP address] # repeat this to all target database nodes

.. Note:: For cloud infrastructure, you may skip this step since all nodes should have been configured with a keypair. Ensure the keypair exists in ClusterControl node and you are good for the next step.

.. Attention:: If you use a non-root user, then it must be in sudoers on all nodes. Read `here <requirements.html#operating-system-user>`_ for details.

*Step #4*

Deploy a new server/cluster or import an existing database server/cluster into ClusterControl via:

:ref:`Import Existing Server Cluster` | :ref:`Deploy Database Cluster` | :ref:`Deploy in the Cloud`

*Step #5*

Start managing and monitor your database instances:

:ref:`User Guide`

*Step #6*

`Follow our blog <http://severalnines.com/blog/>`_ | `Exchange with other users <http://support.severalnines.com/forums/20303393-Community-Help>`_ | `Contact Severalnines Support <http://support.severalnines.com/home>`_ | `Contact Sales <http://www.severalnines.com/contact-us>`_
