Manage
-------

Hosts
``````

Lists of hosts being managed by ClusterControl for the specific cluster. This includes:

* ClusterControl node
* MongoDB/TokuMX shards nodes (MongoDB replica set and sharded cluster)
* MongoDB/TokuMX config nodes (MongoDB sharded cluster)
* MongoDB/TokuMX mongos nodes (MongoDB sharded cluster)
* MongoDB/TokuMX arbiter nodes (MongoDB replica set and sharded cluster)

To remove a host, just select the host and click on the *Remove* button. 

.. Attention:: We strongly recommend user to avoid removing nodes from this page if it still hold a role inside ClusterControl.

Configurations
``````````````

Manages the configuration files of your database nodes. From here you can edit and/or detect whether your database configuration files are in sync and do not diverge. Any changes will not take effect until the database server/process is restarted.

* **Edit/View**
	- Edit and view your database configuration files. Any changes will not take effect until the database server/processes is restarted.

* **Restart**
	- This button will only appear under *Action* when ClusterControl detects configuration changes. Click to restart the database service for selected node.

Processes
`````````

Configures ClusterControl to monitor external processes that are not part of the cluster, e.g. a web server or an application server. ClusterControl will actively monitor these processes and make sure that they are always up and running by executing the check expression command.

To add a new process to be monitored by ClusterControl, click on *Add Custom Managed Process*.

* **Host/Group**
	- Select the managed host.

* **Process Name**
	- Enter the process name.

* **Start Command**
	- OS command to start the process.

* **Pidfile**
	- Full path to the process identifier file.

* **GREP Expression**
	- OS command to check the existence of the process.

* **Remove**
	- Remove the managed process from the list of processes managed by ClusterControl.

* **Deactivate**
	- Disable the managed process.

Software Packages
``````````````````
Allows users to manage packages, upload new versions to ClusterControl’s repository, and select which package to use for deployments. In order to use this feature, set ``post_max_size`` and ``upload_max_filesize`` in php.ini to 256M or more. Make sure you restart Apache to apply the PHP changes. Location of :term:`php.ini` may vary depending on your operating system, infrastructure type and PHP settings.

.. Note:: This feature is intended for packages installed without using package repository.

* **Package Name**
	- Assign a name for the new package.

* **Create**
	- Create the package.

* **Upload**
	- Upload files to an existing package.

* **Available Packages - Database Software**
	- List of softwares and packages. The package *Selected for Deployment* will be rolled out to new nodes, and used for deployments.
	- Check *Delete* and click *Save*, to delete the selected package from ClusterControl server.


Developer Studio
````````````````

Provides functionality to create Advisors, Auto Tuners, or “mini Programs” right within your web browser based on `ClusterControl DSL (Domain Specific Language) <../../dsl.html>`_. The DSL syntax is based on JavaScript, with extensions to provide access to ClusterControl’s internal data structures and functions. The DSL allows you to execute SQL statements, run shell commands/programs across all your cluster hosts, and retrieve results to be processed for advisors/alerts or any other actions. Developer Studio is a development environment to quickly create, edit, compile, run, test, debug and schedule your JavaScript programs.

Advisors in ClusterControl are powerful constructs; they provide specific advice on how to address issues in areas such as performance, security, log management, configuration, storage space, etc. They can be anything from simple configuration advice, warning on thresholds or more complex rules for predictions, or even cluster-wide automation tasks based on the state of your servers or databases. 

ClusterControl comes with a set of basic advisors that include rules and alerts on security settings, system checks (NUMA, Disk, CPU), queries, innodb, connections, performance schema, Galera configuration, NDB memory usage, and so on. The advisors are open source under an MIT license, and available on `GitHub <https://github.com/severalnines/s9s-advisor-bundle>`_. Through the Developer Studio, it is easy to import new advisors as a JS bundle, or export your own for others to try out.

* **New**
	- Name - Specify the file name including folders if you need. E.g. "shared/helpers/cmon.js" will create all appropriate folders if they don't exist yet.
	- File content:
		- Empty file - Create a new empty file.
		- Galera Template - Create a new file containing skeleton code for Galera monitoring.
		- Generic MySQL Template - Create a new file containing skeleton code for generic MySQL monitoring.

* **Import**
	- Imports advisor bundle. Supported format is ``.tar.gz``. See `s9s-advisor-bundle <https://github.com/severalnines/s9s-advisor-bundle>`_.

* **Export**
	- Exports the advisor's directory to a ``.tar.gz`` file. The exported file can be imported to Developer Studio through *ClusterControl > Manage > Developer Studio > Import* function.

* **Advisors**
	- Opens the Advisor list page. See `Advisors <performance.html#advisors>`_ section.

* **Save**
	- Saves the file.
	
* **Move**
	- Moves the file around between different subdirectories.

* **Remove**
	- Remove the script.

* **Compile**
	- Compiles the script.

* **Compile and run**
	- Compile and run the script. The output appears under *Message*, *Graph* or *Raw response* tab down below.
	- The arrow next to the “Compile and Run” button allows us to change settings for a script and, for example, pass some arguments to the ``main()`` function.

* **Schedule Advisor**
	- Schedules the script as an advisor.

We have covered this in details `in this blog post <http://www.severalnines.com/blog/introducing-clustercontrol-developer-studio-creating-your-own-advisors>`_. For full documentation on ClusterControl Domain Specific Language, see `ClusterControl DSL <../../dsl.html>`_ section.
