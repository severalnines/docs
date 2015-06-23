Logs
-----

To make sure

Jobs
````

Any action performed by the user will trigger the creation of a new job, which is added to a job queue. ClusterControl then schedules each job in the order they are added to the queue. When a job is executed, detailed workflow messages related to the execution of the job are logged. This is useful for troubleshooting, especially if a job starts execution but fails to complete. 

You can see which user performed an action in the Job log, what that action actually entailed and from which IP the action originated.

======== ===========
Status   Description
======== ===========
FINISHED The job was successfully executed. It should return a “Command ok” result with “Exit Code: 0”
RUNNING  The job is currently running.
DEFINED  The job has been defined and queued.
FAILED   The job was stopped due to error. If fails, it should return “The job failed” with “Exit Code: 1”
======== ===========

CMON Logs
``````````

Centralised list of log events from the ClusterControl server and agents. You can filter the output by hostname and log level (ALL, DEBUG, INFO, WARNING, ERROR, CRITICAL, ALERT). Log events listed in this page can also be retrieved directly from CMON log file (default is ``/var/log/cmon.log``).


Error Reports
``````````````

Generates a compressed file (tar ball) containing information about the cluster (CMON logs, database error logs, and the contents of some important CMON DB tables). This is very useful for troubleshooting purposes, especially when working with the `Severalnines Support <http://support.severalnines.com>`_ team on a support issue. Attach the generated compressed file to your support request.

To create an error report, click on *Create Error Report* button. This will trigger a CMON job. Once it completes, the error report package will be listed in this page.

* **Store Error Report on Web Server**
	- Yes - The generated error report will be stored on ClusterControl web root directory, you can download it directly via the web browser.
	- No - The generated error report will be stored on the ClusterControl node in a custom location.

* **Destination**
	- Only applicable if you specified *Store Error Report on Web Server* to No. This destination is relative to the ClusterControl node.

.. Note:: We also recommend you take a screenshot showing the problem area, e.g, the Cluster Overview dashboard from the UI is useful if there are node failures, cluster issues or missing data.
 