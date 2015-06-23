Performance
-----------

Database performance monitoring.

Mongo Stats
````````````

Quick overview of the status of a currently running mongod or mongos instance similar to mongostat command collected every ``db_stats_collection_interval`` (configured inside CMON configuration file).

* **Queries**
	- The number of query operations per second.

* **Inserts**
	- The number of objects inserted into the database per second. If followed by an asterisk (e.g. *), the datum refers to a replicated operation.

* **Updates**
	- The number of update operations per second.

* **Deletes**
	- The number of delete operations per second.

* **Command**
	- The number of commands per second.

* **Getmore**
	- The number of get more (i.e. cursor batch) operations per second.

* **Vsize**
	- The amount of virtual memory in megabytes used by the process at the time of the last mongostat call.

* **Res**
	- The amount of resident memory in megabytes used by the process at the time of the last mongostat call.

* **Faults**
	- The number of page faults per second.

* Locked(%)
	- The percent of time in a global write lock.

* idx miss(%)
	- The percent of index access attempts that required a page fault to load a btree node.

* **qr|qw**
	- qr - The length of the queue of clients waiting to read data from the MongoDB instance.
	- qw - The length of the queue of clients waiting to write data from the MongoDB instance.

* **ar|aw**
	- ar - The number of active clients performing read operations.
	- aw - The number of active clients performing write operations.

* **NetIn**
	- The amount of network traffic, in bytes, received by the MongoDB instance.

* **NetOut**
	- The amount of network traffic, in bytes, sent by the MongoDB instance.

* **Conn**
	- The total number of open connections.

* **Time**
	- The last time ClusterControl fetch for node's status.

Performance
````````````

Provides an overview of database operations by type and makes it possible to analyze the load on the database in more granular manner.

* **opcounters**
	- An aggregated view of all opscounters in a single graph.

* **opcounters.query**
	- Provides a graph of the total number of queries received since the mongod instance last started.

* **opcounters.insert**
	- Provides a graph of the total number of insert operations received since the mongod instance last started.

* **opcounters.update**
	- Provides a graph of the total number of update operations received since the mongod instance last started.

* **opcounters.delete**
	- Provides a graph of the total number of delete operations since the mongod instance last started.

* **opcounters.getmore**
	- Provides a graph of the total number of “getmore” operations since the mongod instance last started. 

* **opcounters.command**
	- Provides a graph of the total number of commands issued to the database since the mongod instance last started.