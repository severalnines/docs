.. _pgsql-query-monitor:

Query Monitoring
----------------

Provides summary of query processing across all nodes in the cluster.

Top Queries
````````````

This is an aggregated list of all your top queries running on all the nodes of your database cluster. The list can be ordered by Occurrence or Execution Time, to show the most common or slowest queries respectively. It is also possible to filter and review queries from one particular node. 

You can see the explain output of your queries by selecting a query in the list. You can also email the query/explain output if you have setup email notifications in the *ClusterControl > Settings > General Settings > Email Notification* page.

Review the *ClusterControl > Settings > Query Monitor* to configure what queries to log (e.g. only log queries that take more than 1 seconds to execute).

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
		- Sent - The number of rows returned
		- Examined - The number of rows it must examine to execute the query

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

View current running queries on your database nodes similar to ``select * from pg_stat_activity`` command in PostgreSQL. You can stop a running query by selecting to kill the connection that started the query. The processlist can be filtered out by host.

This page is auto-refresh every 30 seconds. You can change the refresh rate by clicking on the arrow beside the green *Refresh* button.

* **Postgres Instance**
	- IP address or hostname of the instance.

* **App Name**
	- Name of the application that is connected to this backend.

* **Client**
	- IP address of the client connected to this backend. If this field is null, it indicates either that the client is connected via a Unix socket on the server machine or that this is an internal process such as autovacuum.

* **Database name**
	- Name of the database this backend is connected to.

* **Query**
	- Text of this backend's most recent query. If state is active this field shows the currently executing query. In all other states, it shows the last query that was executed.

* **Query Start**
	- Time when the currently active query was started, or if state is not active, when the last query was started.

* **State**
	- Current overall state of this backend. Possible values are:
		- active: The backend is executing a query.
		- idle: The backend is waiting for a new client command.
		- idle in transaction: The backend is in a transaction, but is not currently executing a query.
		- idle in transaction (aborted): This state is similar to idle in transaction, except one of the statements in the transaction caused an error.
		- fastpath function call: The backend is executing a fast-path function.
		- disabled: This state is reported if track_activities is disabled in this backend.

* **Waiting**
	- True if this backend is currently waiting on a lock.

* **Xact. Start**
	- Time when this process' current transaction was started, or null if no transaction is active. If the current query is the first of its transaction, this column is equal to the query_start column.

Query Histogram
````````````````

Use this feature to filter the query activity for a certain time period.

* **Filter by Server**
	- Filter the output by database server.

* **Email Query**
	- Email the query/explain output if you have setup email notifications in the *ClusterControl > Settings > General Settings > Email Notification* page.