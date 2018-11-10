.. _Administration:

Administration
===============

This section provides detailed information on how to administer ClusterControl.

Upgrading ClusterControl
------------------------

There are several ways to upgrade ClusterControl to the latest version. However, we recommend users to perform `Online Upgrade`_ where the instuctions are mostly up-to-date.

Online Upgrade
+++++++++++++++

This is the recommended way to upgrade ClusterControl. The following upgrade procedures require internet connection on ClusterControl node.

RedHat/CentOS
``````````````

1) Setup the :ref:`Installation - Severalnines Repository - YUM Repository`.

2) Clear yum cache so it will retrieve the latest repository list and perform the upgrade:

.. code-block:: bash

	$ yum clean all
	$ yum install clustercontrol clustercontrol-cmonapi clustercontrol-controller clustercontrol-ssh clustercontrol-notifications clustercontrol-cloud clustercontrol-clud s9s-tools

3) Restart the ClusterControl services:

For sysvinit and upstart:

.. code-block:: bash

	$ service cmon restart
	$ service cmon-ssh restart
	$ service cmon-events restart
	$ service cmon-cloud restart

For systemd:

.. code-block:: bash

	$ systemctl daemon-reload
	$ systemctl restart cmon cmon-ssh cmon-events cmon-cloud

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > CMON Settings > Version* or by using command ``cmon -v``. You should re-login if your ClusterControl UI session is active.

Debian/Ubuntu
``````````````

1) Setup the :ref:`Installation - Severalnines Repository - APT Repository`.

2) Update the repository list and perform the upgrade:

.. code-block:: bash

	$ sudo apt-get update
	$ sudo apt-get install clustercontrol clustercontrol-cmonapi clustercontrol-controller clustercontrol-ssh clustercontrol-notifications clustercontrol-cloud clustercontrol-clud s9s-tools

3) Restart the ClusterControl services:

For sysvinit and upstart:

.. code-block:: bash

	$ service cmon restart
	$ service cmon-ssh restart
	$ service cmon-events restart
	$ service cmon-cloud restart

For systemd:

.. code-block:: bash

	$ systemctl daemon-reload
	$ systemctl restart cmon cmon-ssh cmon-events cmon-cloud

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > CMON Settings > Version* or by using command ``cmon -v``. You should re-login if your ClusterControl UI session is active.

Offline Upgrade
++++++++++++++++

The following upgrade procedures can be performed without internet connection on ClusterControl node. You can download the latest ClusterControl packages from `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_.

Manual Upgrade
``````````````

RedHat/CentOS
''''''''''''''

1) Download the latest version of ClusterControl related RPM packages from `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_, for example:

.. code-block:: bash

	$ wget https://severalnines.com/downloads/cmon/clustercontrol-1.7.0-5319-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cmonapi-1.7.0-333-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-controller-1.7.0-2859-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-notifications-1.7.0-153-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-ssh-1.7.0-66-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cloud-1.7.0-154-x86_64.rpm
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-clud-1.7.0-154-x86_64.rpm

2) Install using yum so dependencies are met:

.. code-block:: bash

	$ yum localinstall clustercontrol-*


3) Restart the ClusterControl services:

For sysvinit and upstart:

.. code-block:: bash

	$ service cmon restart
	$ service cmon-ssh restart
	$ service cmon-events restart
	$ service cmon-cloud restart

For systemd:

.. code-block:: bash

	$ systemctl restart cmon cmon-ssh cmon-events cmon-cloud

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > CMON Settings > Version*. You should re-login if your ClusterControl UI session is active.

Debian/Ubuntu
'''''''''''''

1) Download the latest version of ClusterControl related DEB packages from `Severalnines download site <http://www.severalnines.com/downloads/cmon/>`_, for example:

.. code-block:: bash

	$ wget https://severalnines.com/downloads/cmon/clustercontrol_1.7.0-5319_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cmonapi_1.7.0-333_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-controller-1.7.0-2859-x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-notifications_1.7.0-153_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-ssh_1.7.0-66_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-cloud_1.7.0-154_x86_64.deb
	$ wget https://severalnines.com/downloads/cmon/clustercontrol-clud_1.7.0-154_x86_64.deb

2) Upload the packages to the server and install them using dpkg command:

.. code-block:: bash

	$ sudo dpkg -i clustercontrol*.deb

3) Restart the ClusterControl services:

For sysvinit and upstart:

.. code-block:: bash

	$ service cmon restart
	$ service cmon-ssh restart
	$ service cmon-events restart
	$ service cmon-cloud restart

For systemd:

.. code-block:: bash

	$ systemctl restart cmon cmon-ssh cmon-events cmon-cloud

Upgrade is now complete. Verify the new version at *ClusterControl UI > Settings > CMON Settings > Version*. You should re-login if your ClusterControl UI session is active.

Backing Up ClusterControl
-------------------------

To backup ClusterControl manually, you can use your own method to copy or export following files:

ClusterControl CMON Controller
++++++++++++++++++++++++++++++

* CMON binary: ``/usr/sbin/cmon``
* CMON SSH binary: ``/usr/sbin/cmon-ssh``
* CMON Events binary: ``/usr/sbin/cmon-events``
* CMON Cloud binary: ``/usr/sbin/cmon-cloud`` and `/usr/sbin/clud`
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
+++++++++++++++++

* ClusterControl upload directory: ``{wwwroot}/cmon*``
* ClusterControl CMONAPI: ``{wwwroot}/cmonapi*``
* ClusterControl UI: ``{wwwroot}/clustercontrol*``
* ClusterControl UI database dump file:

.. code-block:: bash

	mysqldump -ucmon -p{mysql_password} -h{mysql_hostname} -P{mysql_port} dcps > dcps_dump.sql

Where, ``{wwwroot}`` is equal to the Apache document root and ``{mysql_password}``, ``{mysql_hostname}``, ``{mysql_port}`` are values defined in CMON configuration file.


Restoring ClusterControl
------------------------

Manual restoration can be performed by reverting the backup action and copying everything back to its original location. Restoration may require you to re-grant the 'cmon' user since the backup will not import the grant table of it. Please review the :ref:`Components - ClusterControl Controller - CMON Database` on how to grant the 'cmon' user cmon.

Securing ClusterControl
-----------------------

Firewall and Security Group
++++++++++++++++++++++++++++

Depending on the deployment options, ClusterControl might disable firewalls to minimize the possibilities of failure during the cluster deployment. Once the process is completed, it is important to secure the ClusterControl node and the database cluster. We recommend user to isolate their database infrastructure from the public Internet and just whitelist the known hosts or networks to connect to the database cluster.

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
+++

SSH is very critical for ClusterControl. It must be possible to SSH from the ClusterControl server to the other nodes in the cluster without password, thus the database nodes must accept the SSH port configured in CMON configuration file. Following best practices are recommended:

* Permit a very few people in the organization to access to the servers. The fewer the better.
* Lock down SSH access so it is not possible to SSH into the nodes from any other server than the ClusterControl server.
* Lock down the ClusterControl server so that it is not possible to SSH into it directly from the outside world.

File Permission
+++++++++++++++

CMON configuration and log files contain sensitive information e.g ``mysql_password`` or ``sudo`` where it stores user’s password. Ensure CMON configuration file, e.g ``/etc/cmon.cnf`` and ``/etc/cmon.d/cmon_[clusterid].cnf`` (if exists) have permission 700 while CMON log files, e.g ``/var/log/cmon.log`` and ``/var/log/cmon_[clusterid].log`` has 740 and both are owned by root.

HTTPS
++++++

By default, the installation script installs and configures a self-signed certificate for ClusterControl UI. You can access it by pointing your browser to :samp:`https://{ClusterControl_host}/clustercontrol`. If you would like to use your own SSL certificate (e.g :samp:`https://secure.domain.com/clustercontrol`), just replace the key and certificate path inside Apache’s SSL configuration file and restart Apache daemon. Make sure the server's hostname matches with the SSL domain name that you would like to use.

Running on Custom Port
----------------------

ClusterControl is configurable to support non-default port for selected services:

SSH
++++

ClusterControl requires same custom SSH port across all nodes in the cluster. Make sure the custom port number is defined correctly in ``ssh_port`` option at CMON configuration file, for example:

.. code-block:: bash

	ssh_port=55055

HTTP or HTTPS
+++++++++++++

Running HTTP or HTTPS on custom port will change the ClusterControl UI and the CMONAPI URL e.g :samp:`http://{ClusterControl_host}:8080/clustercontrol` and :samp:`https://{ClusterControl_host}:4433/cmonapi`. Thus, you may need to re-register the new CMONAPI URL for managed cluster at ClusterControl UI :ref:`Cluster Registration` page.

MySQL
++++++

If you are running MySQL for CMON database on different ports, several areas need to be updated:

+-----------------------------------------+--------------------------------------------------+-----------------------------------------+
| Area                                    | File                                             | Example                                 |
+=========================================+==================================================+=========================================+
| CMON configuration files                | ``/etc/cmon.cnf`` and ``/etc/cmon.d/cmon_N.cnf`` | ``mysql_port={custom_port}``            |
+-----------------------------------------+--------------------------------------------------+-----------------------------------------+
| ClusterControl CMONAPI database setting | ``{wwwroot}/cmonapi/config/database.php``        | ``define('DB_PORT', '{custom_port}');`` |
+-----------------------------------------+--------------------------------------------------+-----------------------------------------+
| ClusterControl UI database setting      | ``{wwwroot}/clustercontrol/bootstrap.php``       | ``define('DB_PORT', '{custom_port}');`` |
+-----------------------------------------+--------------------------------------------------+-----------------------------------------+

.. Note:: Where ``{wwwroot}`` is the Apache document root and ``{custom_port}`` is the MySQL custom port.

Housekeeping
------------

ClusterControl monitoring data will be purged based on the value set at *ClusterControl > Settings > General Settings > History* (default is 7 days). Some users might find this value to be too low for auditing purposes. You can increase the value accordingly however, the longer collected data exist in CMON database, the bigger space it needs. It is recommended to lower the disk space threshold under *ClusterControl > Settings > Thresholds > Disk Space Utilization* so you will get early warning in case CMON database grows significantly.

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

The CMON process has internal log rotation scheduling where it will log up to 5 MB in size before archiving ``/var/log/cmon.log`` and ``/var/log/cmon_{cluster id}.log``. The archived log will be named as ``cmon.log.1`` (or ``cmon_{cluster id}.log.1``) sequentially, with up to 9 archived log files (total of 10 log files rotation).

Migrating IP Address or Hostname
--------------------------------

ClusterControl relies on proper IP address or hostname configuration. To migrate to a new set of IP address or hostname, please update the old IP address/hostname occurrences in the following files:

* CMON configuration file: ``/etc/cmon.cnf`` and ``/etc/cmon.d/cmon_N.cnf`` (``hostname`` and ``mysql_hostname`` values)
* ClusterControl CMONAPI configuration file: ``{wwwroot}/cmonapi/config/bootstrap.php``
* HAProxy configuration file (if installed): ``/etc/haproxy/haproxy.cfg``

.. Note:: This section does not cover IP address migration of your database nodes. The easiest solution would be to remove the database cluster from ClusterControl UI using *Delete Cluster* and import it again by using *Import Existing Server/Cluster* in the deployment dialog.

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

Examine the output of the CMON log file to verify the IP migration status. The CMON Controller should report errors and shut down if it could not connect to the specified database hosts or the CMON database. Once the CMON Controller is started, you can remove the old IP addresses or hostnames from the managed host list at *ClusterControl > Manage > Hosts*.

Standby ClusterControl Server for High Availability
---------------------------------------------------

It is possible to have several ClusterControl servers to monitor a single cluster. This is useful if you have a multi-datacenter cluster and you may need to have ClusterControl on the remote site to monitor and manage the alive nodes if connection between them goes down. However, ClusterControl servers must be configured to be working in active-passive mode to avoid race conditions when digesting queries and recovering failed node or cluster.

In active mode, the ClusterControl node act as a primary controller, where it performs automatic recovery and parsing MySQL slow log query for query  monitoring. If The secondary ClusterControl node however must have the following things configured:

* Cluster/Node auto recovery must be turned off.
* Query sampling must be disabled (only if PERFORMANCE_SCHEMA is disabled on the database nodes).

Installing Standby Server
++++++++++++++++++++++++++

Steps described in this section must be performed on the secondary ClusterControl server.

1) Install ClusterControl as explained in the :ref:`Getting Started` page.

2) Add the same cluster via *ClusterControl > Import*. Ensure you choose "Enable Node AutoRecovery: No" and "Enable Cluster AutoRecovery: No" in the dialog box. Click "Add Cluster" to start the import job.

3) Once the cluster is imported, disable query sampling by going to *ClusterControl > Settings > Query Monitoring > Sampling Time = -1*.

Nothing should be performed on the primary side. The primary ClusterControl server shall perform automatic recovery in case of node or cluster failure.

Failover Method
++++++++++++++++

If you want to make the standby server runs in the active mode, just do as follow (assume the primary ClusterControl is unreachable at the moment):

* Cluster/Node auto recovery must be turned on. Click on both red power icons in the summary bar until they appear in green color.
* Enable query sampling. Go to *ClusterControl > Settings > Query Monitor* and change "Sampling Time" to other than "-1".

That's it. At this point, the standby server has taken over the primary role.

.. Attention:: Do not let two or more ClusterControl instances perform automatic recovery to the same cluster at one time. 

Changing 'cmon' or 'root' Password
----------------------------------

For MySQL-based clusters, ClusterControl requires two database users, 'cmon' and 'root' with full privileges and grant option. Most of the time, ClusterControl will use 'cmon' user for monitoring, management and maintenance operations. However, there are some operations which only 'root' user is capable of performing like granting 'cmon' user for the first time after restoration and scaling up a new database nodes. 

In ClusterControl context, both 'cmon' and 'root' users are immutable. The password is defined under ``mysql_password`` variable for 'cmon', while for the MySQL root password should be defined under ``monitored_mysql_root_password`` variable. The variable ``mysql_password`` exists in every cluster's CMON configuration file, located under ``/etc/cmon.d`` directory. Thus, if you have 5 clusters managed by this ClusterControl instance, you need to update the ``mysql_password`` variable for 6 times (1 inside ``/etc/cmon.cnf`` + 5 inside ``/etc/cmon.d/cmon_*.cnf``), as shown in `Changing cmon Password`_.

Changing cmon Password
++++++++++++++++++++++

.. Note:: Before changing the 'cmon' database user password, you must know the MySQL root password for the ClusterControl node and all of the database nodes.

The steps are:

1) Stop ClusterControl Controller (CMON) service:

.. code-block:: bash

	$ systemctl stop cmon # systemd
	$ service cmon stop # sysvinit

2) Update the 'cmon' user password on ClusterControl node. Retrieve the userhost information for 'cmon' beforehand:

.. code-block:: mysql

  mysql> SELECT user,host FROM mysql.user WHERE user = 'cmon';
	+------+------------+
	| user | host       |
	+------+------------+
	| cmon | 10.0.0.156 |
	| cmon | 127.0.0.1  |
	| cmon | localhost  |
	+------+------------+

Then update the 'cmon' password for every host accordingly:

.. code-block:: mysql

	mysql> SET PASSWORD for 'cmon'@'10.0.0.156' = PASSWORD('&5?2+SW9bGq');
	mysql> SET PASSWORD for 'cmon'@'127.0.0.1' = PASSWORD('&5?2+SW9bGq');
	mysql> SET PASSWORD for 'cmon'@'localhost' = PASSWORD('&5?2+SW9bGq');

3) Update the 'cmon' user password on all monitored MySQL nodes. Retrieve the userhost information for 'cmon' beforehand:

.. code-block:: mysql

	mysql> SELECT user,host FROM mysql.user WHERE user = 'cmon';
	+------+------------+
	| user | host       |
	+------+------------+
	| cmon | 10.0.0.156 |
	| cmon | cc.local   |
	+------+------------+

When updating the password, run the following statements on the correct node depending on the cluster type:

* On one of the MySQL node for Galera Cluster.
* On the master server for MySQL Replication.
* On the primary node for MySQL Group Replication.
* On all MySQL API nodes for MySQL Cluster (NDB).

.. code-block:: mysql

	mysql> SET PASSWORD for 'cmon'@'10.0.0.156' = PASSWORD('&5?2+SW9bGq');
	mysql> SET PASSWORD for 'cmon'@'cc.local' = PASSWORD('&5?2+SW9bGq');


4) Edit the value of ``mysql_password`` variables inside all ClusterControl related files:

* CMON main configuration file: ``/etc/cmon.cnf`` under ``mysql_password`` variable.
* Cluster configuration file: ``/etc/cmon.d/cmon_*.cnf`` under ``mysql_password`` variable.
* ClusterControl UI configuration file: ``/var/www/html/clustercontrol/bootstrap.php`` under ``DB_PASS`` constant.
* ClusterControl CMONAPI configuration file: ``/var/www/html/cmonapi/config/database.php`` under ``DB_PASS`` constant.

The following output show the post-edited value when filtering out the password variables:

.. code-block:: mysql

	$ cat /etc/cmon.cnf | grep ^mysql_password
	mysql_password='&5?2+SW9bGq'
	$ cat /etc/cmon.d/cmon_1.cnf | grep ^mysql_password # Galera Cluster
	mysql_password='&5?2+SW9bGq'
	$ cat /etc/cmon.d/cmon_2.cnf | grep ^mysql_password # MySQL Replication
	mysql_password='&5?2+SW9bGq'
	$ cat /var/www/html/clustercontrol/bootstrap.php | grep DB_PASS
	define('DB_PASS', '&5?2+SW9bGq');
	$ cat /var/www/html/cmonapi/config/database.php | grep DB_PASS
	define('DB_PASS', '&5?2+SW9bGq');

.. Note:: If you don't like repetition, you can use Linux replace tool like ``sed`` to do the job in one shot.

5) Start ClusterControl Controller (CMON) service:

.. code-block:: bash

	$ systemctl start cmon # systemd
	$ service cmon start # sysvinit

Verify if CMON starts correctly by looking at the ``/var/log/cmon.log`` or ``/var/log/cmon_*.log``.

Changing MySQL Root Password of your Database Server/Cluster
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

ClusterControl requires a working MySQL root password for management, restoration and granting purposes. It is not necessary for MySQL root user to be granted with remote access because all operations involving this user will be executed via SSH locally. Use ``monitored_mysql_root_password`` value to set the correct password in the CMON configuration file for the respective cluster.

The basic steps involve changing the MySQL root password, update ``monitored_mysql_root_password`` variable in the respective CMON configuration file and restart the ClusterControl (CMON) service. For example, supposed we are having 3 MySQL-based clusters managed by a single ClusterControl server (10.0.0.156) and we would like to update the MySQL root password on all clusters, with the following cluster ID and configuration file:

========================= == ==================
Cluster                   ID Configuration File
========================= == ==================
MariaDB Galera Cluster    1  /etc/cmon.d/cmon_1.cnf
MySQL Replication         2  /etc/cmon.d/cmon_2.cnf
MySQL Cluster (NDB)       3  /etc/cmon.d/cmon_3.cnf
========================= == ==================

1) Update the MySQL root user password on all monitored MySQL nodes. Retrieve the userhost information for root beforehand:

.. code-block:: mysql

	mysql> SELECT user,host FROM mysql.user WHERE user = 'root';
	+------+------------+
	| user | host       |
	+------+------------+
	| root | localhost  |
	| root | ::1        |
	| root | 127.0.0.1  |
	+------+------------+


When updating the password, run the following statements on the correct node depending on the cluster type:

* On one of the MySQL node for Galera Cluster.
* On the master server for MySQL Replication.
* On the primary node for MySQL Group Replication.
* On all MySQL API nodes for MySQL Cluster (NDB).

.. code-block:: mysql

	mysql> SET PASSWORD for 'root'@'localhost' = PASSWORD('&5?2+SW9bGq');
	mysql> SET PASSWORD for 'root'@'::1' = PASSWORD('&5?2+SW9bGq');
	mysql> SET PASSWORD for 'root'@'127.0.0.1' = PASSWORD('&5?2+SW9bGq');

2) Edit the value of ``monitored_mysql_root_password`` variable inside the respective CMON configuration for that particular cluster ID. For example, if we changed the root password for cluster ID 2, we would need to update the value inside ``/etc/cmon.d/cmon_2.cnf`` accordingly. The following output show the post-edited value when filtering out the password variables for all clusters (assuming we have changed the MySQL root password on all clusters to '&5?2+SW9bGq'):

.. code-block:: mysql

	$ cat /etc/cmon.d/cmon_1.cnf | grep ^monitored_mysql_root_password
	monitored_mysql_root_password='&5?2+SW9bGq'
	$ cat /etc/cmon.d/cmon_2.cnf | grep ^monitored_mysql_root_password
	monitored_mysql_root_password='&5?2+SW9bGq'
	$ cat /etc/cmon.d/cmon_3.cnf | grep ^monitored_mysql_root_password
	monitored_mysql_root_password='&5?2+SW9bGq'

3) Restart ClusterControl Controller (CMON) service to load the changes:

.. code-block:: bash

	$ systemctl restart cmon # systemd
	$ service cmon restart # sysvinit


Graceful Shutdown
-----------------

In testing environment, you might need to perform a shutdown on ClusterControl and monitored database hosts in a graceful way. Depending on the clustering technology, the order of startup and shutting down is vital to keep the whole cluster in sync and ensure smooth start up operation in the future.

It's recommended to let the ClusterControl node to be the last one to shutdown, since it needs to oversee the state of the monitored hosts and saves it into CMON database. When starting up the database cluster at the later stage, ClusterControl will perform a proper start-up procedure based on the last known state of the monitored hosts.

ClusterControl needs to know whether the database cluster that you are shutting down was shutdown outside of ClusterControl domain. Therefore, the proper steps to shutdown the database hosts are:

MySQL Replication
+++++++++++++++++

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
+++++++++++++++++

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
+++++++++++++++++++++

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
+++++++++++++++++++

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
++++++++++++++++++++++++

1. Shutdown the application manually. This usually outside of ClusterControl domain.
2. Shutdown the slaves by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
3. Shutdown the master by using *ClusterControl > Nodes > pick the server > Shutdown Node > Execute*.
4. Shutdown ClusterControl host by doing system shut down.

Starting up:

1. Start ClusterControl host. Ensure you are able to connect to the UI and see the last state of the database cluster.
2. Start the master. If auto recover is turned on, this master will be started automatically.
3. Once the master is started, start the remaining slaves. 
4. Start the application manually. This usually outside of ClusterControl domain.

.. Note:: If the database server was being shutdown gracefully outside of ClusterControl knowledge (through command line init script, systemd or ``kill -15``), ClusterControl would still attempt to recover the database node if *Node AutoRecovery* is turned on. Unless, the node is marked as 'Under Maintenance'.

Uninstall
---------

If ClusterControl is installed on a dedicated host (i.e., not co-located with your application), uninstalling ClusterControl is pretty straightforward. It is enough to bring down the ClusterControl node and revoke the cmon user privileges from the managed database nodes:

.. code-block:: mysql

	mysql> DELETE FROM mysql.user WHERE user = 'cmon';
	mysql> FLUSH PRIVILEGES;

Before removing the package, stop all cmon related services:

On systemd:

.. code-block:: bash

	$ systemctl stop cmon cmon-ssh cmon-events cmon-cloud

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

Else, to uninstall ClusterControl Controller manually so you can re-use the host for other purpose, kill the CMON process and remove all ClusterControl related files and databases:

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

.. Note:: Replace ``{wwwroot}`` with value defined in CMON configuration file.

For CMON and ClusterControl UI databases and privileges:

.. code-block:: mysql

	mysql> DROP SCHEMA cmon;
	mysql> DROP SCHEMA dcps;
	mysql> DELETE FROM mysql.user WHERE user = 'cmon';
	mysql> FLUSH PRIVILEGES;

