Query Monitor
-------------

Provides summary of query processing across all nodes in the cluster.

Top Queries
++++++++++++

This is an aggregated list of all your top queries running on all the nodes of your database cluster. The list can be ordered by Occurrence or Execution Time, to show the most common or slowest queries respectively. It is also possible to filter and review queries from one particular node.

ClusterControl gets the information in two different ways:

- Queries are retrieved from PERFORMANCE_SCHEMA
- If PERFORMANCE_SCHEMA is disabled or unavailable, ClusterControl will parse the content of the Slow Query Log

For more details (including how to enable the PERFORMANCE_SCHEMA), see this blog post, `How to use the ClusterControl Query Monitor for MySQL, MariaDB and Percona Server <http://severalnines.com/blog/how-use-clustercontrol-query-monitor-mysql-mariadb-and-percona-server>`_.

Toggle Query Monitor to ON to enable query monitoring. If Performance Schema is enabled, ClusterControl will use it to look for the slow queries. Otherwise, ClusterControl will parse the content of MySQL slow query log based on the following flow:

Start: 
	1. Start slow log (during MySQL runtime).
	2. Run it for a short period of time (a second or couple of seconds).
	3. Stop log.
	4. Parse log.
	5. Truncate log (new log file).
	6. Go to Start.

The collected queries are hashed, calculated and digested (normalize, average, count, sort) and then stored in ClusterControl.

.. Attention:: By using slow query log, there is a slight chance some queries will not be captured, especially during "stop log, parse log, truncate log" parts. You can enable Performance Schema if this is not an option.

If you are using the Slow Query log, only queries that exceed the *Long Query Time* will be listed here. If the data is not populated correctly and you believe that there should be something in there, it could be:

- ClusterControl did not collect enough queries to summarize and populate data. Try to lower the *Long Query Time*.
- You have configured Slow Query Log configuration options in the ``my.cnf`` of MySQL server, and *Override Local Query* is turned off. If you really want to use the value you defined inside ``my.cnf``, probably you have to lower the ``long_query_time`` value so ClusterControl can calculate a more accurate result.
- You have another ClusterControl node pulling the Slow Query log as well (in case you have a standby ClusterControl server). Only allow one ClusterControl server to do this job.

The *Long Query Time* value can be specified to a resolution of microseconds, for example 0.000001 (1 x 10 :superscript:`-6`).

Settings
``````````

Click on the Settings to configure the Query Monitor settings, as explained below:

* **Long Query Time**
	- Collects queries taking longer than Long Query Time seconds, for example:
		- 0.1 - Only queries taking more than 0.1 seconds will be accounted.

* **Log queries not using indexes?**
	- Configures ClusterControl behavior on sampling queries without indexes:
		- Yes - Logs queries which are using indexes.
		- No - Ignores queries that are not using indexes (will not be accounted for in *ClusterControl > Query Monitor > Query Outliers*).

* **MySQL Local Query Override**
	- Choose whether you want ClusterControl to override the local MySQL query sampling:
		- Yes - The local MySQL configuration settings inside ``my.cnf`` for ``long_query_time`` and ``log_queries_not_using_indexes`` will be used.
		- No - ClusterControl uses *Long Query Time* and *Log queries not using indexes* will be used across all MySQL Servers.

* **Auto-Purge Queries**
	- Automatically purge sampled queries hourly. If PERFORMANCE_SCHEMA is enabled, the table ``events_statements_summary_by_digest`` will be auto-purged (using ``TRUNCATE TABLE`` statement) every hour. By default this is disabled.
	
* **Purge Query Monitor**
	- Wipes out the sampled queries. The sampling will be refreshed again after ``db_stats_collection_interval``.

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
+++++++++++++++

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