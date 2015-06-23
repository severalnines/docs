.. _administration:

Administration
===============

This section provides detailed information on ClusterControl requirement on hardware, system environment and security policy.

Upgrading ClusterControl
------------------------

There are several ways to upgrade ClusterControl to the latest version. By default, the ClusterControl UI will notify users of the latest release available via the notification bar, as shown in the following screenshot:

.. image:: img/docs_notification_update.png
   :align: center

You can disable the notification at *ClusterControl UI > Settings > General Settings > Version > untick “Check for updates”*. We recommend our users to keep up-to-date with the latest release once it is available. Details of changes in each release can be found in the `release changelog <http://support.severalnines.com/entries/21633407-ChangeLog>`_.

Online Upgrade
``````````````

The following upgrade procedures require internet connection on ClusterControl node.

Package Manager
'''''''''''''''

This is the recommended way to upgrade ClusterControl. We are currently standardizing the upgrade procedure through yum/apt package manager.

Redhat/CentOS
.............

Even if you have previously installed from our tarball or .deb packages, it is possible to switch to yum repository and perform the upgrade.

1) Setup the `Severalnines Repository <installation.html#severalnines-repository>`_.

2) Clear yum cache so it will retrieve the latest repository list and perform the upgrade:

.. code-block:: bash

	yum clean metadata
	yum install cmon-controller clustercontrol clustercontrol-cmonapi clustercontrol-controller

.. Note:: cmon-controller is a transition package for clustercontrol-controller. You can omit it from the command line if you previously didn't have cmon-controller package installed.

3) Upgrade the CMON database for ClusterControl controller. When performing an upgrade from an older version, it is compulsory to apply the SQL modification files relative to the upgrade. For example, when upgrading from version 1.2.6 to version 1.2.10, apply all SQL modification files between those versions in sequential order:

.. code-block:: bash

	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.6-1.2.7.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.7-1.2.8.sql
	mysql -f- h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.8-1.2.9.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.9-1.2.10.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_data.sql

.. Attention:: Importing ``cmon_db.sql`` and ``cmon_data.sql`` is required on each upgrade. Importing ``cmon_db_mods`` is relative to the previous version.

4) Upgrade the dcps database for ClusterControl UI:

.. code-block:: bash

	mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql

5) Clear the ClusterControl UI cache:

.. code-block:: bash

	rm -f /var/www/html/clustercontrol/app/tmp/cache/models/*

6) Restart the CMON controller service:

.. code-block:: bash

	sudo service cmon restart

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > General Settings > Version*. You should re-login if your ClusterControl UI session is active.

Debian/Ubuntu
.............

Even if you have previously installed from our tarball or .deb packages, it is possible to switch to apt repository and perform the upgrade.

1) Setup the `Severalnines Repository <installation.html#severalnines-repository>`_.

2) Update the repository list and perform the upgrade:

.. code-block:: bash

	sudo apt-get update
	sudo apt-get install cmon-controller clustercontrol clustercontrol-cmonapi clustercontrol-controller

.. Note:: cmon-controller is a transition package for clustercontrol-controller. You can omit it from the command line if you previously didn't have cmon-controller package installed.

3) Upgrade the CMON database for ClusterControl controller. When performing an upgrade from an older version, it is compulsory to apply the SQL modification files relative to the upgrade. For example, when upgrading from version 1.2.6 to version 1.2.10, apply all SQL modification files between those versions in sequential order:

.. code-block:: bash

	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.6-1.2.7.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.7-1.2.8.sql
	mysql -f- h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.8-1.2.9.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.9-1.2.10.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_data.sql

.. Attention:: Importing ``cmon_db.sql`` and ``cmon_data.sql`` is required on each upgrade. Importing ``cmon_db_mods`` is relative to the previous version.

4) Upgrade the dcps database for ClusterControl UI:

.. code-block:: bash

	# For Ubuntu 14.04/Debian 8 or later, where wwwroot is /var/www/html:
	mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql
	# For Debian 7 and Ubuntu 12.04, where wwwroot is /var/www:
	mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/clustercontrol/sql/dc-schema.sql

5) Clear the ClusterControl UI cache:

.. code-block:: bash

	# For Ubuntu 14.04/Debian 8 or later, where wwwroot is /var/www/html:
	rm -f /var/www/html/clustercontrol/app/tmp/cache/models/*
	# For Debian 7 and Ubuntu 12.04, where wwwroot is /var/www:
	rm -f /var/www/clustercontrol/app/tmp/cache/models/*

6) Restart the CMON controller service:

.. code-block:: bash

	sudo service cmon restart

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > General Settings > Version*. You should re-login if your ClusterControl UI session is active.

Automatic Upgrade
'''''''''''''''''

A helper script called ``s9s_upgrade_cmon`` is available from the `Severalnines Github repository <https://github.com/severalnines/s9s-admin>`_ to automate the ClusterControl upgrade process.

.. Important:: Port 9500 must be opened on the Controller. The Apache Server needs to connect to the Controller on localhost:9500 or 127.0.0.1:9500. 

To upgrade to the latest version, clone the Git repo in ClusterControl host:

.. code-block:: bash

	$ git clone https://github.com/severalnines/s9s-admin.git

If you already have that clone, it is very important you update it:

.. code-block:: bash

	$ cd s9s-admin
	$ git pull

Navigate to the ``s9s_upgrade_cmon`` script folder:

.. code-block:: bash

	$ cd s9s-admin/ccadmin

Start the upgrade process:

.. code-block:: bash

	./s9s_upgrade_cmon --latest


.. Note:: For sudoers, kindly run the command with 'sudo'.

The script will compare the current ClusterControl version with the latest version available at Severalnines repository and perform the upgrade in all hosts if necessary. Upgrading using this method will upgrade all ClusterControl related files including the CMON Controller, the CMON DB, the ClusterControl REST API (CMONAPI) and the ClusterControl UI.

For safety precaution, it will backup ClusterControl on every host (default backup path will be ``/$HOME/s9s_backup/s9s_backup_[date & time]``) before performing any upgrade. 

To force upgrade, even though the script detects existing ClusterControl version is already up-to-date, specify the ``--force`` (-f) flag:

.. code-block:: bash

	./s9s_upgrade_cmon --latest --force

By default, the upgrade will skip CMON and dcps databases backup as in some cases this behavior can eaten up a lot of disk space. You may instruct the script to perform the DB backup during the upgrade by using ``--backup-db`` flag (-n) and make sure you have sufficient free space beforehand:

.. code-block:: bash

 ./s9s_upgrade_cmon --latest --backup-db

.. Warning:: Due to various way of deployments, we have seen cases where the automatic upgrade does not fit in all environments. That's why we are standardizing the upgrade through package manager.

Offline Upgrade
```````````````

The following upgrade procedures can be performed without internet connection on ClusterControl node. You can get the ClusterControl packages from `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_.

Manual Upgrade
''''''''''''''

Redhat/CentOS
.............

1) Download the latest version of ClusterControl related RPM packages from `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

	wget -O clustercontrol-controller-1.2.10-752-x86_64.rpm http://www.severalnines.com/downloads/cmon/clustercontrol-controller-1.2.10-752-x86_64.rpm
	wget -O clustercontrol-1.2.10-407-x86_64.rpm http://www.severalnines.com/downloads/cmon/clustercontrol-1.2.10-407-x86_64.rpm
	wget -O clustercontrol-cmonapi-1.2.10-58-x86_64.rpm http://www.severalnines.com/downloads/cmon/clustercontrol-cmonapi-1.2.10-58-x86_64.rpm

2) Install via yum:

.. code-block:: bash

	yum localinstall clustercontrol-controller-1.2.10-727-x86_64.rpm clustercontrol-cmonapi-1.2.10-58-x86_64.rpm clustercontrol-1.2.10-407-x86_64.rpm

.. Note:: cmon-controller is a transition package for clustercontrol-controller. You can omit it from the command line if you previously didn't have cmon-controller package installed.

3) Upgrade the CMON database for ClusterControl controller. When performing an upgrade from an older version, it is compulsory to apply the SQL modification files relative to the upgrade. For example, when upgrading from version 1.2.6 to version 1.2.10, apply all SQL modification files between those versions in sequential order:

.. code-block:: bash

	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.6-1.2.7.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.7-1.2.8.sql
	mysql -f- h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.8-1.2.9.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.9-1.2.10.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_data.sql

.. Attention:: Importing ``cmon_db.sql`` and ``cmon_data.sql`` is required on each upgrade. Importing ``cmon_db_mods`` is relative to the previous version.

4) Upgrade the dcps database for ClusterControl UI:

.. code-block:: bash

	mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql

5) Clear the ClusterControl UI cache:

.. code-block:: bash

	rm -f /var/www/html/clustercontrol/app/tmp/cache/models/*

6) Restart the CMON controller service:

.. code-block:: bash

	sudo service cmon restart

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > General Settings > Version*. You should re-login if your ClusterControl UI session is active.

Debian/Ubuntu
.............

Even if you have previously installed from our tarball or .deb packages, it is possible to switch to apt repository and perform the upgrade.

1) Download the latest version of ClusterControl related DEB packages from `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_:

.. code-block:: bash

	wget -O clustercontrol-controller-1.2.10-752-x86_64.deb http://www.severalnines.com/downloads/cmon/clustercontrol-controller-1.2.10-752-x86_64.deb
	wget -O clustercontrol_1.2.10-407_x86_64.deb http://www.severalnines.com/downloads/cmon/clustercontrol_1.2.10-407_x86_64.deb
	wget -O clustercontrol-cmonapi_1.2.10-58_x86_64.deb http://www.severalnines.com/downloads/cmon/clustercontrol-cmonapi_1.2.10-58_x86_64.deb

2) Install via dpkg:

.. code-block:: bash

	dpkg -i clustercontrol-controller-1.2.10-752-x86_64.deb clustercontrol_1.2.10-407_x86_64.deb clustercontrol-cmonapi_1.2.10-58_x86_64.deb

.. Note:: cmon-controller is a transition package for clustercontrol-controller. You can omit it from the command line if you previously didn't have cmon-controller package installed.

3) Upgrade the CMON database for ClusterControl controller. When performing an upgrade from an older version, it is compulsory to apply the SQL modification files relative to the upgrade. For example, when upgrading from version 1.2.6 to version 1.2.10, apply all SQL modification files between those versions in sequential order:

.. code-block:: bash

	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.6-1.2.7.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.7-1.2.8.sql
	mysql -f- h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.8-1.2.9.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_db_mods-1.2.9-1.2.10.sql
	mysql -f -h127.0.0.1 -ucmon -p cmon < /usr/share/cmon/cmon_data.sql

.. Attention:: Importing ``cmon_db.sql`` and ``cmon_data.sql`` is required on each upgrade. Importing ``cmon_db_mods`` is relative to the previous version.

4) Upgrade the dcps database for ClusterControl UI:

.. code-block:: bash

	# For Ubuntu 14.04/Debian 8 or later, where wwwroot is /var/www/html:
	mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/html/clustercontrol/sql/dc-schema.sql
	# For Debian 7 and Ubuntu 12.04, where wwwroot is /var/www:
	mysql -f -h127.0.0.1 -ucmon -p dcps < /var/www/clustercontrol/sql/dc-schema.sql

5) Clear the ClusterControl UI cache:

.. code-block:: bash

	# For Ubuntu 14.04/Debian 8 or later, where wwwroot is /var/www/html:
	rm -f /var/www/html/clustercontrol/app/tmp/cache/models/*
	# For Debian and Ubuntu 12.04, where wwwroot is /var/www:
	rm -f /var/www/clustercontrol/app/tmp/cache/models/*

6) Restart the CMON controller service:

.. code-block:: bash

	sudo service cmon restart

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > General Settings > Version*. You should re-login if your ClusterControl UI session is active.

Backing Up ClusterControl
-------------------------

The upgrade script, ``s9s_upgrade_cmon`` (as described in Automatic Upgrade section) has ability to backup the existing ClusterControl prior to perform any upgrade. We can use the same script to backup ClusterControl by invoking following parameters:

.. code-block:: bash

	git clone https://github.com/severalnines/s9s-admin
	cd s9s-admin/ccadmin
	./s9s_upgrade_cmon --backup-db --backup all --backupdir [backup directory]

To backup ClusterControl manually, you can use your own method to copy or export following files:

CMON controller
```````````````

* CMON configuration file: ``/etc/cmon.cnf``
* CMON configuration directory and all its content: ``/etc/cmon.d/*``
* CMON cron file: ``/etc/cron.d/cmon``
* CMON init.d file: ``/etc/init.d/cmon``
* CMON logfile: ``/var/log/cmon.log`` or ``/var/log/cmon*``
* CMON helper scripts: ``/usr/bin/s9s_*``
* CMON database dump file:

.. code-block:: bash

	mysqldump -ucmon -p[mysql_password] -h[hostname] -P[mysql_port] cmon > cmon_dump.sql

ClusterControl UI
`````````````````

* ClusterControl upload directory: ``[wwwroot]/cmon*``
* ClusterControl CMONAPI: ``[wwwroot]/cmonapi*``
* ClusterControl UI: ``[wwwroot]/clustercontrol*``
* ClusterControl UI database dump file:

.. code-block:: bash

	mysqldump -ucmon -p[mysql_password] -h[hostname] -P[mysql_port] dcps > dcps_dump.sql

Where, ``[wwwroot]``, ``[mysql_password]``, ``[hostname]`` and ``[mysql_port]`` are values defined in CMON configuration file.


Restoring ClusterControl
------------------------

Automatic restoration can be performed by using ``s9s_upgrade_cmon`` helper script. It requires a backup generated by the corresponding script with ``--backup`` parameter, as described in the previous section. To restore, invoke the following parameters:

.. code-block:: bash

	./s9s_upgrade_cmon --restore all --backupdir [backup directory]

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
* HAproxy statistic page (if installed on ClusterControl node - default is 9600)
* MySQL load balance (if HAproxy installed on ClusterControl node - default is 33306)
* Streaming port for mysqldump through netcat (default is 9999)

SSH
```

SSH is very critical for ClusterControl. It must be possible to SSH from the ClusterControl server to the other nodes in the cluster without password, thus the database nodes must accept the SSH port configured in CMON configuration file. Following best practices are recommended:

* Permit a very few people in the organization to access to the servers. The fewer the better.
* Lock down SSH access so it is not possible to SSH into the nodes from any other server than the ClusterControl server.
* Lock down the ClusterControl server so that it is not possible to SSH into it directly from the outside world.


File Permission
```````````````

CMON configuration and log files contain sensitive information e.g ``mysql_password`` or ``sudo`` where it stores user’s password. Ensure CMON configuration file, e.g ``/etc/cmon.cnf`` has 700 while CMON log file, e.g ``/var/log/cmon.log`` has 740 and both are owned by root.

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

Running HTTP or HTTPS on custom port will change the ClusterControl UI and the CMONAPI URL e.g ``http://10.0.0.10:8080/clustercontrol`` and ``https://10.0.0.10:4433/cmonapi``. Thus, you may need to re-register the new CMONAPI URL for managed cluster at ClusterControl UI Cluster Registration page.

MySQL
`````

If you are running MySQL for CMON database on different ports, several areas need to be updated:

+-----------------------------------------+------------------------------------------------+-----------------------------------------+
| Area                                    | File                                           | Example                                 |
+=========================================+================================================+=========================================+
| CMON configuration file                 | ``/etc/cmon.cnf` or ``/etc/cmon.d/cmon_N.cnf`` | ``mysql_port=[custom_port]``            |
+-----------------------------------------+------------------------------------------------+-----------------------------------------+
| ClusterControl CMONAPI database setting | ``wwwroot/cmonapi/config/database.php``        | ``define('DB_PORT', '[custom_port]');`` |
+-----------------------------------------+------------------------------------------------+-----------------------------------------+
| ClusterControl UI database setting      | ``wwwroot/clustercontrol/bootstrap.php``       | ``define('DB_PORT', '[custom_port]');`` |
+-----------------------------------------+------------------------------------------------+-----------------------------------------+

.. Note:: Replace ``[wwwroot]`` with values defined inside CMON configuration file and ``[custom_port]`` with MySQL port.

HAproxy
```````

By default, HAproxy statistic page will be configured to run on port 9600. To change to another port, change following line in ``/etc/haproxy/haproxy.cfg``:

.. code-block:: bash

	listen admin_page 0.0.0.0:[your custom port]

Save and restart the HAproxy service.

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

The CMON Controller process has internal log rotation scheduling where it will log up to 5 MB in size before archiving ``/var/log/cmon.log``. The archived log will be named as ``cmon.log.1`` sequentially, with up to 9 archived log files (total of 10 log files rotation).

Migrating IP address or Hostname
--------------------------------

ClusterControl relies on proper IP address or hostname configuration. To migrate to a new set of IP address or hostname, please update the old IP address/hostname occurrences in following files:

* CMON configuration file: ``/etc/cmon.cnf`` and ``/etc/cmon.d/cmon_N.cnf`` (``hostname`` and ``mysql_hostname`` values)
* ClusterControl CMONAPI configuration file: ``[wwwroot]/cmonapi/config/bootstrap.php``
* HAproxy configuration file (if installed): ``/etc/haproxy/haproxy.cfg``

.. Note:: Replace ``[wwwroot]`` with value defined in CMON configuration file.

Next, revoke 'cmon' user privileges for old hosts on ClusterControl node and all managed database nodes:

.. code-block:: mysql

	REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'cmon'@'[old ClusterControl IP address or hostname]';

Then, grant cmon user with new IP address or hostname on ClusterControl node and all managed database nodes:

.. code-block:: mysql

	GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'[new ClusterControl IP address or hostname]' IDENTIFIED BY '[mysql password]' WITH GRANT OPTION;
	FLUSH PRIVILEGES;

Or, instead of revoke and re-grant, you can just simply update the MySQL user table:

.. code-block:: mysql

	UPDATE mysql.user SET host='[new IP address]' WHERE host='[old IP address]';
	FLUSH PRIVILEGES;

Restart CMON service to apply the changes:

.. code-block:: bash

	service cmon restart

Examine the output of the CMON log file to verify the IP migration status. The CMON Controller should report errors and shut down if it can not connect to the specified database hosts or the CMON database. Once the CMON Controller is started, you can remove the old IP addresses/hostname from the managed host list at *ClusterControl > Manage > Hosts*.

Standby ClusterControl Server for High Availability
---------------------------------------------------

It is possible to have several ClusterControl servers to monitor a single cluster. This is useful if you have a multi-datacenter cluster and you may need to have ClusterControl on the remote site to monitor and manage the alive cluster if connection between them goes down. However, you will need to disable auto recovery in configuration for the remote ClusterControl to avoid race conditions when recovering failed node or cluster.

Installing Standby Server
`````````````````````````

We have an automation script to install the standby ClusterControl host, built on top of our bootstrap script available at `Severalnines download site <http://www.severalnines.com/blog/installing-clustercontrol-standby-server>`_. On the standby host, do:

.. code-block:: bash

	wget http://severalnines.com/downloads/cmon/cc-bootstrap.tar.gz
	tar zxvf cc-bootstrap.tar.gz
	cd cc-bootstrap-*
	./s9s_bootstrap --install-standby

Follow the installation wizard, and it will guide you through the installation process. At the end of the installation, you should be able to access the standby ClusterControl UI at ``http://<standby_ClusterControl_IP\>/clustercontrol``.

At this point, you are having two ClusterControl servers monitoring the same cluster simultaneously. You can connect to any of these servers to perform usual tasks in ClusterControl UI. However both servers are totally independent to each other whereas you will have two distinct ClusterControl UI settings.

The primary ClusterControl server shall perform automatic recovery in case of node or cluster failure.

Failover Method
```````````````

If you want to enable automatic recovery on the standby server, comment or remove following line inside ``/etc/cmon.cnf`` or ``/etc/cmon.d/cmon_<cluster-id>.cnf``:

.. code-block:: bash

	#enable_autorecovery=0
	
Do not forget to restart cmon service after making changes on ``/etc/cmon.cnf``. You should notice that the standby server has taken over the primary role.

We have covered this in details in `this blog post <http://www.severalnines.com/blog/installing-clustercontrol-standby-server>`_.


Changing 'cmon' or 'root' Password
----------------------------------

ClusterControl has a helper script to change MySQL root password of your database cluster and for cmon database user called ``s9s_change_passwd``. It requires you to supply the old password so cmon user could access the database nodes and perform password update automatically. This tool is NOT intended for password reset.

On ClusterControl server, get :term:`s9s-admin tools` from our `Github repository <https://github.com/severalnines/s9s-admin>`_:

.. code-block:: bash

	git clone https://github.com/severalnines/s9s-admin.git

If you have already cloned s9s-admin, it's important for you to update it first:

.. code-block:: bash

	cd s9s-admin
	git pull

To change password for the 'cmon' user:

.. code-block:: bash

	cd s9s-admin/ccadmin
	./s9s_change_passwd --cmon -i1 -p <current cmon password> -n <new cmon password>

To change password for the 'root' user:

.. code-block:: bash

	cd s9s-admin/ccadmin
	./s9s_change_passwd --root -i1 -p <cmon password> -o <old root password> -n <new root password>

.. Warning:: The script only supports alpha-numeric characters. Special characters like "$!%?" will not work.

Uninstall
---------

If ClusterControl is installed on a dedicated host (i.e., not co-located with your application), uninstalling ClusterControl is pretty straightforward. It is enough to bring down the ClusterControl node and revoke the cmon user privileges from the managed database cluster:

.. code-block:: mysql

	REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'cmon'@'[ClusterControl address or hostname]';

If ClusterControl is installed through Severalnines repository, use following command to uninstall via respective package manager:

.. code-block:: bash

	yum remove -y clustercontrol clustercontrol-controller # Redhat/CentOS
	sudo apt-get remove -y clustercontrol clustercontrol-controller # Debian/Ubuntu

Else, to uninstall ClusterControl Controller manually in order to reuse the host for other purposes, kill the CMON process and remove all ClusterControl related files and databases:

.. code-block:: bash

	killall -9 cmon
	rm -rf /usr/sbin/cmon
	rm -rf /usr/bin/cmon*
	rm -rf /usr/bin/s9s_*
	rm -rf /usr/local/cmon*
	rm -rf /etc/init.d/cmon
	rm -rf /etc/cron.d/cmon
	rm -rf /var/log/cmon*
	rm -rf /etc/cmon*
	rm -rf [wwwroot]/cmon*
	rm -rf [wwwroot]/clustercontrol*
	rm -rf [wwwroot]/cc-*

For CMON and ClusterControl UI databases and privileges:

.. code-block:: mysql

	DROP SCHEMA cmon;
	DROP SCHEMA dcps;
	REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'cmon'@'[ClusterControl address or hostname]';
	REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'cmon'@'127.0.0.1';

.. Note:: Replace ``[wwwroot]`` with value defined in CMON configuration file.
