.. _mysql-query-monitor:

Query Monitoring
----------------

Provides summary of query processing across all nodes in the cluster.

Top Queries
````````````

This is an aggregated list of all your top queries running on all the nodes of your database cluster. The list can be ordered by Occurrence or Execution Time, to show the most common or slowest queries respectively. It is also possible to filter and review queries from one particular node.

Toggle Query Monitor to ON to enable query monitoring. If Performance Schema is enabled, ClusterControl will use it to look for the slow queries. Otherwise, ClusterControl will parse the content of MySQL slow query log based on the following flow:

Start: 
	1. Start slow log.
	2. Run it for a short period of time (a second or couple of seconds).
	3. Stop log.
	4. Parse log.
	5. Truncate log (new log file).
	6. Go to Start.

The collected queries will be hashed and calculated (normalize, average, count, sort) before populated into ClusterControl UI.

.. Attention:: By using slow query log, there is a slight chance some queries will not be captured, especially during “stop log, parse log, truncate log” parts. You can enable Performance Schema if this is not an option.

Settings
''''''''

Click on the Settings to configure the Query Monitor settings, as explained below:

* **Long Query Time**
	- Collects queries taking longer than Long Query Time seconds:
		- 0 - All queries.
		- 0.1 - Only queries taking more than 0.1 seconds will be accounted.

.. Note:: If query monitoring is enabled but the data is not populated, consider to lower down the *Long Query Time* value, which telling ClusterControl to parse more queries and calculate a more accurate result.

* **Log queries not using indexes?**
	- Configures ClusterControl behaviour on sampling queries without indexes:
		- Yes - Logs queries which are using indexes.
		- No - Ignores queries that are not using indexes (will not be accounted for in *ClusterControl > Query Monitor > Query Histogram*).

* **MySQL Local Query Override**
	- Choose whether you want ClusterControl to override the local MySQL query sampling:
		- Yes - The local MySQL configuration settings inside ``my.cnf`` for ``long_query_time`` and ``log_queries_not_using_indexes`` will be used.
		- No - ClusterControl uses *Long Query Time* and *Log queries not using indexes* will be used across all MySQL Servers.

Take note that you can also review the same settings under the legacy page at *ClusterControl > Settings > Query Monitor*.

Top Queries Table
''''''''''''''''''

This page is auto-refresh every 30 seconds. You can change the refresh rate by clicking on *Refresh rate* dropdown at top right. The following describes the Top Queries table columns:

* **Query**
	- List of sampled queries.

* **DB**
	- Database name.

* **Count**
	- Total number of query occurrences.

* **Rows**
	- Total rows involved:
		- Sent - The number of rows MySQL returned.
		- Examined - The number of rows MySQL believes it must examine to execute the query.

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
````````````````

View current running queries on your database cluster similar to ``SHOW FULL PROCESSLIST`` command in MySQL. You can stop a running query by selecting to kill the connection that started the query. The processlist can be filtered out by host.

This page is auto-refresh every 30 seconds. You can change the refresh rate by clicking on *Refresh rate* dropdown at top right.

* **MySQL Server**
	- The MySQL server from which the process is retrieved.

* **Kill [thread ID]**
	- Click to kill the specific MySQL thread ID.

* **ID**
	- Connection identifier number.

* **DB**
	- Database name.

* **User**
	- The MySQL user who issued the statement.

* **Exec T**
	- The time in seconds that the thread has been in its current state.

* **Client**
	- The hostname (or port if TCP/IP) of the client issuing the statement.

* **Info**
	- The statement the thread is executing, or NULL if it is not executing any statement.

* **Command**
	- The type of command the thread is executing.

* **State**
	- An action, event, or state that indicates what the thread is doing, as explained in `MySQL Documentation <http://dev.mysql.com/doc/refman/5.6/en/general-thread-states.html>`_ page.

Query Histogram
````````````````

Use this feature to filter the query activity for a certain time period. This feature is relative to Top Queries. If Query Monitoring is enabled and Top Queries is captured and populated, Query Historgram will summarize it and provide a filter based on timestamp. You can view the query history as old as one year ago.

* **Time**
	- The exact time when the query is captured.

* **Query**
	- The SQL query.

* **Query Time**
	- Query's execution time in microseconds.

* **Avg Query Time**
	- Query's average execution time in microseconds.

* **Stdev**
	- Query's standard devioation execution time in microseconds.

* **Max Query Time**
	- Query's maximum execution time in microseconds.

* **Max Lock Time**
	- Query's lock time in microseconds.