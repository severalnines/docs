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
	
Custom Expressions
``````````````````

Creates expressions to raise custom alarms. An expression statement is currently limited be using MySQL variables only. The returned value must be in form of percentage, where it does not exceed 100. This will allow ClusterControl to raise alarm based on the exceeded threshold. Clicking on an expression will open a histogram related to it at the bottom window.

By default, ClusterControl provides a number of example expressions. It makes use of MySQL status and variables collected from all database nodes. To create a new expression, click on *Create New Alert* button.

* **Group**
	- Enter a new expression group name or select an existing group.

* **Expression**
	- Specify an expression that will return a percentage value. The expression can contain the following symbols:
		* \+ (addition)
		* \- (subtraction)
		* \* (multiplication)
		* / (division)
		* ^ (exponent)
		* () (parentheses)
		* MySQL variables and status (*ClusterControl > Performance > DB Status* or *DB Variables*)
	- The expression formula will comply to rules of precedence: 
		1. Parentheses
		2. Exponents
		3. Multiplication and Division
		4. Addition and Subtraction

* **Active?**
	- Activate the expression. ClusterControl executes the expression every ``db_stats_collection_interval`` defined in CMON configuration file.

* **Applies On**
	- Choose the server you want this expression to run on.

* **Expression Name**
	- Give the expression a name.

* **Trigger**
	- Trigger directions:
		- > - The returned value is higher than.
		- < - The returned value is lower than.

* **Warning Level**
	- Set the warning percentage level to raise an alarm.

* **Critical Level**
	- Set the critical percentage level to raise an alarm.

* **Send Notification?**
	- Allow ClusterControl to send notification for this custom expression.

* **Threshold breaches before notify**
	- How many times the threshold is breached before ClusterControl raise an alarm and send a notification. This caters for flapping behaviour, or rapid, repeated changes to the monitored state close to threshold values.

* **Last Execution Error**
	- Shows error (if any) if the last execution fails.
