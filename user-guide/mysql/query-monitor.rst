.. _mysql-query-monitor:

Query Monitoring
----------------

Provides summary of query processing across all nodes in the cluster.

Top Queries
````````````

This is an aggregated list of all your top queries running on all the nodes of your database cluster. The list can be ordered by Occurrence or Execution Time, to show the most common or slowest queries respectively. It is also possible to filter and review queries from one particular node. 

You can see the explain output of your queries by selecting a query in the list. You can also email the query/explain output if you have setup email notifications in the *ClusterControl > Settings > General Settings > Email Notification* page.

Review the *ClusterControl > Settings > Query Monitor* to configure what queries to log (e.g. only log queries that take more than 0.5 seconds to execute).

* **Top Queries By**
	- Sort top queries by:
		- Occurrences
		- Execution Time

* **Clear All Queries**
	- Clear all captured queries.

* **Email Query**
	- Email the selected query and explain.

* **Query**
	- List of sampled queries.

* **DB**
	- Database name.

* **Count**
	- Total number of query occurrences.

* **Rows**
	- Total rows involved:
		- Sent - The number of rows MySQL returned
		- Examined - The number of rows MySQL believes it must examine to execute the query

* **Lock Time**
	- The amount of time the query spent waiting to acquire the lock it needs to run:
		- Max - Maximum locking time
		- Avg - Average locking time

* **Exec Time**
	- Execution time:
	- Max - Maximum execution time
	- Avg - Average execution time

* **Total Exec Time**
	- The total amount of execution time of:
		- Absolute - Total execution time of the query.
		- Relative - Percentage of the query execution time over total time.

* **Expl**
	- Y means the query has EXPLAIN command output.

.. Note:: By default, *Rows and Lock Time* columns are hidden. You can add them to the list by clicking on any of the columns and choosing *Columns > Rows/Lock Time*.

Running Queries
````````````````

View current running queries on your database cluster similar to ``SHOW PROCESSLIST`` command in MySQL. You can stop a running query by selecting to kill the connection that started the query. The processlist can be filtered out by host.

This page is auto-refresh every 30 seconds. You can change the refresh rate by clicking on the arrow beside the green *Refresh* button.

* **Kill [thread ID]**
	- Click to kill the specific MySQL thread ID.

* **ID**
	- Connection identifier number.

* **DB**
	- Database name.

* **MySQL Server**
	- The MySQL server from which the process is retrieved.

* **User**
	- The MySQL user who issued the statement.

* **Exec T**
	- The time in seconds that the thread has been in its current state.

* **Host**
	- The hostname (or port if TCP/IP) of the client issuing the statement.

* **Info**
	- The statement the thread is executing, or NULL if it is not executing any statement.

* **Command**
	- The type of command the thread is executing.

* **State**
	- An action, event, or state that indicates what the thread is doing, as explained in `MySQL Documentation <http://dev.mysql.com/doc/refman/5.6/en/general-thread-states.html>`_ page.

Query Histogram
````````````````

Use this feature to filter the query activity for a certain time period.

* **Filter by Server**
	- Filter the output by database server.

* **Email Query**
	- Email the query/explain output if you have setup email notifications in the *ClusterControl > Settings > General Settings > Email Notification* page.