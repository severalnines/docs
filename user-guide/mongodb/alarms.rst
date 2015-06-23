Alarms
-------

Provides a detailed explanation on the problem, and recommended action (if available) to resolve the problem. Each alarm is categorized as:

* Cluster
* Cluster recovery
* DB health
* DB performance
* Host
* Node
* Network

An alarm can be acknowledged by checking the ‘Ignore?’ checkbox. When ignored, no notification will be sent via email. An alarm cannot be deleted or dismissed.

Event Viewer
````````````

Event viewer provides unified interface to view a number of logs on all related hosts in the cluster so you can try to correlate them with respective alarms/events. 

Troubleshooting distributed database systems, where problems span across multiple hosts, often requires an admin to search through different types of log entries, from multiple hosts. This will help sysadmins analyze the relevant data and hopefully gain quicker insight into issues.

* **Alarms Window**
	- Timeline of each event/alarms. Click on each event name to view related logs to the event/alarm.

* **Search Filters**
	- Filters based on the what shows up in the log entries. Click on each of the component tag to filter out the entries. It supports following component tag:
		- Sources - Source of the log entries.
		- Clusters - Cluster identifier number.
		- Hosts - Originated host of the log message.
		- Identity - Component responsibles for the message.
		- Severity - Log level severity.

* **Show/Hide Queries**
	- Shows or hide queries window.

* **Show cmon.log event**
	- Shows all events from ClusterControl Controller log.

* **Queries**
	- Lists all queries that were running during the selected interval.