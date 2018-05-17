.. _pgsql-query-monitor:

Query Monitoring
----------------

Provides summary of query processing across all nodes in the cluster.

Top Queries
+++++++++++

This is an aggregated list of all your top queries running on all the nodes of your database cluster. The list can be ordered by Occurrence or Execution Time, to show the most common or slowest queries respectively. It is also possible to filter and review queries from one particular node. 

You can see the explain output of your queries by selecting a query in the list. Review the *Settings > Query Monitor* to configure what queries to log (e.g. only log queries that take more than 1 seconds to execute).
	
Settings
````````

Configures the Query Monitor settings, as explained below:

* **Long Query Time**
	- Collects queries taking longer than Long Query Time seconds, for example:
		- 0.1 - Only queries taking more than 0.1 seconds will be accounted.

* **Log queries not using indexes?**
	- Configures ClusterControl behavior on sampling queries without indexes:
		- Yes - Logs queries which are using indexes.
		- No - Ignores queries that are not using indexes (will not be accounted for in *ClusterControl > Query Monitor > Query Outliers*).

Top Queries Table
``````````````````

This page is auto-refresh every 30 seconds. You can change the refresh rate by clicking on *Refresh rate* dropdown at top right. The following describes the Top Queries table columns:

* **Query**
	- List of sampled queries.

* **DB**
	- Database name.

* **Count**
	- Total number of query occurrences.

* **Rows**
	- Total rows involved:
		- Sent - The number of rows PostgreSQL returned.
		- Examined - The number of rows PostgreSQL believes it must examine to execute the query.

* **Tmp tables**
	- The number of temporary tables created for this query, on RAM or on disk.

* **Exec Time**
	- Execution time in microseconds of:
		- Max - Maximum execution time.
		- Avg - Average execution time.
		- Stdev - Standard deviation time. 

* **Total Exec Time**
	- The total amount of execution time of:
		- Absolute - Total execution time of the query.
		- Relative - Percentage of the query execution time over total time.

Running Queries
++++++++++++++++

View current running queries on your database nodes similar to ``select * from pg_stat_activity`` command in PostgreSQL. You can stop a running query by selecting to kill the connection that started the query. The process list can be filtered out by host.

This page is auto-refresh every 30 seconds. You can change the refresh rate by clicking on the arrow beside the green *Refresh* button.

* **Host**
	- IP address or hostname of the instance.

* **DB**
	- Name of the database this backend is connected to.

* **User**
	- PostgreSQL user.

* **Client**
	- IP address of the client connected to this backend. If this field is null, it indicates either that the client is connected via a Unix socket on the server machine or that this is an internal process such as autovacuum.

* **Query**
	- Text of this backend's most recent query. If state is active this field shows the currently executing query. In all other states, it shows the last query that was executed.

* **State**
	- Current overall state of this backend. Possible values are:
		- active: The backend is executing a query.
		- idle: The backend is waiting for a new client command.
		- idle in transaction: The backend is in a transaction, but is not currently executing a query.
		- idle in transaction (aborted): This state is similar to idle in transaction, except one of the statements in the transaction caused an error.
		- fastpath function call: The backend is executing a fast-path function.
		- disabled: This state is reported if track_activities is disabled in this backend.

Query Outliers
+++++++++++++++

Shows queries that are outliers. An outlier is a query taking longer time than the normal query of that type. Use this feature to filter out the outliers for a certain time period. After a number of samples and when ClusterControl has had enough stats, it can determine if latency is higher than normal (2 sigma + ``average_query_time``) then it is an outlier, and will be added into the *Query Outlier*.

This feature is dependent on the Top Queries feature above. If *Query Monitoring* is enabled and *Top Queries* are captured and populated, the *Query Outliers* will summarize these and provide a filter based on timestamp. You can view the query history as old as one year ago.


* **Time**
	- The exact time when the query is captured.

* **Query**
	- The SQL query.

* **Query Time**
	- Query's execution time in microseconds.

* **Avg Query Time**
	- Query's average execution time in microseconds.

* **Stdev**
	- Query's standard deviation execution time in microseconds.

* **Max Query Time**
	- Query's maximum execution time in microseconds.

* **Max Lock Time**
	- Query's lock time in microseconds.