Settings
--------

Provides global settings for ClusterControl.

General Settings
````````````````

Cluster Settings
................

* **Cluster Name**
	- Cluster name. This name will appear in the database cluster list and database cluster summary.

* **Staging Area**
	- The staging area is a directory (created automatically) to store intermediate data, such as mysqldumps, binaries, RPMs used in e.g installations/upgrades and automatic scaling. The staging area is automatically cleaned, but should be big to hold mysqldumps of your entire database.

* **Sudo**
	- Sudo password if running as sudoers.

* **SSH User**
	- Read-only. The user that can SSH from the ClusterControl Server to the other nodes without password. Change in CMON configuration file under ``os_user`` or osuser.

* **SSH Identity**
	- Read-only. Change in CMON configuration file under ``ssh_identity``.

* **SSH Options**
	- Read-only. SSH options for the SSH connections. Change in CMON configuration file under ``ssh_opts``.

* **History**
	- Number of days you would like to keep historic data. Ensure you have enough disk space on the ClusterControl server if you want to store historic data.

* **Error Log Collection Interval**
	- How many minutes between automatic MySQL error log retrieval.

Configure Mailserver
....................

Configure how email notifications are to be sent. ClusterControl supports two options for sending email notifications, either using local mail commands via local MTA (Sendmail/Postfix/Exim) or using an external SMTP server. Make sure the local MTA is installed and verified with *Test Email* button.

* **Use ‘mail’**
	- Use this option to enable sendmail to send notifications. Instructions on how to set it up can be found `here <http://support.severalnines.com/entries/22897447-setting-up-mail-notifications>`_.

* **Reply-to/From**
	- Specify the sender of the email. This will appear in the ‘From’ field of mail header.

* **SMTP Mailserver**
	- SMTP mail server that you are going to use to send email.

* **SMTP Port**
	- SMTP port for mail server. Usually this value is 25 or 587, depending on your SMTP mail server configuration.

* **Username**
	- SMTP username. Leave empty if no authentication required.

* **Password**
	- SMTP password. Leave empty if no authentication required.

* **Test Email**
	- Test the mail settings. If successful, an email will be sent to all users in the *ClusterControl > Settings > General Settings > Cluster Settings > Email Notification*. Do not forget to add a recipient before pressing this button.

Many of Linux distributions come with Sendmail as default MTA. To replace Sendmail and use other MTA, e.g Postfix, you just need to uninstall Sendmail, install Postfix and start the service. Following example shows commands that need to be executed on ClusterControl node as root user for RHEL:

.. code-block:: bash

	$ service sendmail stop 
	$ yum remove sendmail -y 
	$ yum install postfix mailx cronie -y 
	$ chkconfig postfix on 
	$ service postfix start

Email Notification
.................. 

Configures email notifications for alarms generated for your database cluster.

* **Send email to**
	- Recipient email address.

* **Send digests at**
	- Send a digested (summary) email at this time every dayf or the selected recipient.

* **Timezone**
	- Timezone for the selected recipient.

* **Daily limit for non-digest email as**
	- The maximum number of non-digest email notification should be sent per day for the selected recipient. -1 for unlimited.

* **Alarm/Event Category**
	====================== ===========
	Event                  Description
	====================== ===========
	Network                Network related messages, e.g host unreachable, SSH issues.
	CmonDatabase           Internal CMON database related messages.
	Mail                   Mail system related messages.
	Cluster                Cluster related messages, e.g cluster failed.
	ClusterConfiguration   Cluster configuration messages, e.g software configuration messages.
	ClusterRecovery        Recovery messages like cluster or node recovery failures.
	Node                   Message related to nodes, e.g node disconnected, missing GRANT, failed to start HAproxy, failed to start NDB cluster nodes.
	Host                   Host related messages, e.g CPU/disk/RAM/swap alarms.
	DbHealth               Database health related messages, e.g memory usage of mysql servers, connections.
	DbPerformance          Alarms for long running transactions and deadlocks
	SoftwareInstallation   Software installation related messages.
	Backup                 Messages about backups.
	Unknown                Other uncategorized messages.
	====================== ===========

* **Select how you wants alarms/events delivered**
	======= ===========
	Action  Description
	======= ===========
	Ignore  Ignore if an alarm raised.
	Deliver Send notification immediately via email once an alarm raised.
	Digest  Send a summary of alarms raised everyday at *Send digests at*
	======= ===========

Version
........

View the database server, vendor, operating system distribution and ClusterControl version installed. Check the Check for updates button to get notified when a new ClusterControl version is released. New versions are made available from `our download site <http://www.severalnines.com/downloads/cmon>`_ and `Severalnines repository <../../installation.html#severalnines-repository>`_.

To upgrade to the latest version, see `Upgrading ClusterControl section <../../administration.html#upgrading-clustercontrol>`_.

Subscription
````````````

For users with a valid subscription (Standard, Pro, Enterprise), enter your license information here to enable additional features based on the subscription. The license key is validated during runtime. Reload your web browser after registering the license.

.. Note:: When the license expires, ClusterControl defaults back to the Community Edition.

Thresholds
``````````

Provides thresholds for warnings and criticals event. Thresholds specify the threshold level at which an alarm will be triggered and notification will be sent via email to the list of recipients configured in the `Email Notification`_. Set your alarm thresholds for:

* CPU, RAM, Disk Space utilization
* MySQL Server Memory utilization
* DataMemory, IndexMemory, Tablespace, REDO Log and Buffer utilization (MySQL Cluster)

========= ===========
Level     Description
========= ===========
Warning   Sets your warning threshold in percentage for specific event.
Critical  Sets your critical threshold in percentage for specific event.
========= ===========

Graphs
``````

Manages graph settings and data capturing. From here, it is possible to record up to 83 different MySQL counters. You can choose to view up to 8 counters on this page. The settings configured here affects *ClusterControl > Performance > Overview* page.

* **ON?**
	- Choose counters you want to record and graph.

* **Search**
	- Filters the status variables available in the counter list.

* **VAR**
	- Choose the status variables that you want to track. Tick the 'On' checkbox to the left of the counter that you want to record. For detailed explanation on status variables of your cluster, you can refer to following pages:
		- MySQL: http://dev.mysql.com/doc/refman/5.6/en/server-status-variables.html
		- Galera Cluster: http://galeracluster.com/documentation-webpages/galerastatusvariables.html
		- MySQL Cluster: http://dev.mysql.com/doc/refman/5.6/en/mysql-cluster-status-variables.html

* **Graph 1-8**
	- Choose the enabled counter to be graphed.
	
* **Save Graph**
	- Applies your selection.
	
Query Monitor
``````````````

Manages how ClusterControl should perform query monitoring. It determines the output of:

* *ClusterControl > Overview > Cluster-Wide Queries*
* *ClusterControl > Query Monitoring > Top Queries*
* *ClusterConrol > Query Monitoring > Query Histogram*

Changes happened in this page does not require the CMON service to restart.

* **Query Sample Time**
	- How many seconds between query sampling. Queries are sampled in 30 seconds chunks. This time sets how long the interval should be between each chunk.
		- -1 - disabled (if you want to manage the query monitoring yourself)
		- 0 - all the time.
	- SQL performance is affected if *Query Sample Time* is too frequent and you have a lot of query traffic since the slow query log file will grow big. Setting this to 5 is a good starting point. You can also set the *Long Query Time* to capture queries taking longer than a certain time.

* **MySQL Local Query Override**
	- Choose whether you want ClusterControl to override the local MySQL query sampling:
		- Yes - The local MySQL configuration settings inside ``my.cnf`` for ``long_query_time`` and ``log_queries_not_using_indexes`` will be used.
		- No - ClusterControl uses *Long Query Time* and *Log queries not using indexes* will be used across all MySQL Servers.

* **Long Query Time**
	- Collects queries taking longer than Long Query Time seconds:
		- 0 - All queries.
		- 0.1 - Only queries taking more than 0.1 seconds will be accounted.

* **Log queries not using indexes?**
	- Configures ClusterControl behaviour on sampling queries without indexes:
		- Yes - Logs queries which are using indexes.
		- No - Ignores queries that are not using indexes (will not be accounted for in *ClusterControl > Query Monitor > Query Histogram*).
		
Backup
``````

Manages the default backup directory and retention period.

* **Backup Directory**
	- Default backup directory. This directory will be created automatically if does not exist on the target node.

* **Backup Retention Period**
	- Backup retention period in days. Backups older than the retention period (days) will be deleted. Set to 0 for no retention.
