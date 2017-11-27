Logs
-----

Provides interface for log messages, CMON jobs and error reporting for troubleshooting and auditing purposes. The tab is embedded with a counter (highlighted in red) indicating there are new jobs executed by ClusterControl under *ClusterControl > Logs > Jobs* which might require your attention.

Jobs
````

Any action performed by the user will trigger the creation of a new job, which is added to a job queue. ClusterControl then schedules each job in the order they are added to the queue. When a job is executed, detailed workflow messages related to the execution of the job are logged. This is useful for troubleshooting, especially if a job starts execution but fails to complete. 

You can see which user performed an action in the Job log, what that action actually entailed and from which IP the action originated.

======== ===========
Status   Description
======== ===========
FINISHED The job was successfully executed. It should return a "Command ok" result with "Exit Code: 0"
RUNNING  The job is currently running.
DEFINED  The job has been defined and queued.
FAILED   The job was stopped due to error. If fails, it should return "The job failed" with "Exit Code: 1"
======== ===========

CMON Logs
``````````

Centralized list of log events from the ClusterControl server and agents. You can filter the output by hostname and log level (ALL, DEBUG, INFO, WARNING, ERROR, CRITICAL, ALERT). Log events listed in this page can also be retrieved directly from the CMON log file of the respective cluster ID, e.g (default is ``/var/log/cmon.log``).


Error Reports
``````````````

Generates a compressed file (tar ball) containing information about the cluster (CMON logs, database error logs, and the contents of some important CMON DB tables). This is very useful for troubleshooting purposes, especially when working with the `Severalnines Support <http://support.severalnines.com>`_ team on a support issue. Attach the generated compressed file to your support request.

* **Create Error Report**
	- Create an error report. This will trigger a CMON job. Once it completes, the error report package will be listed in this page. You can then download it directly from ClusterControl UI.

* **Delete Error Report**
	- Removes the selected error report from ClusterControl node.

.. Note:: We also recommend you take a screenshot showing the problem area, e.g, the Cluster Overview dashboard from the UI is useful if there are node failures, cluster issues or missing data.

System Logs
````````````

Tree view of corresponding database logs across cluster. Clicking on a log file will appear in a new tab in viewing panel on the right-hand side. Browsing through the appropriate error logs is key element of the troubleshooting process.

* **Refresh Logs**
	- Triggers a job for ClusterControl to perform logs collection. By default, the logs are fetched every ``db_stats_collection`` which is 30 minutes. You can verify this from the 'Last Updated' value at the bottom of the section.
	
* **Search**
	- Look up the search term throughout the active document in viewing panel.
	
* **Clear**
	- Clear the search term.

ClusterControl collects important database logs depending on the cluster type as shown below:

- MySQL:
	- MySQL error log
	- innobackup backup log
	- innobackup prepare log

- MongoDB:
	- MongoDB database system log
	- mongodump backup log

.. Note:: ClusterControl does not pull MySQL general logs (if enabled) since it can easily become very huge in size.