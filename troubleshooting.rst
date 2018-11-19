.. _troubleshooting:

Troubleshooting
===============

This troubleshooting guide provides detailed information on how to troubleshoot ClusterControl. ClusterControl is an operational management and automation software for database clusters, which aims to simplify deployment, monitoring, management and scaling of clusters. 

This document is a guide to help troubleshoot problems that commonly arise with ClusterControl. In particular, this guide addresses possible problems that may originate from ClusterControl components namely controller, CMON database, UI, notifications, clouds, web-based SSH and also CMONAPI. The document provides instructions on what data to collect when creating error reports to be submitted to `Severalnines Support <http://support.severalnines.com>`_.

Note that this troubleshooting guide covers the latest ClusterControl version. We recommend you to stay up-to-date with the latest version of ClusterControl as it contains the latest bug fixes. To upgrade ClusterControl to the latest version, please refer to the section on :ref:`Administration - Upgrading ClusterControl`.

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
| ClusterControl UI error log   | /var/www/html/clustercontrol/app/tmp/logs/error.log  | /var/www/clustercontrol/app/tmp/logs/debug.log       |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| ClusterControl UI debug log   | /var/www/html/clustercontrol/app/tmp/logs/debug.log  | /var/www/clustercontrol/app/tmp/logs/debug.log       |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| Apache error log              | * /var/log/httpd/ssl_error_log                       | /var/log/apache2/error.log                           |
|                               | * /var/log/httpd/error_log                           |                                                      |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+
| Apache access log             | * /var/log/httpd/access_log                          | * /var/log/apache2/access.log                        |
|                               | * /var/log/httpd/ssl_access_log                      | * /var/log/apache2/ssl_access.log                    |
+-------------------------------+------------------------------------------------------+------------------------------------------------------+

Output of CMON process log per cluster is also accessible directly from *ClusterControl > pick a cluster > Logs > CMON Logs*.

Reporting and Debugging
-----------------------

If the above does not address the issues you are having, please refer to our `knowledge base <http://support.severalnines.com/categories/20019191-Knowledge-Base>`_ or contact us via our Support Portal by creating a support request at http://support.severalnines.com/tickets/new. We encourage you to make use of our `Error Reporting`_ tool as described in the next section.

Error Reporting
+++++++++++++++

ClusterControl provides an error reporting tool called ``s9s_error_reporter``. This can greatly facilitate the troubleshooting process as it collects the necessary information on the entire database cluster setup and archives it in a compressed package. Use this tool to generate an error report and then attach the generated tar ball package when creating a `Severalnines Support Ticket <http://support.severalnines.com>`_.

To generate an error report, run the following command on ClusterControl node:

.. code-block:: bash

	# as root:
	s9s_error_reporter -i 1
	# or as a non-root user:
	sudo s9s_error_reporter -i 1 

Where, 1 is the cluster ID of the corresponding cluster. This tool will generate an error report for the particular cluster. If you have no cluster managed by ClusterControl, you might want to specify 0, where 0 has a special meaning in ClusterControl. It means it will generate a global error report without specifically collecting information about a particular cluster (mostly information about local ClusterControl installation and configuration).

At the end of the execution, it will print out something like this:

.. code-block:: bash

	Executing tar -C /var/tmp/cmon-000809-42b7c3b595d5282a -czf '/var/www/html/clustercontrol/app/tmp/logs/error-report-2018-11-19_065637-cluster0.tar.gz' error-report-2018-11-19_065637-cluster0
	Please attach /var/www/html/clustercontrol/app/tmp/logs/error-report-2018-11-19_065637-cluster0.tar.gz to the support issue.

Attach the generated log file to your support issue.

.. Note:: We also recommend you take a screenshot showing the problem area, e.g, the Overview from the UI is always great to see if there are node failures, cluster issues or missing data.

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

CMON will enable LOG_DEBUG messages and print detailed information on the screen (stdout) as well as ``/var/log/cmon.log`` or ``/var/log/cmon_{cluster ID}.log``. Press ``Ctrl + C`` to terminate the process. In certain cases, the CMON  output might be needed to get insight on the problem.

Debugging ClusterControl UI
+++++++++++++++++++++++++++

To enable ClusterControl UI debug, SSH into the ClusterControl node and adjust following values inside ``{wwwroot}/clustercontrol/app/Config/core.php``:

.. code-block:: php

	Configure::write('debug', 0);

Where,

* 0: Production mode. All errors and warnings are suppressed.
* 1: Errors and warnings shown, model caches refreshed, flash messages halted.
* 2: As in 1, but also with full debug messages and SQL output.

Make sure ``{wwwroot}/clustercontrol/app/tmp`` has write permission and is owned by Apache user for the debug and error log to be generated.

Common Issues
-------------

This section covers common issues when dealing with ClusterControl components, with possible troubleshooting steps and solutions. There is also a `community forum <http://support.severalnines.com/hc/en-us/community/topics>`_ available with knowledge base sections for public reference.

If you need further assistance, please contact us via our support channel by `submitting a support request <http://support.severalnines.com/hc/en-us/requests/new>`_ or post a new thread in `our community help forum <http://support.severalnines.com/hc/en-us/community/topics/200447583-Community-Help>`_.


ClusterControl Controller (CMON)
++++++++++++++++++++++++++++++++

This section covers common issues encountered related to ClusterControl Controller (CMON).

CMON unable to restart MySQL using service command
````````````````````````````````````````````````````

* **Description:**
	- When scheduling a start/restart job, ClusterControl fails to start the node with error "Command not found".

* **Example error:**

.. code-block:: bash

	galera1.domain.com: Starting mysqld failed: Error: Command not found (host: galera1.domain.com): service mysql restart 
	galera1.domain.com: Starting mysqld

* **Troubleshooting steps:**

1. SSH into the DB node and check the user's environment path variable:

.. code-block:: bash

	ssh -tt -i /home/admin/.ssh/id_rsa admin@galera1.domain.com "sudo env | grep PATH"
	PATH=/usr/local/bin:/bin:/usr/bin

2. Look at the PATH output.

* **Solution:**
	- Ensure the ``/sbin`` path is there. This way, ClusterControl can automatically locate and run the "service" command.
	- If the ``/sbin`` path is not listed in the PATH, add it by using the following command:
	
.. code-block:: bash

	PATH=$PATH:/sbin 
	export PATH

- However, the above won't persist if the user logs out from the terminal. To make it persistent, add those lines into ``/home/{SSH user}/.bash_profile`` or ``/home/{SSH user}/.bashrc``


CMON always tries to recover failed database nodes during my maintenance window.
````````````````````````````````````````````````````````````````````````````````

* **Description:**
	- By default, CMON is configured to perform recovery of failed nodes or clusters. This behavior can be overridden by disabling automatic recovery feature or enabling maintenance mode for the node/cluster.

* **Solution:**
	1) Enabling maintenance mode for selected nodes (recommended).
		- To enable maintenance window, go to *Nodes > select the node > toggle ON on the Maintenance Mode*. You have to specify the reason and duration of maintenance window. During this period, any alarms and notifications raised for this node will be disabled. You can toggle OFF the maintenance mode at any time when the maintenance exercise is completed.
	2) Disabling automatic recovery.
		- To disable automatic recovery temporarily, you can just click on the 'power' icon for node and cluster. Red means automatic recovery is turned off while green indicates recovery is turned on. This behavior will not persistent if CMON is restarted.
		- To make the above change persistent, disable node or cluster auto recovery by specifying following line inside CMON configuration file of respective cluster. For example, if you want to disable automatic recovery for cluster ID 1, inside ``/etc/cmon.d/cmon_1.cnf``, set the following line:

.. code-block:: bash

	enable_autorecovery=0


CMON process dies with “Critical error (mysql error code 1)”
````````````````````````````````````````````````````````````

* **Description:**
	- After starting CMON service, it stops and /var/log/cmon.log shows the following error.

* Example error:

.. code-block:: bash

	(ERROR) Critical error (mysql error code 1) occurred - shutting down

* **Troubleshooting steps:**

1) Run the following command on the ClusterControl host to check if the ClusterControl host has the ability to connect to the DB host with current credentials:

.. code-block:: bash

	$ mysql -ucmon -p -h[database node IP] -P[MySQL port] -e 'SHOW STATUS'

2) Check GRANT for cmon user on each database host:

.. code-block:: mysql

	mysql> SHOW GRANTS FOR 'cmon'@'[ClusterControl IP address]';


* **Solution:**
	- It is not recommended to mix public IP address and internal IP address. For the GRANT, try to use the IP address that your database nodes use to communicate with each other.
	- If the SHOW STATUS returns ``ERROR 1130 (HY000): Host '[ClusterControl IP address]' is not allowed to connect to this``, the database host is missing the cmon user grant. Run following command to reset the cmon user privileges:

.. code-block:: mysql

	mysql> GRANT ALL PRIVILEGES ON *.* TO 'cmon'@'[ClusterControl IP]' IDENTIFIED BY '[cmon password]' WITH GRANT OPTION; 
	mysql> FLUSH PRIVILEGES;
	
Where, [ClusterControl IP] is ClusterControl IP address and [cmon password] is ``mysql_password`` value inside CMON configuration file.

ClusterControl UI
+++++++++++++++++

This section covers common issues encountered related to ClusterControl UI.

/{ID}/auth error
`````````````````

* **Description:**
	- The ClusterControl UI shows a toaster notification (on the top right of the UI) indicating that it has authentication problem to connect to a specific cluster ID.

* **Troubleshooting steps:**
	- Run the following command to verify if token is set correctly for corresponding cluster:

.. code-block:: mysql

	mysql> SELECT cluster_id, token FROM dcps.clusters;
	
* **Solution:**
	- In this case you need to update the token column in ``dcps.clusters`` table for the ``cluster_id={ID}`` so it matches the ``rpc_key`` in ``/etc/cmon.d/cmon_{ID}.cnf``. These tokens must match. Execute the following update query on the dpcs database:

.. code-block:: mysql

	mysql> UPDATE dcps.clusters SET token=‘<rpc_key>’ WHERE cluster_id={ID};


/0/auth error
`````````````

* **Description:**
	- The ClusterControl UI shows a toaster notification (on the top right of the UI) indicating that it has authentication problem to connect to cluster 0 (0 means global view of clusters under ClusterControl management).

* **Troubleshooting steps:**
	- Retrieve the value of global token inside ``/etc/cmon.cnf``, ``/var/www/html/clustercontrol/bootstrap.php`` and ``/var/www/html/cmonapi/config/bootstrap.php``:

.. code-block:: bash

	$ grep rpc_key /etc/cmon.cnf
	$ grep RPC_TOKEN /var/www/html/clustercontrol/bootstrap.php
	$ grep CMON_TOKEN /var/www/html/cmonapi/config/bootstrap.php

* **Solutions:**
	- Verify that the ``RPC_TOKEN`` value in ``/var/www/html/clustercontrol/bootstrap.php`` and ``CMON_TOKEN`` value in ``/var/www/html/cmonapi/config/bootstrap.php`` match the token defined as ``rpc_key`` in ``/etc/cmon.cnf``. If you manipulate ``/etc/cmon.cnf`` you must restart cmon for the change to take effect.

Known Issues and Limitations
----------------------------

ClusterControl is not fully tested in OS-level virtualization platform (containers) like OpenVZ. This may cause some issues in reporting of host statistics since it does not use the conventional device naming and mapping. 

Known issues in ClusterControl:

* Running two simultaneous backups (storage on Controller) on two different clusters. One will most likely fail (due to netcat port conflict)
* Running two simultaneous HAProxy install on two different clusters (different load balancer hosts), one will most likely fail.

