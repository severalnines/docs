.. _PostgreSQL - Query Monitor:

Query Monitoring
----------------

Provides summary of query processing across all nodes in the cluster.

.. _PostgreSQL - Query Monitor - Top Queries:

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

.. _PostgreSQL - Query Monitor - Running Queries:

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

.. _PostgreSQL - Query Monitor - Query Outliers:

Query Outliers
+++++++++++++++

Shows queries that are outliers. An outlier is a query taking longer time than the normal query of that type. Use this feature to filter out the outliers for a certain time period. After a number of samples and when ClusterControl has had enough stats, it can determine if latency is higher than normal (2 sigma + ``average_query_time``) then it is an outlier, and will be added into the *Query Outlier*.

This feature is dependent on the *Top Queries* feature above. If *Query Monitoring* is enabled and *Top Queries* are captured and populated, the *Query Outliers* will summarize these and provide a filter based on timestamp. You can view the query history as old as one year ago.


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
	
.. _PostgreSQL - Query Monitor - Query Statistics:
	
Query Statistics
++++++++++++++++

.. Note:: This feature is introduced in v1.7.1.

Views advanced query statistics of individual PostgreSQL server. Some statistics are collected per database-level and some are server-wide, as explained in the following table:

===================================== ==========
Statistics                            Description
===================================== ==========
Access by sequential or index scans   Identify whether tables are being accessed by sequential scans or index scans.
Table I/O statistics                  Table I/O statistics. Ratio of heap bloks read from memory vs Disk I/O for a given table.
Index I/O statistics                  Disk I/O for every index on a table.
Database wide statistics              Server-wide database statistics like Datname, Numbackends, Xact_commit, Xact_rollback, Blks_read, Blks_hit, Tup_returned, Tup_fetched, Tup_inserted, Tup_updated, Tup_deleted.
Table bloat and index bloat           The estimated amount of bloat in your tables and indices.
Top 10 largest tables                 The largest top 10 tables in the selected database.
Database sizes                        Every database's size in MB.
Last analyzed or vacuumed             The last time a table was last analyzed or vacuumed.
Unused indexes                        Returns unused indexes.
Duplicate indexes                     Returns duplicate indexes.
Exclusive lock waits                  Returns exclusive lock waits.
===================================== ==========