.. _mongodb-ops-monitor:

Ops Monitoring
----------------

Provides summary of database operations across all nodes in the cluster.

Running Operations
``````````````````

Provides view of current running operations similar to ``db.currentOp()`` command. Click on an entry to see details on the bottom window.

* **Show All Queries**
	- Shows all information on all operations, including operations on idle connections and system operations.

* **Show Only Queries on Replica Set**
	- Shows all information on all operations, including operations on idle connections and system operations.

* **ID**
	- The identifier for the operation. See `currentOp.opid <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.opid>`_.

* **Client**
	- The IP address (or hostname) and the ephemeral port of the client connection where the operation originates. See `currentOp.client <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.client>`_.

* **Replica Set**
	- The replica set where the operation is run against.

* **Active**
	- Auto checked if the operation has started or none if the operation is idle, such as an idle connection or an internal thread that is currently idle. See `currentOp.active <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.active>`_.

* **Exec T**
	- Execution time of the operation.

* **Op**
	- Type of operation. See `currentOp.op <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.op>`_.

* **NS**
	- The namespace the operation targets. See `currentOp.ns <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.ns>`_.

* **Query**
	- Information on operations whose op value is not "insert". See `currentOp.query <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.query>`_.

* **Thread ID**
	- Identifier for the thread that handles the operation and its connection. See `currentOp.threadId <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.threadId>`_.

* **Connection ID**
	- An identifier for the connection where the operation originated. See `currentOp.connectionId <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.connectionId>`_.

* **Waiting Lock**
	- Auto checked if the operation is waiting for a lock and none if the operation has the required lock. See `currentOp.waitingForLock <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.waitingForLock>`_.

* **Locked (r^w)us**
	- The amount of time in microseconds that the operation has spent both acquiring and holding locks. See `currentOp.lockStats <http://docs.mongodb.org/v2.6/reference/method/db.currentOp/#currentOp.lockStats>`_.

* **Acquiring (r^w)us**
	- Cumulative time in microseconds that the operation had to wait to acquire the locks. See `currentOp.lockStats.timeAcquiringMicros <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.lockStats.timeAcquiringMicros>`_.

* **Desc**
	- Description of the client. See `currentOp.desc <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.desc>`_.

* **Msg**
	- Provides a message that describes the status and progress of the operation. See `currentOp.msg <http://docs.mongodb.org/manual/reference/method/db.currentOp/#currentOp.msg>`_.
