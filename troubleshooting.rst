.. _Troubleshooting:

Troubleshooting
===============

This troubleshooting guide provides detailed information on how to troubleshoot ClusterControl and all of its components namely controller, CMON database, UI, notifications, clouds, web-based SSH and also CMONAPI. The document provides instructions on what data to collect when creating error reports to be submitted to `Severalnines Support <https://support.severalnines.com>`_.

Note that this troubleshooting guide only covers the latest version of ClusterControl. We highly advise you to upgrade to the latest version before troubleshooting any kind of issues related to ClusterControl due to the following reasons:

- ClusterControl has a short release cycle if compared to other software application (new major release every quarter of the year).
- We usually release a new maintenance patch every Friday (if there is). See :ref:`Changelog`.
- ClusterControl has to keep up with all the latest changes and modifications introduced by supported database and application vendors.
- Some of the issues you encountered probably have been fixed in the latest weekly maintenance or latest major release.

To upgrade ClusterControl to the latest version, see :ref:`Administration - Upgrading ClusterControl`.

.. _Troubleshooting - Logging:

Logging
-------

ClusterControl consists of a number of components which write to their own log file. These files reside on the ClusterControl node. By default, CMON and ClusterControl UI run without the debug option enabled. Please refer to `Reporting and Debugging`_ section on how to get them run in debug mode.

If you encounter any problems with ClusterControl, it is highly recommended to examine the related log files:

+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| Log Type                      |                                          Default Location                                                   |
|                               +------------------------------------------------------+------------------------------------------------------+
|                               | RedHat/CentOS                                        | Debian/Ubuntu                                        |
+===============================+======================================================+======================================================+
| CMON process log              | /var/log/cmon.log or /var/log/cmon_[cluster_id].log  | /var/log/cmon.log or /var/log/cmon_[cluster_id].log  |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| CMON cloud log                | /var/log/cmon-cloud.log                              | /var/log/cmon-cloud.log                              |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| CMON events log               | /var/log/cmon-events.log                             | /var/log/cmon-events.log                             |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| ClusterControl UI error log   | /var/www/html/clustercontrol/app/tmp/logs/error.log  | /var/www/clustercontrol/app/tmp/logs/debug.log       |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| ClusterControl UI debug log   | /var/www/html/clustercontrol/app/tmp/logs/debug.log  | /var/www/clustercontrol/app/tmp/logs/debug.log       |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| ClusterControl LDAP log       | /var/www/html/clustercontrol/app/log/cc-ldap.log     | /var/www/clustercontrol/app/log/cc-ldap.log          |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| Apache error log              | * /var/log/httpd/ssl_error_log                       | /var/log/apache2/error.log                           |
|                               | * /var/log/httpd/error_log                           |                                                      |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| Apache access log             | * /var/log/httpd/access_log                          | * /var/log/apache2/access.log                        |
|                               | * /var/log/httpd/ssl_access_log                      | * /var/log/apache2/ssl_access.log                    |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+

Output of CMON process log per cluster is also accessible directly from *ClusterControl > pick a cluster > Logs > CMON Logs*.

.. _Troubleshooting - Reporting and Debugging:

Reporting and Debugging
-----------------------

If the above does not help identify the root cause of the issues, please refer to our `knowledge base <http://support.severalnines.com/categories/20019191-Knowledge-Base>`_ or contact us via our Support Portal by creating a support request at http://support.severalnines.com/tickets/new. We encourage you to make use of our error reporting tool as described in the next section.

.. _Troubleshooting - Reporting and Debugging - Error Report:

Error Report
++++++++++++

ClusterControl provides an error reporting tool called ``s9s_error_reporter``. This can greatly facilitate the troubleshooting process as it collects the necessary information on the entire database cluster setup and archives it in a compressed package. Use this tool to generate an error report and then attach the generated tar ball package when creating a `Severalnines Support Ticket <http://support.severalnines.com>`_.

To generate an error report for one particular cluster, simply go to *ClusterControl > pick a cluster > Logs > Error Reports > Create an Error Report*. Once the job completes, the error report package will be listed in this page. You can then download it into your workstation and use it for debugging and troubleshooting purposes, or attach it to alongside your `Severalnines Support Ticket <http://support.severalnines.com>`_. See :ref:`MySQL - Logs - Error Reports` for details.

You can also use the helper script to generate an error report via command line interface. Run the following command on ClusterControl node:

.. code-block:: bash

	# as root:
	s9s_error_reporter -i 1
	# or as a non-root user:
	sudo s9s_error_reporter -i 1 

Where, 1 is the cluster ID of the corresponding cluster. For other cluster, just change the cluster ID accordingly. The cluster ID can be retrieved from :ref:`UserGuide - Database Cluster List` or by looking into CMON configuration file for the respective cluster inside ``/etc/cmon.d``. If you have no cluster managed by ClusterControl, you might want to specify 0 instead, where 0 has a special meaning in ClusterControl - collect information about ClusterControl installation, configurations and logs on the local node only.

.. code-block:: bash

	# as root:
	s9s_error_reporter -i 0

At the end of the execution, it will print out something like this:

.. code-block:: bash

	Executing tar -C /var/tmp/cmon-000809-42b7c3b595d5282a -czf '/var/www/html/clustercontrol/app/tmp/logs/error-report-2018-11-19_065637-cluster0.tar.gz' error-report-2018-11-19_065637-cluster0
	Please attach /var/www/html/clustercontrol/app/tmp/logs/error-report-2018-11-19_065637-cluster0.tar.gz to the support issue.

Attach the generated tar ball to your support issue. Sometimes, the generated tar ball might get too big to be uploaded into our support system, where the attachment limit is 20MB in size. You probably want to use cloud storage services like Google Drive, Dropbox or WeTransfer to upload the error report there and share the download link in the support ticket. Please note that error report might contain sensitive and confidential information. Restrict the file from public access and only share with us the HTTPS download URL.

.. Note:: We also recommend you to take screenshots showing the area of the problem, e.g, the Overview page from the UI allows us to understand the current state of nodes and clusters.

.. _Troubleshooting - Reporting and Debugging - Debugging ClusterControl Controller (CMON):

Debugging ClusterControl Controller (CMON)
++++++++++++++++++++++++++++++++++++++++++

Starting from ClusterControl v1.3.0, ClusterControl comes with a debuginfo package. In case if you encounter CMON crash, please install the debuginfo package and the necessary packages as shown below.

Install Debugging Components (RedHat/CentOS)
````````````````````````````````````````````

1. Enable the debug repo under ``/etc/yum.repos.d/CentOS-Debuginfo.repo`` and set ``enabled=1``.

2. Install yum utilities:

.. code-block:: bash

    yum -y install yum-utils

3. Install ClusterControl debuginfo and gdb:

.. code-block:: bash

    yum -y install clustercontrol-controller-debuginfo gdb

4. Then, run:

.. code-block:: bash

    debuginfo-install clustercontrol-controller

Install Debugging Components (Debian/Ubuntu)
````````````````````````````````````````````

1. Install ClusterControl debuginfo package and gdb:

.. code-block:: bash

    apt-get install clustercontrol-controller-dbg gdb

Optionally, you can 

2. Install the debugging components' library:

.. code-block:: bash

    apt-get install libstdc++6-4.8-dbg libc6-dbg

However, this totally depends on the libstdc++6 version installed. Print the shared object dependencies using ``ldd``:

.. code-block:: bash

    ldd /usr/sbin/cmon | grep libstdc
	    libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007ff508001000)

Based on the library path, locate the package name that provides this library:

.. code-block:: bash

    dpkg -S /usr/lib/x86_64-linux-gnu/libstdc++.so.6
    libstdc++6:amd64: /usr/lib/x86_64-linux-gnu/libstdc++.so.6

Then, find the package's version:

.. code-block:: bash

    dpkg -l | grep libstdc++6
    ii  libstdc++6:amd64                  4.9.2-10                     amd64        GNU Standard C++ Library v3

In this case, we have version "4.9" installed for libstc++6. Finally, install the corresponding debug packages:

.. code-block:: bash

    apt-get install gdb libc6-dbg libstdc++-6-4.9-dbg  


Debugging Steps
```````````````

Debugging is a program that produces a core dump. It consists of the recorded state of the working memory of a computer program at a specific time, generally when the program has crashed or otherwise terminated abnormally. ClusterControl Controller (CMON) package comes with a cron file installed under ``/etc/cron.d/`` which will auto-restart if the cmon process is terminated abnormally. Typically, you may notice if cmon process has crashed by looking at the ``dmesg`` output.

In such cases, generating a core dump is the only way to backtrace the issue. Make sure you have the debugging components installed as described in the previous section beforehand. On ClusterControl node as root user, increase the CPU limit, adjust kernel's core pattern value and run CMON on foreground:

.. code-block:: bash

    ulimit -c unlimited
    echo "/tmp/core.%e.%p.%h.%t" > /proc/sys/kernel/core_pattern
    cmon

When cmon crashes there will now be a core file in ``/tmp``. Compress the core dump (gzip is recommended) and attach it to a support ticket so we can take a look and perform necessary fix. Alternatively, you can send only the backtrace in a support ticket by using following command:

.. code-block:: bash

    gdb /usr/sbin/cmon /tmp/<corefile>
    thread apply all bt full


Attach the full output and potentially replace sensitive information with "XXXXXXXXX". Traces may contain password information.

CMON on Foreground
+++++++++++++++++++

If you would like to run cmon as foreground process, you can do that by invoking ``-d`` option:

.. code-block:: bash

	$ service cmon stop
	$ CMON_DEBUG=1 cmon -d

CMON will enable LOG_DEBUG messages and print detailed information on the screen (stdout) as well as ``/var/log/cmon.log`` or ``/var/log/cmon_{cluster ID}.log``. Press ``Ctrl + C`` to terminate the process. In some cases, this type of CMON output might be needed to get insight of the problem.

Debugging ClusterControl UI
+++++++++++++++++++++++++++

To enable ClusterControl UI debug, SSH into the ClusterControl node and modify the following values inside ``{wwwroot}/clustercontrol/app/Config/core.php``:

.. code-block:: php

	Configure::write('debug', 0);

Where,

* 0: Production mode. All errors and warnings are suppressed.
* 1: Errors and warnings shown, model caches refreshed, flash messages halted.
* 2: As in 1, but also with full debug messages and SQL output.

Make sure ``{wwwroot}/clustercontrol/app/tmp`` has write permission and is owned by Apache user for the debug and error log to be generated.

.. _Troubleshooting - Common Issues:

Common Issues
-------------

This section covers common issues when configuring and dealing with ClusterControl components, with possible troubleshooting steps and solutions. There is also a `community forum <https://support.severalnines.com/hc/en-us/community/topics>`_ available with knowledge base sections for public reference.

If you need further assistance, please contact us via our support channel by `submitting a support request <https://support.severalnines.com/hc/en-us/requests/new>`_ or post a new thread in `our community help forum <https://support.severalnines.com/hc/en-us/community/topics/200447583-Community-Help>`_.


ClusterControl Installation
+++++++++++++++++++++++++++

This section covers common issues encountered during ClusterControl installation and the installer script.

Failed to start MySQL Server during ClusterControl installation
````````````````````````````````````````````````````````````````

**Description:**

During installation, the installer script fails to start the MySQL/MariaDB server on ClusterControl host and returns the following lines:

.. code-block:: bash

	=> Starting database. This may take a couple of minutes. Do NOT press any key. 
	mysqld: unrecognized service 
	=> Failed to start the MySQL Server. ... 
	Please contact Severalnines support at http://support.severalnines.com if you have installation issues that cannot be resolved.

**Troubleshooting steps:**

1) Examine the MySQL error log on possible reasons why does MySQL fail to start. Typically, the log file is located under ``/var/log/mysqld.log`` or ``/var/lib/mysql/error.log``.
2) Try starting the MySQL server manually by using ``systemctl`` or ``service`` command.

**Solutions:**

If you are running on older operating system, ClusterControl might not support the distribution and some issues are expected to show up. See :ref:`Requirements` for details. Once the MySQL server is up and running, restart the ClusterControl installer script again:

.. code-block:: bash

	$ ./install-cc


ClusterControl Controller (CMON)
++++++++++++++++++++++++++++++++

This section covers common issues encountered related to ClusterControl Controller (CMON).

CMON unable to restart MySQL using service command
````````````````````````````````````````````````````

**Description:**

When scheduling a start/restart job, ClusterControl fails to start the node with error "Command not found".

**Example error:**

.. code-block:: bash

	galera1.domain.com: Starting mysqld failed: Error: Command not found (host: galera1.domain.com): service mysql restart 
	galera1.domain.com: Starting mysqld

**Troubleshooting steps:**

1. SSH into the DB node and check the user's environment path variable:

.. code-block:: bash

	ssh -tt -i /home/admin/.ssh/id_rsa admin@galera1.domain.com "sudo env | grep PATH"
	PATH=/usr/local/bin:/bin:/usr/bin

2. Look at the PATH output.

**Solution:**

- Ensure the ``/sbin`` path is there. Otherwise, ClusterControl won't be able to automatically locate and run the "service" command.
- If the ``/sbin`` path is not listed in the PATH, add it by using the following command:
	
.. code-block:: bash

	PATH=$PATH:/sbin 
	export PATH

- However, the above won't persist if the user logs out from the terminal. To make it persistent, add those lines into ``/home/{SSH user}/.bash_profile`` or ``/home/{SSH user}/.bashrc``


CMON always tries to recover failed database nodes during my maintenance window.
````````````````````````````````````````````````````````````````````````````````

**Description:**

By default, CMON is configured to perform recovery of failed nodes or clusters. This behavior can be overridden by disabling automatic recovery or enabling maintenance mode for the node.

**Solution:**

1) Enable maintenance mode for selected nodes (recommended). To enable maintenance window, go to *Nodes > select the node > Schedule Maintenance Mode > Enable*. You have to specify the reason and duration of maintenance window. During this period, any alarms and notifications raised for this node will be disabled.
2) Disabling automatic recovery. To disable automatic recovery temporarily, you can just click on the 'power' icon for node and cluster. Red means automatic recovery is turned off while green indicates recovery is turned on. This behavior will not persistent if CMON is restarted. To make the above change persistent, disable node or cluster auto recovery by specifying following line inside CMON configuration file of respective cluster. For example, if you want to disable automatic recovery for cluster ID 1, inside ``/etc/cmon.d/cmon_1.cnf``, set the following line:

.. code-block:: bash

	enable_autorecovery=0


CMON process dies with "Critical error (mysql error code 1)"
````````````````````````````````````````````````````````````

**Description:**

After starting CMON service, it stops and ``/var/log/cmon.log`` shows the following error:

.. code-block:: bash

	(ERROR) Critical error (mysql error code 1) occurred - shutting down

**Troubleshooting steps:**

1) Run the following command on the ClusterControl host to check if it has the ability to connect to the DB host with current credentials:

.. code-block:: bash

	$ mysql -ucmon -p -h[database node IP] -P[MySQL port] -e 'SHOW STATUS'

2) Check GRANT for cmon user on each database host:

.. code-block:: mysql

	mysql> SHOW GRANTS FOR 'cmon'@'[ClusterControl IP address]';


**Solution:**

It is not recommended to mix public IP address and internal IP address. For the GRANT, try to use the IP address that your database nodes use to communicate with each other. If the ``SHOW STATUS`` statement returns ``ERROR 1130 (HY000): Host '[ClusterControl IP address]' is not allowed to connect to this``, the database host is missing the cmon user grant. Run following command to reset the cmon user privileges:

.. code-block:: mysql

	mysql> GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'[ClusterControl IP]' IDENTIFIED BY '[cmon password]' WITH GRANT OPTION; 
	mysql> FLUSH PRIVILEGES;
	
Where, [ClusterControl IP] is ClusterControl IP address and [cmon password] is ``mysql_password`` value inside CMON configuration file.

Job fails with 'host is already in an other cluster' error
````````````````````````````````````````````````````````````

**Description:**

When deploying a new node, or adding a node into an existing cluster managed by ClusterControl, the deployment fails with the following error:

``"Host 1.2.3.4:nnnn is already in an other cluster."``

**Solution:**

A host can only exist in one cluster at a time. Check if you have an ``/etc/cmon.d/cmon_X.cnf`` file (where X is an integer) that contains the hostname and remove the file if the corresponding cluster does no exist in the UI (be careful to not remove the wrong cmon_X.cnf file):

.. code-block:: bash

	$ rm /etc/cmon.d/cmon_X.cnf

Otherwise, delete the host from ``server_node``, ``mysql_server``, and ``hosts`` tables:

.. code-block:: mysql

	mysql> DELETE FROM cmon.server_node WHERE hostname='1.2.3.4';
	mysql> DELETE FROM cmon.mysql_server WHERE hostname='1.2.3.4';
	mysql> DELETE FROM cmon.hosts WHERE hostname='1.2.3.4';

Restart CMON to load the new changes:

.. code-block:: bash

	$ service cmon restart

You may have to execute the deletion several times for each hostname/IP of the cluster you are trying to add.

ClusterControl UI
+++++++++++++++++

This section covers common issues encountered related to ClusterControl UI.

Cluster details cannot be retrieved. Please check the CMON process status (service cmon status)
````````````````````````````````````````````````````````````````````````````````````````````````

**Description:**

When listing out the database cluster, ClusterControl reports the following:

.. topic:: Error Message

	Cluster details cannot be retrieved. Please check the CMON process status (service cmon status). Also, ensure the dcps.apis token matches the rpc_key in /etc/cmon.cnf.

Additionally, ClusterControl UI shows a toaster notification (on the top right of the UI) indicating that it has authentication problem to connect to cluster 0 (0 means global view of clusters under ClusterControl management):

.. topic:: Error Message

	Authentication required on '/0/auth'

**Troubleshooting steps:**

Retrieve the value of global token inside ``/etc/cmon.cnf``, ``/var/www/html/clustercontrol/bootstrap.php`` and ``/var/www/html/cmonapi/config/bootstrap.php``:

.. code-block:: bash

	$ grep rpc_key /etc/cmon.cnf
	$ grep RPC_TOKEN /var/www/html/clustercontrol/bootstrap.php
	$ grep CMON_TOKEN /var/www/html/cmonapi/config/bootstrap.php

**Solutions:**

Verify that the ``RPC_TOKEN`` value in ``/var/www/html/clustercontrol/bootstrap.php`` and ``CMON_TOKEN`` value in ``/var/www/html/cmonapi/config/bootstrap.php`` match the token defined as ``rpc_key`` in ``/etc/cmon.cnf``. If you manipulate ``/etc/cmon.cnf`` directly, you must restart cmon for the change to take effect.

Database connection "Mysql" is missing, or could not be created
````````````````````````````````````````````````````````````````

**Description:**

When opening ClusterControl UI on the browser, ClusterControl shows the following error:

.. topic:: Error Message

	Error Details 
	Database connection "Mysql" is missing, or could not be created.
	An Internal Error Has Occurred.

**Troubleshooting steps:**


1) Verify the installed PHP version:

.. code-block:: bash

	$ php --version

2) Verify if php-mysql is installed:

.. code-block:: bash

	$ rpm -qa | grep -i php-mysql # RHEL/CentOS
	$ dpkg -l | egrep php.*mysql # Ubuntu/Debian
	
3) Verify if MySQL/MariaDB is running:

.. code-block:: bash

	$ ps -ef | grep -i mysql
	

**Solution:**

ClusterControl requires php-mysql package to be installed together a running MySQL server. See :ref:`Requirements`. In some cases, php-mysqlnd package was installed (due to phpMyAdmin dependencies) and this would cause ClusterControl UI fails to establish connection to the MySQL server using the standard mysql calls. Also, custom package repository could also install a different version of PHP that we would expected. Use the OS's default package repository is highly recommended during the installation.

Authentication required on '/{cluster_id}/auth'
```````````````````````````````````````````````

**Description:**

The ClusterControl UI shows a toaster notification (on the top right of the UI) indicating that it has authentication problem to connect to a specific cluster ID.

**Troubleshooting steps:**

Run the following command to verify if token is set correctly for corresponding cluster:

.. code-block:: mysql

	mysql> SELECT cluster_id, token FROM dcps.clusters;
	
**Solution:**

In this case you need to update the token column in ``dcps.clusters`` table for the ``cluster_id={ID}`` so it matches the ``rpc_key`` in ``/etc/cmon.d/cmon_{ID}.cnf``. These tokens must match. Execute the following update query on the "dcps" database:

.. code-block:: mysql

	mysql> UPDATE dcps.clusters SET token='[rpc_key]' WHERE cluster_id=[ID];


Unable to authenticate to LDAP server
``````````````````````````````````````

**Description:**

Unable to login to ClusterControl using the LDAP user after *LDAP Settings* is configured.

**Troubleshooting steps:**

1) Make sure that you have mapped ClusterControl's Roles with the respective LDAP Group Name under *ClusterControl > Sidebar > User Management > LDAP Settings*
2) Verify if the configured *LDAP Settings* are still correct. Go to *ClusterControl > Sidebar > User Management > LDAP Settings > Settings* and hit the 'Verify and Save' button once more. Note that Windows Active directory DNs are case-sensitive.
3) For failure LDAP events, ClusterControl will capture the error log under ``/var/www/html/clustercontrol/app/log/cc-ldap.log``. Examine this log to look for any clues why LDAP authentication fails.
4) On ClusterControl node, try to list out the directory branch by using the admin DN's and password using ``ldapsearch`` command (openldap-clients package is required):

.. code-block:: bash

	$ ldapsearch -H ldaps://ad.company.com -x -b 'CN=DBA,OU=Groups,DC=ad,DC=company,DC=com' -D 'CN=Administrator,OU=Users,DC=ad,DC=company,DC=com' -W

**Solutions:**

ClusterControl supports Active Directory, FreeIPA and OpenLDAP authentication, see :ref:`Sidebar - User Management - LDAP Settings` for details. If the above troubleshooting steps do not help you solve the issue, please contact us via our support channel for further assistance.


Known Issues and Limitations
----------------------------

ClusterControl is not fully tested in OS-level virtualization platform (containers) like OpenVZ. This may cause some issues in reporting of host statistics since it does not use the conventional device naming and mapping. 

Known issues in ClusterControl:

* Running two simultaneous backups (storage on Controller) on two different clusters. One will most likely fail (due to netcat port conflict)
* Running two simultaneous HAProxy install on two different clusters (different load balancer hosts), one will most likely fail.
