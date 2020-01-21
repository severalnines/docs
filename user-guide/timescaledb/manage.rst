.. _TimeScaleDB - Manage:

Manage
-------

Hosts
++++++

Lists of hosts being managed by ClusterControl for the specific cluster. This includes:

* ClusterControl node
* TimeScaleDB nodes (standalone)
* TimeScaleDB master nodes (replication)
* TimeScaleDB slave nodes (replication)

To remove a host, just select the host and click on the *Remove* button. 

.. Attention:: We strongly recommend user to avoid removing nodes from this page if they still hold a role inside ClusterControl.

.. _TimeScaleDB - Manage - Configurations:

Configurations
+++++++++++++++

Manages the configuration files of your database and HAProxy nodes. For TimeScaleDB database, within the ``postgresql.conf`` file, some parameters contain comments that indicate ``# (change requires restart)``. Note that ClusterControl will neither perform the reload nor restart operation after modifying the configuration file. One has to schedule server reload/restart operation to load the changes into the server runtime.

.. Note:: ClusterControl does not store configuration changes history so there is no versioning at the moment. Only one version exists at one time. It imports the latest configuration files every 30 minutes and overwrites it in CMON database. This limitation will be improved in the upcoming release where ClusterControl shall support configuration versioning with dynamic import interval.

* **Save**
	- Save the changes that you have made and push them to the corresponding node.

* **Import**
	- Re-import configuration if you have:
		- Performed local configuration changes directly on the configuration files.
		- Restarted the PostgreSQL servers/performed a rolling restart after a configuration change.
	- ClusterControl will trigger a job to fetch the latest modification from each PostgreSQL and/or HAProxy node.

* **Change/Set Parameter**
	- ClusterControl will attempt to modify the configuration value if the parameter is valid in the respective PostgreSQL configuration file.


.. _TimeScaleDB - Manage - Configurations - Base Template Files:
	
Base Template Files
````````````````````

All services configured by ClusterControl use a base configuration template available under ``/usr/share/cmon/templates`` on the ClusterControl node. You can directly modify the file to suit your deployment policy however, this directory will be replaced on every package upgrade. 

To make sure your custom configuration template files persist across upgrade, store the files under ``/etc/cmon/templates`` directory. When ClusterControl loads up the template file for deployment, files under ``/etc/cmon/templates`` will always have higher priority over the files under ``/usr/share/cmon/templates``. If two files having identical name exist on both directories, the one located under ``/etc/cmon/templates`` will be used.

The following are template files provided by ClusterControl related to TimeScaleDB:

============================ ===========
Filename                     Description
============================ ===========
postgreschk                  TimeScaleDB health check script template for multi-master.
postgreschk_rw_split         TimeScaleDB health check script template for read-write splitting.
postgreschk_xinetd           Xinetd configuration template for TimeScaleDB health check.
postgresql.service.override  Systemd unit file template for TimeScaleDB service.
haproxy.cfg                  HAProxy configuration template for Galera Cluster.
haproxy_rw_split.cfg         HAProxy configuration template for read-write splitting.
keepalived-1.2.7.conf        Legacy keepalived configuration file (pre 1.2.7). This is deprecated.
keepalived.conf              Keepalived configuration file.
keepalived.init              Keepalived init script, for 'Build from Source' installation option.
============================ ===========

.. _TimeScaleDB - Manage - Load Balancer:

Load Balancer
++++++++++++++

Deploys supported load balancers and virtual IP address for this cluster.

HAProxy
````````

Installs and configures an :term:`HAProxy` instance. ClusterControl will automatically install and configure HAProxy, install ``postgreschk_rw_split`` script (to report the TimeScaleDB healthiness) on each of database nodes as part of :term:`xinetd` service and start the HAProxy service. Once the installation completes, TimeScaleDB will listen on *Listen Port* (5433 for read-write and 5434 for read-only connections) on the configured node.

This feature is idempotent, you can execute it as many times as you want and it will always reinstall everything as configured.

Deploy HAProxy
'''''''''''''''

* **Server Address**
	- Select on which host to add the load balancer. If the host is not provisioned in ClusterControl (see `Hosts`_), type in the IP address. The required files will be installed on the new host. Note that ClusterControl will access the new host using passwordless SSH.

* **Policy**
	- Choose one of these load balancing algorithms:
		- leastconn - The server with the lowest number of connections receives the connection.
		- roundrobin - Each server is used in turns, according to their weights.
		- source - The same client IP address will always reach the same server as long as no server goes down.

* **Listen Port (Read/Write)**
	- Specify the HAProxy listening port. This will be used as the load balanced TimeScaleDB connection port for read/write connections.

* **Install for read/write splitting (master-slave replication)**
	- Toggled on if you want the HAProxy to use another listener port for read-only. A new text box will appear right next to the *Listen Port (Read/Write)* text box.
	
* **Build from Source**
	- ClusterControl will compile the latest available source package downloaded from http://www.haproxy.org/#down. 
	- This option is only required if you intend to use the latest version of HAProxy or if you are having problem with the package manager of your OS distribution. Some older OS versions do not have HAProxy in their package repositories.

**Advanced Settings**
	
* **Stats Socket**
	- Specify the path to bind a UNIX socket for HAProxy statistics. See `stats socket <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#stats%20socket>`_.

* **Admin Port**
	- Port to listen HAProxy statistic page. 
	
* **Admin User**
	- Admin username to access HAProxy statistic page. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.
	
* **Admin Password**
	- Password for *Admin User*. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.

* **Backend Name**
	- Name for the backend. No whitespace or tab allowed.
	
* **Timeout Server (seconds)**
	- Sets the maximum inactivity time on the server side. See `timeout server <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#timeout%20server>`_.

* **Timeout Client (seconds)**
	- Sets the maximum inactivity time on the client side. See `timeout client <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-timeout%20client>`_.
	
* **Max Connections Frontend**
	- Sets the maximum per-process number of concurrent connections to the HAProxy instance. See `maxconn <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#maxconn>`_.

* **Max Connections Backend/per instance**
	- Sets the maximum per-process number of concurrent connections per backend instance. See `maxconn <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#maxconn>`_.

* **xinetd allow connections from**
	- The specified subnet will be allowed to access the ``postgreschk_rw_split`` via as xinetd service, which listens on port 9201 on each of the database nodes. To allow connections from all IP address, use the default value, 0.0.0.0/0.

**Server instances in the load balancer**

* **Include**
	- Select TimeScaleDB servers in your cluster that will be included in the load balancing set.

* **Role**
	- Supported roles:
		- Active - The server is actively used in load balancing.
		- Backup - The server is only used in load balancing when all other non-backup servers are unavailable.
		
* **Connection Address**
	- Pick the IP address where HAProxy should be listening to on the host.

Import HAProxy
''''''''''''''

* **HAProxy Address**
	- Select on which host to add the load balancer. If the host has not been provisioned by ClusterControl (see `Hosts`_), type in the IP address or hostname. The required files will be installed on the new host. Note that ClusterControl will access the new host using passwordless SSH.

* **cmdline**
	- Specify the command line that ClusterControl should use to start the HAProxy service. You can verify this by using ``ps -ef | grep haproxy`` and retrieve the full command how the HAProxy process started. Copy the full command line and paste it in the textfield.

* **Port**
	- Port to listen HAProxy admin/statistic page (if enable).
	
* **Admin User**
	- Admin username to access HAProxy statistic page. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.

.. Note:: You need to have an admin user/password set in HAProxy configuration otherwise you will not see any HAProxy stats.
	
* **Admin Password**
	- Password for *Admin User*. See `stats auth <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#4-stats%20auth>`_.

* **LB Name**
	- Name for the backend. No whitespace or tab allowed.
	
* **HAProxy Config**
	- Location of HAProxy configuration file (haproxy.cfg) on the target node.

* **Stats Socket**
	- Specify the path to bind a UNIX socket for HAProxy statistics. See `stats socket <http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#stats%20socket>`_. 
	- Usually, HAProxy writes the socket file to  ``/var/run/haproxy.socket`` . This is needed by ClusterControl to monitor HAProxy. This is usually defined in the ``haproxy.cfg`` file, and the line looks like:

.. code-block:: bash

	stats socket /var/run/haproxy.socket user haproxy group haproxy mode 600 level

Keepalived
```````````

:term:`Keepalived` requires two HAProxy instances in order to provide virtual IP address failover. By default, this IP address will be assigned to instance 'Keepalived 1'. If the node goes down, the IP address will be automatically failover to 'Keepalived 2' accordingly.

Deploy Keepalived
'''''''''''''''''

* **Select type of loadbalancer**
	- Only HAProxy is supported for TimeScaleDB at the moment.

* **Keepalived 1**
	- Select the primary Keepalived node (installed or imported using `HAProxy`_).
	
* **Keepalived 2**
	- Select the secondary Keepalived node (installed or imported using `HAProxy`_).

* **Virtual IP**
	- Assigns a virtual IP address. The IP address should not exist in any node in the cluster to avoid conflict.

* **Network Interface** 
	- Specify a network interface to bind the virtual IP address. This interface must able to communicate with other Keepalived instances and support IP protocol 112 (VRRP) and unicasting.

* **Install Keepalived**
	- Starts installation of Keepalived.
	
Import Keepalived
'''''''''''''''''

* **Keepalived 1**
	- Specify the IP address or hostname of the primary Keepalived node.
	
* **Add Keepalived Instance**
	- Shows additional input field for secondary Keepalived node.

* **Remove Keepalived Instance**
	- Hides additional input field for secondary Keepalived node.

* **Virtual IP**
	- Assigns a virtual IP address. The IP address should not exist in any node in the cluster to avoid conflict.

* **Deploy Keepalived**
	- Starts the import of Keepalived job.

Users Management
+++++++++++++++++

Users
``````

Shows a summary of PostgreSQL users and privileges for the cluster. All of the changes are automatically synced to all database nodes in the cluster.

You can filter the list by username, hostname, database or table in the text box. Click on *Edit* to update the existing user or *Drop User* to remove the existing user. Click on *Create New User* to open the user creation wizard:

* **Username**
	- PostgreSQL username.

* **Password**
	- Password for *Username*. Minimum requirement is 4 characters.

* **Hostname**
	- Hostname or IP address range of the user or client. For IP address range, use the IP address/subnet format, e.g, 192.168.100.0/24.

* **Privileges**
	- Specify the privilege for this user. If the *Privileges* text box is active, it will list out all possible privileges of the server.
	- Specify the database or table name. It can be in ``*.*``, ``{database_name}``, ``{database_name}.*`` or ``{database_name}.{table_name}`` format.

* **Add Statement**
	- Add another *Privileges* statement builder entry for this user.

Custom Advisors
+++++++++++++++

Manages threshold-based advisors with host or TimeScaleDB statistics without needing to write your own JavaScript script (like all the default scripts under `Developer Studio`_). The threshold advisor allows you to set threshold to be alerted on if a metric falls below or raises above the threshold and stays there for a specified timeframe.

Clicking on 'Create Custom Advisor' and 'Edit Custom Advisor' will open a new dialog, which described as follows:

* **Type**
	- Type of custom advisor. At the moment, only Threshold is supported.

* **Applies To**
	- Choose the target cluster.

* **Resource**
	- Threshold resources.
		- Host: Host metrics collected by ClusterControl.
		- Node: Database node metrics collected by ClusterControl.

* **Hosts**
	- Target host(s) in the chosen cluster. You can select individual host or all hosts monitored under this cluster.

Condition
``````````

* **If metric**
	- List of metrics monitored by ClusterControl. Choose one metric to create a threshold condition.

* **Condition**
	- Type of conditions for the Warning and Critical values.

* **For(s)**
	- Timeframe in seconds before falling/raising an alarm.

* **Warning**
	- Value for warning threshold.

* **Critical**
	- Value for critical threshold.

* **Max Values seen for selected period**
	- ClusterControl provides preview of already recorded data in a graph to help you determine accurate values for timeframe, warning and critical.

Advisor Description
````````````````````

Describe the Advisor and provide instructions on what actions that may be needed if the threshold is triggered. Available variables substitutions:

================= ============
Variable          Description
================= ============
%CLUSTER%         Selected cluster
%CONDITION%       Condition
%DURATION%        Duration
%HOSTNAME%        Selected host or node
%METRIC%          Metric
%METRIC_GROUP%    Group for the selected metric
%RESOURCE%        Selected resource
%TYPE%            Type of the custom advisor
%CRITICAL_VALUE%  Critical Value
%WARNING_VALUE%   Warning Value
================= ============

.. _TimeScaleDB - Manage - Developer Studio:

Developer Studio
++++++++++++++++

Provides functionality to create Advisors, Auto Tuners, or Mini Programs right within your web browser based on :ref:`ClusterControl DSL`. The DSL syntax is based on JavaScript, with extensions to provide access to ClusterControl's internal data structures and functions. The DSL allows you to execute SQL statements, run shell commands/programs across all your cluster hosts, and retrieve results to be processed for advisors/alerts or any other actions. Developer Studio is a development environment to quickly create, edit, compile, run, test, debug and schedule your JavaScript programs.

Advisors in ClusterControl are powerful constructs; they provide specific advice on how to address issues in areas such as performance, security, log management, configuration, storage space, etc. They can be anything from simple configuration advice, warning on thresholds or more complex rules for predictions, or even cluster-wide automation tasks based on the state of your servers or databases. 

ClusterControl comes with a set of basic advisors that include rules and alerts on security settings, system checks (NUMA, Disk, CPU), queries, InnoDB, connections, PERFORMANCE_SCHEMA, configuration, NDB memory usage, and so on. The advisors are open source under MIT license, and publicly available at `GitHub <https://github.com/severalnines/s9s-advisor-bundle>`_. Through the Developer Studio, it is easy to import new advisors as a JS bundle, or export your own for others to try out.

* **New**
	- Name - Specify the file name including folders if you need. E.g. ``shared/helpers/cmon.js`` will create all appropriate folders if they don't exist yet.
	- File content:
		- Empty file - Create a new empty file.
		- Template - Create a new file containing skeleton code for monitoring.
		- Generic MySQL Template - Create a new file containing skeleton code for generic MySQL monitoring.

* **Import**
	- Imports advisor bundle. Supported format is ``.tar.gz``. See `s9s-advisor-bundle <https://github.com/severalnines/s9s-advisor-bundle>`_.

* **Export**
	- Exports the advisor's directory to a ``.tar.gz`` format. The exported file can be imported to Developer Studio through *ClusterControl > Manage > Developer Studio > Import* function.

* **Advisors**
	- Opens the Advisor list page. See :ref:`TimeScaleDB - Performance - Advisors`.

* **Save**
	- Saves the file.
	
* **Move**
	- Moves the file around between different subdirectories.

* **Remove**
	- Removes the script.

* **Compile**
	- Compiles the script.

* **Compile and run**
	- Compile and run the script. The output appears under *Message*, *Graph* or *Raw response* tab underneath the editor.
	- The arrow next to the "Compile and Run" button allows us to change settings for a script and for example, pass some arguments to the ``main()`` function.

* **Schedule Advisor**
	- Schedules the script as an advisor.

.. seealso:: `Introducing ClusterControl Developer Studio and Creating your own Advisors in JavaScript <https://severalnines.com/blog/introducing-clustercontrol-developer-studio-and-creating-your-own-advisors-javascript>`_.

For full documentation on ClusterControl Domain Specific Language, see :ref:`ClusterControl DSL`.
