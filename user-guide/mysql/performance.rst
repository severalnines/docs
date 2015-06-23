Performance
-----------

Database performance monitoring and advisors.

Overview
````````

You can view graphs of different database counters on this page. You can record up to 36 different MySQL counters.

* **Choose Graph**
	- Choose which counters to record.

* **Search**
	- Filter the status variables available in the counter list.
	- Choose the status variables that you want to track. Check the 'On' box to the left of the counter that you want to record.
	
For detailed explanation on status variables of your cluster, you can refer to following pages:
		- MySQL: http://dev.mysql.com/doc/refman/5.6/en/server-status-variables.html
		- Galera Cluster: http://galeracluster.com/documentation-webpages/monitoringthecluster.html
		- MySQL Cluster: http://dev.mysql.com/doc/refman/5.6/en/mysql-cluster-status-variables.html

Advisors
````````

Lists of scheduled Advisors created in Developer Studio using `ClusterControl DSL <../../dsl.html>`_.

* **Expand all**
	- Expands all advisors with detailed.

* **Collapse all**
	- Lists the advisors summary.

* **Filter by status**
	- Shows advisors based on status - Ok, Warning, Critical.
	
* **DB Instance**
	- The database server the advisor running on.

* **Status**
	- Advisor status - Ok, Warning, Critical.

* **Advice**
	- The advisor's decision based on the justification.

* **Justification**
	- The result of advisors' execution.

Health Report
``````````````

ClusterControl uses a number of best practice advisors to create a Health Report for your cluster. These advisors automatically examine your node configurations and performance levels, and identify any deviations from best practice rules. 

This tab provides a quick overview of the overall health of your database nodes. A new tab will appear for each category of Advisors, in case there are alarms in that category. See `Database Performance Advisor`_ section for detailed explaination.

* **Help**
	- Health Report Guide - This will open the ClusterControl documentation page for health report status in details.
	- Inquire a Health Check - This will send an email request to our engineers so we can schedule a health check up on your database cluster.

.. Note:: Please note that consulting requests are for things like review of database configurations, slow queries or any other requests that are not covered by our ClusterControl support. Consulting hours can be purchased, our sales team will then be in contact with you to arrange this.

* **Full Report**
	- Query optimization - Full Table Scans, Select Full Join, Select Range Check, Sort Passes
	- InnoDB - Buffer Pool Hit Ratio, Wait for Checkpoint, Wait for REDO log
	- Connections - Current Connections, Maximum Connections Seen
	- Database Memory usage - Current Memory Usage, Max Memory Usage Seen
	- Open files - Open Files Ratio
	- Cache usage - Table Cache Hit Ratio, Table Cache Usage, Thread Cache Hit Ratio
	- Locks - Table Lock Contention
	- Temporary tables usage - Temporary Disk Table Ratio

Database Performance Advisor
''''''''''''''''''''''''''''

ClusterControl advisor rules are a set of MySQL best practices that allow DBAs and sysadmins to manage the dynamic nature of their MySQL servers over time. The Health Report module monitors all database nodes for adherence to MySQL recommended configuration and server settings. Below is a description of each advisor, as well as some instructions on how to resolve any exceptions raised.

Memory Usage
............

Overview of memory usage by MySQL. 

- Fields:
	- System Memory - Detected system’s memory.
	- Max Memory Configured - Maximum memory configured for MySQL service, which is equal to Global Buffers + Thread Max Configured Memory
	- Max Memory Ever Used - Maximum memory ever used by MySQL service (all time).
	- Max Memory Curr Used - Maximum memory currently used by MySQL service.
	- Global Buffers - Total of memory allocated for global buffers during MySQL service startup:
		- innodb_buffer_pool_size
		- innodb_additional_memory_pool_size
		- innodb_log_buffer_size
		- key_buffer_size
		- query_cache_size
	- Thread Max Configured Memory - Total of memory allocated for MySQL threads.
	- Thread Max Ever Used Memory - Maximum memory ever used by MySQL threads (all time).
	- Thread Max Curr Used Memory - Maximum memory currently used by MySQL threads.

- Calculation:

.. math::

	Max\ Memory\ Configured = Global\ Buffers + Thread\ Max\ Configured\ Memory

where,

.. math::

	Global\ Buffers& = innodb\_buffer\_pool\_size \\
	& \quad + innodb\_additional\_mem\_pool\_size \\
	& \quad + innodb\_log\_buffer\_pool\_size \\
	& \quad + query\_cache\_size \\
	& \quad + key\_buffer\_size
	
.. math::

	Thread\ Max\ Configured\ Memory& = max\_connections \times \\
	& \quad (read\_buffer\_size \\
	& \quad + read\_rnd\_buffer\_size \\
	& \quad + sort\_buffer\_size \\
	& \quad + thread\_stack \\
	& \quad + join\_buffer\_size \\
	& \quad + binlog\_cache\_size)

- Threshold:
	- OK < 90
	- Warning >= 90
	- Critical >= 92

- Recommendation:
	- A warning indicates that the MySQL server is using most of the available memory, with less than 10% of the remaining memory to other processes.
	- The most significant variables would be ``innodb_buffer_pool_size`` or ``max_connections``. Adjusting these two values will mostly determined the Max Memory Configured. Max Memory Configured is the most important here. If it is greater than System RAM then there is a chance that the MySQL Server process will terminate with an OOM exception, if all connections are used. This alarm may affect stability.
 

Query
.....

Query related statistics since the last MySQL restart.

FULL TABLE SCANS
++++++++++++++++

- The ratio of full table scans, an operation that requires reading the entire contents of a table, rather than just selected portions using an index. Impacts performance, not stability.

- Calculation:

.. math::

	Full\ table\ scans (\%) = (\frac{handler\_read\_rnd\_next + handler\_read\_rnd}{handler\_read\_rnd\_next + handler\_read\_rnd + handler\_read\_first + handler\_read\_next + handler\_read\_key + handler\_read\_prev}) \times 100

- Threshold:
	- OK < 25
	- Warning >= 25
	- Critical >= 40

- Recommendation:
	- This value is high if you are doing a lot of queries that require sorting of results or table scans. Generally this suggests that tables are not properly indexed or that your queries are not written to take advantage of the indexes you have. 
	- Examine the Query Monitor output to find out which queries they are.
 

SELECT FULL JOIN
++++++++++++++++

- The number of joins that perform table scans because they do not use indexes. Impacts performance, not stability.

- Calculation:

.. math::

	Select\ full\ join = select\_full\_join

- Threshold:
	- OK <= 10
	- Warning > 10

- Recommendation:
	- If this value is not 0, you should carefully check the indexes of your tables. Set *Query Sample Interval = 1* and lower the Long Query Time value under *ClusterControl > Settings > Query Monitor* if you don't find any queries in the Query Monitor.
 

SELECT RANGE CHECK
++++++++++++++++++

- The number of joins without keys that check key usage after each row. Impacts performance, not stability.

Calculation:

.. math::

	Select\ range\ check = select\_range\_check

- Threshold:
	- OK <= 10
	- Warning > 10

- Recommendation:
	- If this is not 0, you should carefully check the indexes of your tables. If you don't find any queries in the *Query Monitor*, set *Query Sample Interval = 1* and lower the Long Query Time value under *ClusterControl > Settings > Query Monitor*.
 

SORT PASSES
+++++++++++

- The ratio of merge passes that the sort algorithm has had to do. Impacts performance, not stability.

- Calculation:

.. math::

	Sort\ passes = \frac {sort\_merge\_passes}{sort\_scan + sort\_range}

- Threshold:
	- OK < 3
	- Warning > 3
	- Critical > 20

- Recommendation:
	- If this value is high, you should consider increasing the value of ``sort_buffer_size`` and ``read_rnd_buffer_size``. Increase in small increments until the message disappears.
 

InnoDB
......

InnoDB related statistics since the last MySQL startup restart. 

INNODB BUFFER POOL HIT RATIO
++++++++++++++++++++++++++++

- Ratio of how often your pages are retrieved from memory instead of disk. Impacts performance, not stability.

- Calculation:

.. math:: 

	InnoDB\ buffer\ pool\ hit\ ratio(\%) = 1000 - (1000 \times \frac {innodb\_buffer\_pool\_reads}{innodb\_buffer\_pool\_read\_requests + innodb\_buffer\_pool\_reads})

- Threshold:
	- OK > 999
	- Warning <= 999
	- Critical <= 998

- Recommendation:
	- Increase ``innodb_buffer_pool_size`` or remove redundant indexes. 
	- If the value is low during early MySQL startup, please allow some time for the buffer pool to warm up.
 

INNODB DIRTY PAGES
++++++++++++++++++

- Ratio of how often InnoDB needs to be flushed. Impacts performance, not stability.

- Calculation:

.. math::

	InnoDB\ dirty\ pages(\%) = \frac{innodb\_buffer\_pool\_pages\_dirty}{innodb\_buffer\_pool\_pages\_total}

- Threshold:
	- OK < 75
	- Warning >= 75
	- Critical >= 86

Recommendation:
	- During write heavy load, it is normal that this percentage increases. If the percentage of dirty pages stays high for a long time, you may want to increase the buffer pool and/or get faster disks to avoid performance bottlenecks.
 

INNODB WAIT FOR CHECKPOINT
++++++++++++++++++++++++++

- Ratio of how often InnoDB needs to read or create a page where no clean pages are available. Impacts performance, not stability.

- Calculation:

.. math::

	InnoDB\ wait\ for\ checkpoint = \frac{innodb\_buffer\_pool\_wait\_free}{innodb\_buffer\_pool\_write\_requests}

- Threshold:
	- OK < 1
	- Warning = 1
	- Critical = 1

- Recommendation:
	- Normally, writes to the InnoDB Buffer Pool happen in the background. However, if it is necessary to read or create a page and no clean pages are available, it is also necessary to wait for pages to be flushed first. The ``innodb_buffer_pool_wait_free`` counter counts how many times this has happened. 
	- ``innodb_buffer_pool_wait_free`` greater than 0 is a strong indicator that the InnoDB buffer pool is too small, and operations had to wait on a Checkpoint. Increase the ``innodb_buffer_pool_size``.
 

INNODB WAIT FOR REDOLOG
+++++++++++++++++++++++

- Ratio of redo log contention. Impacts performance, not stability.

- Calculation:

.. math::

	InnoDB\ wait\ for\ redolog = \frac{innodb\_log\_waits}{innodb\_log\_writes}

- Threshold:
	- OK < 1
	- Warning = 1
	- Critical = 1

- Recommendation:
	- Check ``innodb_log_waits`` and if it continues to increase (from ClusterControl version 1.2.9 you can create a custom Dashbord monitoring this variable) then increase the ``innodb_log_buffer_size``.
	- It can also mean that the disks are too slow and cannot sustain disk IO, perhaps due to peak write load.
 

Connections
...........

MySQL connection statistics since last MySQL restart.

MAX CONNECTIONS CURRENT
+++++++++++++++++++++++

- The ratio of currently open connections (connection thread). Impacts stability.

- Calculation:

.. math::

	Max\ connections\ current(\%) = \frac{threads\_connected}{max\_connections} \times 100

- Threshold:
	- OK < 80
	- Warning >= 80
	- Critical >= 90

- Recommendation:
	- If the ratio is high, it indicates there are many concurrent connections to the MySQL server and could lead to 'too many connections' error. Try increasing the ``max_connections`` value or inspect the connections using ``SHOW FULL PROCESSLIST``.

MAX CONNECTIONS EVER SEEN
+++++++++++++++++++++++++

- The ratio of maximum connections to MySQL server that was ever seen. Impacts stability.

- Calculation:

.. math::

	Max\ connections\ ever\ seen(\%) = (\frac{max\_used\_connections}{max\_connections}) \times 100

- Threshold:
	- OK < 80
	- Warning >= 80
	- Critical >= 90

- Recommendation:
	- If the ratio is high, it indicates the MySQL has once reached a high number of connections and would lead to 'too many connections' error. Inspect the ``MAX CONNECTIONS CURRENT`` ratio for more information.
 

Memory
......

Percentage of system RAM used by MySQL server.

MYSQL MEMORY USAGE CURR
+++++++++++++++++++++++

- Percentage of system RAM used by MySQL server.

- Calculation:

.. math::

	MySQL\ memory\ usage\ current(\%) &= threads\_connected \times \\
	& \quad (read\_buffer\_size \\
	& \quad + read\_rnd\_buffer\_size \\
	& \quad + sort\_buffer\_size \\
	& \quad + thread\_stack \\
	& \quad + join\_buffer\_size \\
	& \quad + binlog\_cache\_size)

- Threshold:
	- OK < 90
	- Warning >= 90
	- Critical >= 92

- Recommendation:
	- If the host is swapping, increase RAM or lower ``innodb_buffer_pool_size`` or ``max_connections``.
 

MYSQL MEMORY USAGE EVER
+++++++++++++++++++++++

- Maximum percentage of system RAM that has ever been used by MySQL Server. 

- Calculation:

.. math::
	MySQL\ memory\ usage\ ever(\%) &= max\_used\_connections \times \\
	& \quad (read\_buffer\_size \\
	& \quad + read\_rnd\_buffer\_size \\
	& \quad + sort\_buffer\_size \\
	& \quad + thread\_stack \\
	& \quad + join\_buffer\_size \\
	& \quad + binlog\_cache\_size)

- Threshold:
	- OK < 90
	- Warning >= 90
	- Critical >= 92

- Recommendation:
	- If the host is swapping, increase RAM or lower ``innodb_buffer_pool_size`` or ``max_connections``.

Files
.....

Files-related performance since the last MySQL restart.

OPEN FILES RATIO
+++++++++++++++++

- The ratio of files that are open. Impacts performance, not stability.

- Calculation:

.. math::

	Open\ files\ ratio(\%) = (\frac{open\_files}{open\_files\_limit}) \times 100

- Threshold:
	- OK <= 80
	- Warning > 80
	- Critical > 90

- Recommendation:
	- Increase the system’s open files ulimit. The default value might be to low. Please refer to `this knowledgebase article <http://support.severalnines.com/entries/24464231-Adjust-Open-Files-Limit>`_ on how to do it.
 

Cache
.....

Table cache performance since the last MySQL restart.

TABLE CACHE USAGE
+++++++++++++++++

- The ratio of table cache usage for all threads. Impacts performance, not stability.

- Calculation:

.. math::

	Table\ cache\ usage(\%) = (\frac{opened\_tables}{table\_open\_cache}) \times 100

- Threshold:
	- OK < 80
	- Warning >= 80
	- Critical >= 90

- Recommendation:
	- Increase ``table_open_cache`` variable until the alarm message disappears.
 

TABLE CACHE HIT RATIO
+++++++++++++++++++++

- The ratio of table cache hit usage. Impacts performance, not stability.

- Calculation:

.. math::

	Table\ cache\ hit\ ratio(\%) = (\frac{open\_tables}{opened\_tables}) \times 100

- Threshold:
	- OK > 90
	- Warning <= 90
	- Critical <= 80

- Recommendation:
	- Increase ``table_open_cache`` variable until the alarm message disappears.
 

Locking
.......

TABLE LOCK CONTENTION
+++++++++++++++++++++

- Table lock contention ratio. Impacts performance, not stability (may impact stability on Galera clusters).

- Calculation:

.. math::

	Table\ lock\ contention(\%) = (\frac{table\_locks\_waited}{table\_locks\_waited + table\_locks\_immediate}) \times 100

- Threshold:
	- OK < 1
	- Warning >= 1
	- Critical >= 1

- Recommendation:
	- You have queries or operations that are locking tables, thus preventing concurrency (look for ``LOCK TABLE`` etc). If you are using MyISAM, change the storage engine to InnoDB if possible.

Status Time Machine
````````````````````

By default, this feature is disabled until you set ``enable_mysql_timemachine=1`` inside CMON configuration file. The status time machine allows you to select status variable for a time range and compare the values at the start and end of that range. The table shows the selected status variables for the given range. Use the slider at the end of the table change the time range.

* **Filter Stats**
	- Open the Filter Stats window.

* **Apply Filter**
	- Apply the search based on available selected filters.

* **Clear Filter**
	- Clear the selected filters.

* **Search**
	- Filter the result based on defined search text.

* **Show only changed values**
	- Show results with changed values only.

* **Start Value**
	- The status/variables value on the start time. (left scroller)

* **End Value**
	- The status/variables value on the end time. (right scroller)

* **Diff/Second**
	- The difference between values on start time and end time divide by the amount of time in seconds between those ranges.
	
DB Status
``````````

DB Status provides a quick overview of MySQL status across all your database nodes. You can use the *Search* text field to filter the result.

.. Note:: You can check *Hide all zero values* to filter out any status that returned 0.

DB Variables
````````````

DB Variables provide a quick overview of MySQL variables that are set across all your database nodes. You can use the *Search* text field to filter the result.

.. Note:: Red text means that the variable setting is different. In some cases that is acceptable (e.g., IP address of the node).

DB Growth
``````````

Provides a summary of your database and table growth on daily basis for the last 30 days. On the first line of the *Top 25 Largest Databases* grid, you should notice the actual size of MySQL data directory (with a folder icon). This is useful to determine whether any other files exist in the data directory may consume huge spaces e.g binary log, error log or MySQL general log.

Click on a database listed for further details on growth summary per table.

InnoDB Status
``````````````

Fetches the current InnoDB monitor output for selected host, similar to ``SHOW ENGINE INNODB STATUS`` command.

Schema Analyzer
````````````````

Analyzes your database schemas for missing primary keys, redundant indexes and tables using the :term:`MyISAM` storage engine. Galera Cluster needs an explicitly defined primary keys on each table (unique key does not count). MyISAM tables are not recommended in Galera. ClusterControl will periodically check the schemas for these (default every 8 hours or every CMON restart), and raise an alert if necessary.

* **Show tables without Primary Keys**
	- List of tables without primary keys. Primary key is important in Galera. DELETE operations are unsupported on tables without a primary key. Also, rows in tables without a primary key may appear in a different order on different nodes.

* **Show MyISAM Tables**
	- MyISAM does not support transactions. However, the DMLs for MyISAM should also work but its still experimental in Galera.

* **Show Redundant Indexes**
	- Having duplicate keys in schemas can hurt the performance of database:
		- They make the optimizer phase slower because MySQL needs to examine more query plans.
		- The storage engine needs to maintain, calculate and update more index statistics.
		- DML and even read queries can be slower because MySQL needs update fetch more data to Buffer Pool for the same load.
		- Data needs more disk space so the backups will be bigger and slower.

Transaction Log
````````````````

Lists of long-running transactions and deadlocks across database cluster where you can easily view what transactions are causing the deadlocks. The timeout is 10 seconds by default. This is configurable in CMON configuration file under ``db_long_query_time_alarm`` configuration option. 

Click on the listed query to see the output of InnoDB status for detailed debugging.

* **Db instance**
	- Database instance that process the transaction.

* **Host**
	- Host that performs the transaction.

* **Db**
	- Database name.

* **Tx Id**
	- Transaction ID.

* **Blocking Tx Id**
	- Transaction ID that blocked the actual transaction.

* **Query**
	- Query executed inside the transaction.

* **Duration (sec)**
	- The duration of long running transaction.

* **Last Seen**
	- When was the last time ClusterControl has seen the error.

