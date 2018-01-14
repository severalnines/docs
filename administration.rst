.. _administration:

Administration
===============

This section provides detailed information on how to administer ClusterControl.

Upgrading ClusterControl
------------------------

There are several ways to upgrade ClusterControl to the latest version. However, we recommend users to perform `Online Upgrade`_ where the instuctions are mostly up-to-date.

Online Upgrade
``````````````

This is the recommended way to upgrade ClusterControl. The following upgrade procedures require internet connection on ClusterControl node.

RedHat/CentOS
'''''''''''''

1) Setup the `Severalnines Repository <installation.html#severalnines-repository>`_.

2) This step is only applicable if you are **upgrading from version older than 1.3.x and you have** ``cluster_id`` **parameter inside** ``/etc/cmon.cnf`` (usually when you deployed ClusterControl using the deprecated Severalnines Configurator). Before performing the upgrade procedure as described in step #3, create a default ``/etc/cmon.cnf`` and move the current configuration into ``/etc/cmon.d`` directory. Starting from version 1.3.0, ClusterControl Controller (CMON) handles its configuration files differently, where it expects the ``/etc/cmon.cnf`` is the default configuration file containing minimal parameters. The minimal parameters are:

.. code-block:: bash

  mysql_port={controller mysql server port}
  mysql_password={cmon user password}
  mysql_hostname={controller hostname/IP address}
  hostname={controller hostname/IP address}

2a) Create a new CMON configuration directory:

.. code-block:: bash

  $ mkdir /etc/cmon.d

2b) Check the value of ``cluster_id`` in the existing ``/etc/cmon.cnf``:

.. code-block:: bash
  
  $ grep cluster_id /etc/cmon.cnf
  cluster_id=1

.. Attention:: If the ``grep`` command returns nothing, you can skip the remaining sub-steps and proceed to step 3.

2c) Copy the existing ``cmon.cnf`` into the configuration directory. The destination must be in ``cmon_{cluster_id}.cnf``. For example, if you have ``cluster_id=1`` as retrieved in step 2b, the destination file name is ``cmon_1.cnf``:

.. code-block:: bash
  
  $ cp /etc/cmon.cnf /etc/cmon.d/cmon_1.cnf
  
2d) Edit the existing ``/etc/cmon.cnf`` and remove all parameters except the 4 parameters mentioned in step 2, for example:

.. code-block:: bash
  
  mysql_port=3306
  mysql_password=cmon
  mysql_hostname=192.168.1.13
  hostname=192.168.1.13

2e) Verify the new hierarchy of CMON configuration files. It should be similar to below:

.. code-block:: bash

  $ ls -1 /etc/cmon*
  /etc/cmon.cnf
  
  /etc/cmon.d:
  cmon_1.cnf

Now, we can safely perform the package upgrade as described in the next steps.

.. Note:: Your old configuration file now lives in ``/etc/cmon.d/cmon_{cluster_id}.cnf``.


3) Clear yum cache so it will retrieve the latest repository list and perform the upgrade:

.. code-block:: bash

	$ yum clean all
	$ yum install clustercontrol clustercontrol-cmonapi clustercontrol-controller clustercontrol-ssh clustercontrol-notifications clustercontrol-cloud clustercontrol-clud

4) If you are upgrading from version 1.3.0 or later, you can skip this step. Upgrade the CMON database for ClusterControl controller. When performing an upgrade from an older version, it is compulsory to apply the SQL modification files relative to the upgrade. For example, when upgrading from version 1.2.9 to version 1.5.1, apply all SQL modification files between those versions in sequential order:

.. code-block:: bash

	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.9-1.2.10.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.10-1.2.11.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.11-1.2.12.sql
 	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.12-1.3.0.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.0-1.3.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.1-1.3.2.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.2-1.4.0.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.0-1.4.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.1-1.4.2.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.2-1.5.0.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.5.0-1.5.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_data.sql

.. Attention:: ClusterControl 1.3.0 introduces automatic schema upgrade where it will check the CMON DB version upon startup after the upgrade. If the schema version is not as expected, it will perform the import automatically.

5) Upgrade the dcps database for ClusterControl UI:

.. code-block:: bash

	$ mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql

6) Clear the ClusterControl UI cache:

.. code-block:: bash

	$ rm -f /var/www/html/clustercontrol/app/tmp/cache/models/*

7) Restart the ClusterControl services:

For sysvinit and upstart:

.. code-block:: bash

	$ service cmon restart
	$ service cmon-ssh restart
	$ service cmon-events restart
	$ service cmon-cloud restart

For systemd:

.. code-block:: bash

	$ systemctl restart cmon
	$ systemctl restart cmon-ssh
	$ systemctl restart cmon-events
	$ systemctl restart cmon-cloud

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > General Settings > Version* or by using command ``cmon -v``. You should re-login if your ClusterControl UI session is active.

Debian/Ubuntu
'''''''''''''

1) Setup the `Severalnines Repository <installation.html#severalnines-repository>`_.

2) This step is only applicable if you are **upgrading from version older than 1.3.x and you have** ``cluster_id`` **parameter inside** ``/etc/cmon.cnf`` (usually when you deployed ClusterControl using the deprecated Severalnines Configurator). Before performing the package upgrade as described in step #3, create a default ``/etc/cmon.cnf`` and move the current configuration into ``/etc/cmon.d`` directory. Starting from version 1.3.0, ClusterControl Controller (CMON) handles its configuration files differently, where it expects the ``/etc/cmon.cnf`` is the default configuration file containing minimal parameters. The minimal parameters are:

.. code-block:: bash

  mysql_port={controller mysql server port}
  mysql_password={cmon user password}
  mysql_hostname={controller hostname/IP address}
  hostname={controller hostname/IP address}

2a) Create a new CMON configuration directory:

.. code-block:: bash

  $ mkdir /etc/cmon.d

2b) Check the value of ``cluster_id`` in the existing ``/etc/cmon.cnf``:

.. code-block:: bash
  
  $ grep cluster_id /etc/cmon.cnf
  cluster_id=1

.. Attention:: If the ``grep`` command returns nothing, you can skip the remaining sub-steps and proceed to step 3.

2c) Copy the existing ``cmon.cnf`` into the configuration directory. The destination must be in ``cmon_{cluster_id}.cnf``. For example, if you have ``cluster_id=1`` as retrieved in step 2b, the destination file name is ``cmon_1.cnf``:

.. code-block:: bash
  
  $ cp /etc/cmon.cnf /etc/cmon.d/cmon_1.cnf
  
2d) Edit the existing ``/etc/cmon.cnf`` and remove all parameters except the 4 parameters mentioned in step 2, for example:

.. code-block:: bash
  
  mysql_port=3306
  mysql_password=cmon
  mysql_hostname=192.168.1.13
  hostname=192.168.1.13

2e) Verify the new hierarchy of CMON configuration files. It should be similar to below:

.. code-block:: bash

  $ ls -1 /etc/cmon*
  /etc/cmon.cnf
  
  /etc/cmon.d:
  cmon_1.cnf

Now, we can safely perform the package upgrade as described in the next steps.

.. Note:: Your old configuration file now lives in ``/etc/cmon.d/cmon_{cluster_id}.cnf``.

3) Update the repository list and perform the upgrade:

.. code-block:: bash

	$ sudo apt-get update
	$ sudo apt-get install clustercontrol clustercontrol-cmonapi clustercontrol-controller clustercontrol-ssh clustercontrol-notifications clustercontrol-cloud clustercontrol-clud

4) If you are upgrading from version 1.3.0 or later, you can skip this step. Upgrade the CMON database for ClusterControl controller. When performing an upgrade from an older version, it is compulsory to apply the SQL modification files relative to the upgrade. For example, when upgrading from version 1.2.9 to version 1.5.1, apply all SQL modification files between those versions in sequential order:

.. code-block:: bash

	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.9-1.2.10.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.10-1.2.11.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.11-1.2.12.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.12-1.3.0.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.0-1.3.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.1-1.3.2.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.2-1.4.0.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.0-1.4.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.1-1.4.2.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.2-1.5.0.sql
	$ mysql -f- h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.5.0-1.5.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_data.sql

.. Attention:: ClusterControl 1.3.0 introduces automatic schema upgrade where it will check the CMON DB version upon startup after the upgrade. If the schema version is not as expected, it will perform the import automatically.

5) Upgrade the dcps database for ClusterControl UI:

.. code-block:: bash

	# For Ubuntu 14.04/Debian 8 or later, where wwwroot is /var/www/html:
	$ mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql
	# For Debian 7 and Ubuntu 12.04, where wwwroot is /var/www:
	$ mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/clustercontrol/sql/dc-schema.sql

6) Clear the ClusterControl UI cache:

.. code-block:: bash

	# For Ubuntu 14.04/Debian 8 or later, where wwwroot is /var/www/html:
	$ sudo rm -f /var/www/html/clustercontrol/app/tmp/cache/models/*
	# For Debian 7 and Ubuntu 12.04, where wwwroot is /var/www:
	$ sudo rm -f /var/www/clustercontrol/app/tmp/cache/models/*

7) Restart the ClusterControl services:

For sysvinit and upstart:

.. code-block:: bash

	$ service cmon restart
	$ service cmon-ssh restart
	$ service cmon-events restart
	$ service cmon-cloud restart

For systemd:

.. code-block:: bash

	$ systemctl restart cmon
	$ systemctl restart cmon-ssh
	$ systemctl restart cmon-events
	$ systemctl restart cmon-cloud

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > General Settings > Version* or by using command ``cmon -v``. You should re-login if your ClusterControl UI session is active.

Offline Upgrade
```````````````

The following upgrade procedures can be performed without internet connection on ClusterControl node. You can get the ClusterControl packages from `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_.

Manual Upgrade
''''''''''''''

RedHat/CentOS
.............

1) Download the latest version of ClusterControl related RPM packages from `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

	$ wget https://severalnines.com/downloads/cmon/clustercontrol-1.5.1-4265-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cmonapi-1.5.0-290-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-controller-1.5.1-2299-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-notifications-1.5.0-70-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-ssh-1.5.0-39-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cloud-1.5.0-31-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-clud-1.5.0-31-x86_64.rpm

2) This step is only applicable if you are **upgrading from version older than 1.3.x and you have** ``cluster_id`` **parameter inside** ``/etc/cmon.cnf`` (usually when you deployed ClusterControl using Severalnines Configurator). Before performing the package upgrade as described in step #3, create a default ``/etc/cmon.cnf`` and move the current configuration into ``/etc/cmon.d`` directory. Starting from version 1.3.0, ClusterControl Controller (CMON) handles its configuration files differently, where it expects the ``/etc/cmon.cnf`` is the default configuration file containing minimal parameters. The minimal parameters are:

.. code-block:: bash

  mysql_port={controller mysql server port}
  mysql_password={cmon user password}
  mysql_hostname={controller hostname/IP address}
  hostname={controller hostname/IP address}


2a) Create a new CMON configuration directory:

.. code-block:: bash

  $ mkdir /etc/cmon.d

2b) Check the value of ``cluster_id`` in the existing ``/etc/cmon.cnf``:

.. code-block:: bash
  
  $ grep cluster_id /etc/cmon.cnf
  cluster_id=1
  
.. Attention:: If the ``grep`` command returns nothing, you may skip the remaining sub-steps and proceed to step 3.

2c) Copy the existing ``cmon.cnf`` into the configuration directory. The destination must be in ``cmon_{cluster_id}.cnf``. For example, if you have ``cluster_id=1`` as retrieved in step 2b, the destination file name is ``cmon_1.cnf``:

.. code-block:: bash
  
  $ cp /etc/cmon.cnf /etc/cmon.d/cmon_1.cnf
  
2d) Edit the existing ``/etc/cmon.cnf`` and remove all parameters except the 4 parameters mentioned in step 2, for example:

.. code-block:: bash
  
  mysql_port=3306
  mysql_password=cmon
  mysql_hostname=192.168.1.13
  hostname=192.168.1.13

2e) Verify the new hierarchy of CMON configuration files. It should be similar to below:

.. code-block:: bash

  $ ls -1 /etc/cmon*
  /etc/cmon.cnf
  
  /etc/cmon.d:
  cmon_1.cnf

Now, we can safely perform the package upgrade as described in the next steps.

.. Note:: Your old configuration file now lives in ``/etc/cmon.d/cmon_{cluster_id}.cnf``.

3) Install via yum so dependencies are met:

.. code-block:: bash

	$ yum localinstall clustercontrol-*


4) If you are upgrading from version 1.3.0 or later, you can skip this step. Upgrade the CMON database for ClusterControl controller. When performing an upgrade from an older version, it is compulsory to apply the SQL modification files relative to the upgrade. For example, when upgrading from version 1.2.9 to version 1.5.1, apply all SQL modification files between those versions in sequential order:

.. code-block:: bash

	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.9-1.2.10.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.10-1.2.11.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.11-1.2.12.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.12-1.3.0.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.0-1.3.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.1-1.3.2.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.2-1.4.0.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.0-1.4.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.1-1.4.2.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.2-1.5.0.sql
	$ mysql -f- h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.5.0-1.5.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_data.sql

.. Attention:: ClusterControl 1.3.0 introduces automatic schema upgrade where it will check the CMON DB version upon startup after the upgrade. If the schema version is not as expected, it will perform the import automatically.

5) Upgrade the dcps database for ClusterControl UI:

.. code-block:: bash

	$ mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql

6) Clear the ClusterControl UI cache:

.. code-block:: bash

	$ rm -f /var/www/html/clustercontrol/app/tmp/cache/models/*

7) Restart the ClusterControl services:

For sysvinit and upstart:

.. code-block:: bash

	$ service cmon restart
	$ service cmon-ssh restart
	$ service cmon-events restart
	$ service cmon-cloud restart

For systemd:

.. code-block:: bash

	$ systemctl restart cmon
	$ systemctl restart cmon-ssh
	$ systemctl restart cmon-events
	$ systemctl restart cmon-cloud

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > General Settings > Version*. You should re-login if your ClusterControl UI session is active.

Debian/Ubuntu
.............

1) Download the latest version of ClusterControl related DEB packages from `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

	$ wget https://severalnines.com/downloads/cmon/clustercontrol_1.5.1-4265_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cmonapi_1.5.0-290_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-controller-1.5.1-2299-x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-notifications_1.5.0-70_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-ssh_1.5.0-39_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cloud_1.5.0-31_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-clud_1.5.0-31_x86_64.deb

2) This step is only applicable if you are **upgrading from version older than 1.3.x and you have** ``cluster_id`` **parameter inside** ``/etc/cmon.cnf`` (usually when you deployed ClusterControl using Severalnines Configurator). Before performing the package upgrade as described in step #3, create a default ``/etc/cmon.cnf`` and move the current configuration into ``/etc/cmon.d`` directory. Starting from version 1.3.0, ClusterControl Controller (CMON) handles its configuration files differently, where it expects the ``/etc/cmon.cnf`` is the default configuration file containing minimal parameters. The minimal parameters are:

.. code-block:: bash

  mysql_port={controller mysql server's mysql port}
  mysql_password={cmon user password}
  mysql_hostname={controller hostname/IP address}
  hostname={controller hostname/IP address}

2a) Create a new CMON configuration directory:

.. code-block:: bash

  $ mkdir /etc/cmon.d

2b) Check the value of ``cluster_id`` in the existing ``/etc/cmon.cnf``:

.. code-block:: bash
  
  $ grep cluster_id /etc/cmon.cnf
  cluster_id=1

.. Attention:: If the ``grep`` command returns nothing, you may skip the remaining sub-steps and proceed to step 3.

2c) Copy the existing ``cmon.cnf`` into the configuration directory. The destination must be in ``cmon_{cluster_id}.cnf``. For example, if you have cluster_id=1 as retrieved in step 2b, the destination file name is ``cmon_1.cnf``:

.. code-block:: bash
  
  $ cp /etc/cmon.cnf /etc/cmon.d/cmon_1.cnf
  
2d) Edit the existing ``/etc/cmon.cnf`` and remove all parameters except the 4 parameters mentioned in step 2, for example:

.. code-block:: bash
  
  mysql_port=3306
  mysql_password=cmon
  mysql_hostname=192.168.1.13
  hostname=192.168.1.13

2e) Verify the new hierarchy of CMON configuration files. It should be similar to below:

.. code-block:: bash

  $ ls -1 /etc/cmon*
  /etc/cmon.cnf
  
  /etc/cmon.d:
  cmon_1.cnf

Now, we can safely perform the package upgrade as described in the next steps.

.. Note:: Your old configuration file now lives in ``/etc/cmon.d/cmon_{cluster_id}.cnf``.

3) Install via dpkg:

.. code-block:: bash

	$ sudo dpkg -i clustercontrol-*.deb

4) Upgrade the CMON database for ClusterControl controller. When performing an upgrade from an older version, it is compulsory to apply the SQL modification files relative to the upgrade. For example, when upgrading from version 1.2.9 to version 1.5.1, apply all SQL modification files between those versions in sequential order:

.. code-block:: bash

	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.9-1.2.10.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.10-1.2.11.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.11-1.2.12.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.12-1.3.0.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.0-1.3.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.1-1.3.2.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.3.2-1.4.0.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.0-1.4.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.1-1.4.2.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.4.2-1.5.0.sql
	$ mysql -f- h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.5.0-1.5.1.sql
	$ mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_data.sql

.. Attention:: ClusterControl 1.3.0 introduces automatic schema upgrade where it will check the CMON DB version upon startup after the upgrade. If the schema version is not as expected, it will perform the import automatically.

5) Upgrade the dcps database for ClusterControl UI:

.. code-block:: bash

	# For Ubuntu 14.04/Debian 8 or later, where wwwroot is /var/www/html:
	$ mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql
	# For Debian 7 and Ubuntu 12.04, where wwwroot is /var/www:
	$ mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/clustercontrol/sql/dc-schema.sql

6) Clear the ClusterControl UI cache:

.. code-block:: bash

	# For Ubuntu 14.04/Debian 8 or later, where wwwroot is /var/www/html:
	$ sudo rm -f /var/www/html/clustercontrol/app/tmp/cache/models/*
	# For Debian and Ubuntu 12.04, where wwwroot is /var/www:
	$ sudo rm -f /var/www/clustercontrol/app/tmp/cache/models/*

7) Restart the ClusterControl services:

For sysvinit and upstart:

.. code-block:: bash

	$ service cmon restart
	$ service cmon-ssh restart
	$ service cmon-events restart
	$ service cmon-cloud restart

For systemd:

.. code-block:: bash

	$ systemctl restart cmon
	$ systemctl restart cmon-ssh
	$ systemctl restart cmon-events
	$ systemctl restart cmon-cloud

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > General Settings > Version*. You should re-login if your ClusterControl UI session is active.

Backing Up ClusterControl
-------------------------

The backup tool in ``s9s_upgrade_cmon`` is deprecated. To backup ClusterControl manually, you can use your own method to copy or export following files:

ClusterControl CMON Controller
````````````````````````````````

* CMON binary: ``/usr/sbin/cmon``
* CMON SSH binary: ``/usr/sbin/cmon-ssh``
* CMON Events binary: ``/usr/sbin/cmon-events``
* CMON main configuration file: ``/etc/cmon.cnf``
* CMON configuration directory and all its content: ``/etc/cmon.d/*``
* CMON cron file: ``/etc/cron.d/cmon``
* CMON init.d file: ``/etc/init.d/cmon``
* CMON logfile: ``/var/log/cmon.log`` or ``/var/log/cmon*``
* CMON helper scripts: ``/usr/bin/s9s_*``
* CMON database dump file:

.. code-block:: bash

	mysqldump -ucmon -p{mysql_password} -h{mysql_hostname} -P{mysql_port} cmon > cmon_dump.sql

ClusterControl UI
`````````````````

* ClusterControl upload directory: ``{wwwroot}/cmon*``
* ClusterControl CMONAPI: ``{wwwroot}/cmonapi*``
* ClusterControl UI: ``{wwwroot}/clustercontrol*``
* ClusterControl UI database dump file:

.. code-block:: bash

	mysqldump -ucmon -p{mysql_password} -h{mysql_hostname} -P{mysql_port} dcps > dcps_dump.sql

Where, ``{wwwroot}`` is equal to the Apache document root and ``{mysql_password}``, ``{mysql_hostname}``, ``{mysql_port}`` are values defined in CMON configuration file.


Restoring ClusterControl
------------------------

Manual restoration can be performed by reverting the backup action and copying everything back to its original location. Restoration may require you to re-grant the 'cmon' user since the backup will not import the grant table of it. Please review the `CMON Database <components.html#cmon-database>`_ section on how to grant the 'cmon' user cmon.

Securing ClusterControl
-----------------------

Firewall and Security Group
```````````````````````````

If users used Severalnines Configurator to deploy a cluster, the deployment script disables firewalls by default to minimize the possibilities of failure during the cluster deployment. Once it is completed, it is important to secure the ClusterControl node and the database cluster. We recommend user to isolate their database infrastructure from the public Internet and just whitelist the known hosts or networks to connect to the database cluster.

ClusterControl requires ports used by the following services to be opened/enabled:

* ICMP (echo reply/request)
* SSH (default is 22)
* HTTP (default is 80)
* HTTPS (default is 443)
* MySQL (default is 3306)
* CMON RPC (default is 9500)
* CMON RPC-TLS (default is 9501)
* CMON Events (default is 9510)
* CMON SSH (default is 9511)
* Streaming port for database backup through netcat (default is 9999)

SSH
````

SSH is very critical for ClusterControl. It must be possible to SSH from the ClusterControl server to the other nodes in the cluster without password, thus the database nodes must accept the SSH port configured in CMON configuration file. Following best practices are recommended:

* Permit a very few people in the organization to access to the servers. The fewer the better.
* Lock down SSH access so it is not possible to SSH into the nodes from any other server than the ClusterControl server.
* Lock down the ClusterControl server so that it is not possible to SSH into it directly from the outside world.

File Permission
```````````````

CMON configuration and log files contain sensitive information e.g ``mysql_password`` or ``sudo`` where it stores user’s password. Ensure CMON configuration file, e.g ``/etc/cmon.cnf`` and ``/etc/cmon.d/cmon_[clusterid].cnf`` (if exists) have permission 700 while CMON log files, e.g ``/var/log/cmon.log`` and ``/var/log/cmon_[clusterid].log`` has 740 and both are owned by root.

HTTPS
``````

By default, the installation script installs and configures a self-signed certificate for ClusterControl UI. You can access it by pointing your browser to :samp:`https://{ClusterControl_host}/clustercontrol`. If you would like to use your own SSL certificate (e.g :samp:`https://secure.domain.com/clustercontrol`), just replace the key and certificate path inside Apache’s SSL configuration file and restart Apache daemon. Make sure the server's hostname matches with the SSL domain name that you would like to use.

Running on Custom Port
----------------------

ClusterControl is configurable to support non-default port for selected services:

SSH
```

ClusterControl requires same custom SSH port across all nodes in the cluster. Make sure you specified the custom port number in ``ssh_port`` option at CMON configuration file, for example:

.. code-block:: bash

	ssh_port=55055

HTTP or HTTPS
`````````````

Running HTTP or HTTPS on custom port will change the ClusterControl UI and the CMONAPI URL e.g :samp:`http://{ClusterControl_host}:8080/clustercontrol` and :samp:`https://{ClusterControl_host}:4433/cmonapi`. Thus, you may need to re-register the new CMONAPI URL for managed cluster at ClusterControl UI `Cluster Registration <user-guide/index.html#cluster-registrations>`_ page.

MySQL
`````

If you are running MySQL for CMON database on different ports, several areas need to be updated:

+-----------------------------------------+-------------------------------------------------+-----------------------------------------+
| Area                                    | File                                            | Example                                 |
+=========================================+=================================================+=========================================+
| CMON configuration file                 | ``/etc/cmon.cnf`` or ``/etc/cmon.d/cmon_N.cnf`` | ``mysql_port={custom_port}``            |
+-----------------------------------------+-------------------------------------------------+-----------------------------------------+
| ClusterControl CMONAPI database setting | ``{wwwroot}/cmonapi/config/database.php``       | ``define('DB_PORT', '{custom_port}');`` |
+-----------------------------------------+-------------------------------------------------+-----------------------------------------+
| ClusterControl UI database setting      | ``{wwwroot}/clustercontrol/bootstrap.php``      | ``define('DB_PORT', '{custom_port}');`` |
+-----------------------------------------+-------------------------------------------------+-----------------------------------------+

.. Note:: Where ``{wwwroot}`` is the Apache document root and ``{custom_port}`` is the MySQL custom port.

HAProxy
```````

By default, HAProxy statistic page will be configured to run on port 9600. To change to another port, change following line in ``/etc/haproxy/haproxy.cfg``:

.. code-block:: bash

	listen admin_page 0.0.0.0:{your custom port}

Save and restart the HAProxy service.

Housekeeping
------------

ClusterControl monitoring data will be purged based on the value set at *ClusterControl UI > Settings > General Settings > History* (default is 7 days). Some users might find this value to be too low for auditing purposes. You can increase the value accordingly however, the longer collected data exist in CMON database, the bigger space it needs. It is recommended to lower the disk space threshold under *ClusterControl UI > Settings > Thresholds > Disk Space Utilization* so you will get early warning in case CMON database grows significantly.

If you intend to manually purge the monitoring data, you can truncate following tables (recommended to truncate based on the following order):

.. code-block:: mysql

	mysql> TRUNCATE TABLE mysql_advisor_history;
	mysql> TRUNCATE TABLE mysql_statistics_tm;
	mysql> TRUNCATE TABLE ram_stats_history;
	mysql> TRUNCATE TABLE cpu_stats_history;
	mysql> TRUNCATE TABLE disk_stats_history;
	mysql> TRUNCATE TABLE net_stats_history;
	mysql> TRUNCATE TABLE mysql_global_statistics_history;
	mysql> TRUNCATE TABLE mysql_statistics_history;
	mysql> TRUNCATE TABLE cmon_log_entries;
	mysql> TRUNCATE TABLE collected_logs;

The CMON Controller process has internal log rotation scheduling where it will log up to 5 MB in size before archiving ``/var/log/cmon.log`` and ``/var/log/cmon_{cluster id}.log``. The archived log will be named as ``cmon.log.1`` (or ``cmon_{cluster id}.log.1``) sequentially, with up to 9 archived log files (total of 10 log files rotation).

Migrating IP Address or Hostname
--------------------------------

ClusterControl relies on proper IP address or hostname configuration. To migrate to a new set of IP address or hostname, please update the old IP address/hostname occurrences in the following files:

* CMON configuration file: ``/etc/cmon.cnf`` and ``/etc/cmon.d/cmon_N.cnf`` (``hostname`` and ``mysql_hostname`` values)
* ClusterControl CMONAPI configuration file: ``{wwwroot}/cmonapi/config/bootstrap.php``
* HAProxy configuration file (if installed): ``/etc/haproxy/haproxy.cfg``

.. Note:: This section does not cover IP address migration of your database nodes. The easiest solution would be to remove the database cluster from ClusterControl UI using *Delete Cluster* and add it again by using *Import Existing Server/Cluster* in the deployment dialog.

Next, revoke 'cmon' user privileges for old hosts on ClusterControl node and all managed database nodes:

.. code-block:: mysql

	mysql> REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'cmon'@'{old ClusterControl IP address or hostname}';

Then, grant cmon user with new IP address or hostname on ClusterControl node and all managed database nodes:

.. code-block:: mysql

	mysql> GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'{new ClusterControl IP address or hostname}' IDENTIFIED BY '{mysql password}' WITH GRANT OPTION;
	mysql> FLUSH PRIVILEGES;

Or, instead of revoke and re-grant, you can just simply update the MySQL user table:

.. code-block:: mysql

	mysql> UPDATE mysql.user SET host='{new IP address}' WHERE host='{old IP address}';
	mysql> FLUSH PRIVILEGES;

Restart CMON service to apply the changes:

.. code-block:: bash

	$ service cmon restart

Examine the output of the CMON log file to verify the IP migration status. The CMON Controller should report errors and shut down if it can not connect to the specified database hosts or the CMON database. Once the CMON Controller is started, you can remove the old IP addresses/hostname from the managed host list at *ClusterControl > Manage > Hosts*.

Standby ClusterControl Server for High Availability
---------------------------------------------------

It is possible to have several ClusterControl servers to monitor a single cluster. This is useful if you have a multi-datacenter cluster and you may need to have ClusterControl on the remote site to monitor and manage the alive nodes if connection between them goes down. However, ClusterControl servers must be configured to be working in active/passive mode to avoid race conditions when digesting queries and recovering failed node or cluster.

In active mode, the ClusterControl node act as a primary controller, where it can perform automatic recovery and parsing MySQL slow log query for query  monitoring. The secondary ClusterControl node however must have following things configured:

* Cluster/Node auto recovery must be turned off.
* Query sampling must be disabled.

Installing Standby Server
`````````````````````````

Steps in this section must be performed on the secondary ClusterControl server.

1) Install ClusterControl as explained in the Getting Started page.

2) Add the same cluster via *ClusterControl > Add Existing Server/Cluster*. Ensure you choose "Enable Node AutoRecovery: No" and "Enable Cluster AutoRecovery: No" in the dialog box. Click "Add Cluster" to start the job.

3) Once the cluster is added, disable query sampling by go to *ClusterControl > Settings > Query Monitoring > Sampling Time = -1*.

Nothing should be performed on the primary side. The primary ClusterControl server shall perform automatic recovery in case of node or cluster failure.

Failover Method
```````````````

If you want to make the standby server run in active mode, just do as follow (assume the primary ClusterControl is unreachable at the moment):

* Cluster/Node auto recovery must be turned on. Click on both red power icons in the summary bar until they appear in green color.
* Enable query sampling. Go to *ClusterControl > Settings > Query Monitor* and change "Sampling Time" to other than "-1".

That's it. You should notice that the standby server has taken over the primary role.

Changing 'cmon' or 'root' Password
----------------------------------

ClusterControl has a helper script to change MySQL root password of your database cluster and for cmon database user called ``s9s_change_passwd``. It requires you to supply the old password so cmon user could access the database nodes and perform password update automatically. This tool is NOT intended for password reset.

On ClusterControl server, get :term:`s9s-admin tools` from our `Github repository <https://github.com/severalnines/s9s-admin>`_:

.. code-block:: bash

	$ git clone https://github.com/severalnines/s9s-admin.git

If you have already cloned s9s-admin, it's important for you to update it first:

.. code-block:: bash

	$ cd s9s-admin
	$ git pull

To change password for the 'cmon' user:

.. code-block:: bash

	$ cd s9s-admin/ccadmin
	$ ./s9s_change_passwd --cmon -i1 -p <current cmon password> -n <new cmon password>

To change password for the 'root' user:

.. code-block:: bash

	$ cd s9s-admin/ccadmin
	$ ./s9s_change_passwd --root -i1 -p <cmon password> -o <old root password> -n <new root password>

.. Warning:: The script only supports alpha-numeric characters. Special characters like "$!%?" will not work.

Graceful Shutdown
-----------------

In testing environment, you might need to perform a shutdown on ClusterControl and monitored database hosts in a graceful way. Depending on the clustering technology, the order of startup and shutting down is vital to keep the whole cluster in sync and ensure smooth start up operation in the future.

It's recommended to let the ClusterControl node to be the last one to shutdown, since it needs to oversee the state of the monitored hosts and saves it into CMON database. When starting up the database cluster at the later stage, ClusterControl will perform a proper start-up procedure based on the last known state of the monitored hosts.

ClusterControl needs to know whether the database cluster that you are shutting down is a shutdown outside of ClusterControl domain. 

Therefore, the proper way to shutdown the database hosts is:

MySQL Replication
``````````````````

Shutting down:

1. Shutdown the application manually. This usually outside of ClusterControl domain.
2. Shutdown the Keepalived (if exists) by doing system shut down or through ClusterControl UI.
3. Shutdown the load balancer service (if exists) by doing system shut down or through ClusterControl UI.
4. Shutdown the slaves by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
5. Shutdown the master by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
6. Shutdown ClusterControl host by doing system shut down.

Starting up:

1. Start ClusterControl host. Ensure you are able to connect to the UI and see the last state of the database cluster.
2. Start the master. If auto recover is turned on, this master will be started automatically.
3. Once the master is started, start the remaining slaves. 
4. Start the load balancer service (if exists).
5. Start the virtual IP service (if exists).
6. Start the application manually. This usually outside of ClusterControl domain.

MySQL Galera
`````````````

Shutting down:

1. Shutdown the application manually. This usually outside of ClusterControl domain.
2. Shutdown the Keepalived (if exists) by doing system shut down or through ClusterControl UI.
3. Shutdown the load balancer service (if exists) by doing system shut down or through ClusterControl UI.
4. Shutdown the database cluster by using *ClusterControl > Cluster Actions > Stop Cluster > Proceed*.
5. Shutdown ClusterControl host by doing system shut down.

Starting up:

1. Start ClusterControl host. Ensure you are able to connect to the UI and see the last state of the database cluster.
2. If auto recovery is turned on, the cluster will be started automatically. Otherwise, go to *ClusterControl > Cluster Actions > Bootstrap Cluster > Proceed*
3. If auto recovery is turned on, load balancer and virtual IP service will be started automatically. Otherwise, start the load balancer service and virtual IP service accordingly.
4. Start the application manually. This usually outside of ClusterControl domain.

MySQL Cluster (NDB)
````````````````````

Shutting down:

1. Shutdown the application manually. This usually outside of ClusterControl domain.
2. Shutdown the Keepalived (if exists) by doing system shut down or through ClusterControl UI.
3. Shutdown the load balancer service (if exists) by doing system shut down or through ClusterControl UI.
4. Shutdown the MySQL API servers by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
5. Shutdown the Data (NDB) servers by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
6. Shutdown the MySQL Cluster management servers by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
7. Shutdown ClusterControl host by doing system shut down.

Starting up:

1. Start ClusterControl host. Ensure you are able to connect to the UI and see the last state of the database cluster.
2. If auto recover is turned on, the cluster will be started automatically. Otherwise, start the nodes in this order - MySQL management, MySQL data, MySQL API.
3. Start the load balancer service (if exists).
4. Start the virtual IP service (if exists).
5. Start the application manually. This usually outside of ClusterControl domain.

MongoDB ReplicaSet
``````````````````

Shutting down:

1. Shutdown the application manually. This usually outside of ClusterControl domain.
2. Shutdown the secondaries by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
3. Shutdown the primary by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
4. Shutdown ClusterControl host by doing system shut down.

Starting up:

1. Start ClusterControl host. Ensure you are able to connect to the UI and see the last state of the database cluster.
2. Start the primary. If auto recover is turned on, this primary will be started automatically.
3. Once the primary is started, start the remaining secondaries. 
4. Start the application manually. This usually outside of ClusterControl domain.

PostgreSQL Replication
```````````````````````

1. Shutdown the application manually. This usually outside of ClusterControl domain.
2. Shutdown the slaves by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
3. Shutdown the master by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
4. Shutdown ClusterControl host by doing system shut down.

Starting up:

1. Start ClusterControl host. Ensure you are able to connect to the UI and see the last state of the database cluster.
2. Start the master. If auto recover is turned on, this master will be started automatically.
3. Once the master is started, start the remaining slaves. 
4. Start the application manually. This usually outside of ClusterControl domain.

.. Note:: If a database node is being shutdown gracefully outside of ClusterControl knowledge (through command line init script, systemd or ``kill -15``), ClusterControl will still attempt to recover the database node if *Node AutoRecovery* is turned on. Unless, the node is marked as 'Under Maintenance'.

Uninstall
---------

If ClusterControl is installed on a dedicated host (i.e., not co-located with your application), uninstalling ClusterControl is pretty straightforward. It is enough to bring down the ClusterControl node and revoke the cmon user privileges from the managed database nodes:

.. code-block:: mysql

	mysql> DELETE FROM mysql.user WHERE user = 'cmon';
	mysql> FLUSH PRIVILEGES;

Before removing the package, stop all cmon related services:

On systemd:

.. code-block:: bash

	$ systemctl stop cmon
	$ systemctl stop cmon-ssh
	$ systemctl stop cmon-events
	$ systemctl stop cmon-cloud

On SysVinit:

.. code-block:: bash

	$ service cmon stop
	$ service cmon-ssh stop
	$ service cmon-events stop
	$ service cmon-cloud stop

If ClusterControl is installed through Severalnines repository, use following command to uninstall via respective package manager:

On CentOS/RHEL:

.. code-block:: bash

	$ yum remove -y clustercontrol*
	
On Debian/Ubuntu:

.. code-block:: bash

	$ sudo apt-get remove -y clustercontrol*

Else, to uninstall ClusterControl Controller manually so you can to re-use the host for other purposes, kill the CMON process and remove all ClusterControl related files and databases:

.. code-block:: bash

	$ killall -9 cmon
	$ rm -rf /usr/sbin/cmon*
	$ rm -rf /usr/bin/cmon*
	$ rm -rf /usr/bin/s9s_*
	$ rm -rf /usr/share/cmon*
	$ rm -rf /etc/init.d/cmon
	$ rm -rf /etc/cron.d/cmon
	$ rm -rf /var/log/cmon*
	$ rm -rf /var/lib/cmon*
	$ rm -rf /etc/cmon*
	$ rm -rf {wwwroot}/cmon*
	$ rm -rf {wwwroot}/clustercontrol*
	$ rm -rf {wwwroot}/cc-*

For CMON and ClusterControl UI databases and privileges:

.. code-block:: mysql

	mysql> DROP SCHEMA cmon;
	mysql> DROP SCHEMA dcps;
	mysql> DELETE FROM mysql.user WHERE user = 'cmon';
	mysql> FLUSH PRIVILEGES;

.. Note:: Replace ``{wwwroot}`` with value defined in CMON configuration file.
