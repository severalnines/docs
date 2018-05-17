Settings
--------

Settings for the cluster including general settings, mail notifications and thresholds.

CMON Settings
++++++++++++++

General Settings
``````````````````

* **Cluster Name**
	- Cluster name. This name will appear in the database cluster list and database cluster summary.

* **Staging Area**
	- The staging area is a directory (created automatically) to store intermediate data, such as dump files, binaries, packages used in installations, upgrades or scaling. The staging area is automatically cleaned, but should be big enough to hold dump files of your entire database.

* **Sudo**
	- Sudo command password (if *SSH User** is not root) if running as sudoers. Change in CMON configuration file under ``sudo``.

* **SSH User**
	- Read-only. The user that can SSH from the ClusterControl Server to the other nodes without password. Change in CMON configuration file under ``os_user`` or ``osuser``.

* **SSH Identity**
	- Read-only. Change in CMON configuration file under ``ssh_identity``.

* **History**
	- Number of days you would like to keep historic data. Ensure you have enough disk space on the ClusterControl server if you want to store historic data.

* **Log Interval**
	- How many minutes between automatic MySQL error log retrieval.

Configure Mail Server
``````````````````````

Configures how email notifications should be sent out. ClusterControl supports two options for sending email notifications, either using local mail commands via local MTA (Sendmail/Postfix/Exim) or using an external SMTP server. Make sure the local MTA is installed and verified using *Test Email* button.

Option 1: Sendmail
'''''''''''''''''''

* **Use sendmail**
	- Use this option to enable sendmail to send notifications. Instructions on how to set up Sendmail can be found in this guide, `Setting up mail notifications <http://support.severalnines.com/entries/22897447-setting-up-mail-notifications>`_. If you want to use Postfix, see `Using Postfix`_.

* **Reply-to/From**
	- Specify the sender of the email. This will appear in the 'From' field of mail header.

Option 2: SMTP Server (Recommended)
''''''''''''''''''''''''''''''''''''

* **SMTP Server**
	- SMTP mail server address that you are going to use to send email.

* **SMTP Port**
	- SMTP port for mail server. Usually this value is 25 or 587, depending on your SMTP mail server configuration.

* **SMTP TLS/SSL required**
	- Check this box if you want to use TLS/SSL for extra security. The mail server must support TLS/SSL.

* **Username**
	- SMTP user name. Leave empty if no authentication required.

* **Password**
	- SMTP password. Leave empty if no authentication required.

* **Reply-to/From**
	- Specify the sender of the email. This will appear in the 'From' field of mail header.

* **Test Email**
	- Test the mail settings. If successful, an email will be sent to all users in the `Email Notification Settings`_. Do not forget to add a recipient before pressing this button.

Using Postfix
''''''''''''''

Many of Linux distributions come with Sendmail as default MTA. To replace Sendmail and use other MTA, e.g Postfix, you just need to uninstall Sendmail, install Postfix and start the service. Following example shows commands that need to be executed on ClusterControl node as root user for RHEL:

.. code-block:: bash

	$ service sendmail stop 
	$ yum remove sendmail -y 
	$ yum install postfix mailx cronie -y 
	$ chkconfig postfix on 
	$ service postfix start

Email Notification Settings
````````````````````````````

Configures email notifications for alarms generated for your database cluster.

* **Send email to**
	- Recipient email address.

* **Send digests at**
	- Send a digested (summary) email at this time every day for the selected recipient.

* **Time-zone**
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
	Node                   Message related to nodes, e.g node disconnected, missing GRANT, failed to start HAProxy, failed to start NDB cluster nodes.
	Host                   Host related messages, e.g CPU/disk/RAM/swap alarms.
	DbHealth               Database health related messages, e.g memory usage of mysql servers, connections.
	DbPerformance          Alarms for long running transactions and deadlocks
	SoftwareInstallation   Software installation related messages.
	Backup                 Messages about backups.
	Unknown                Other uncategorized messages.
	====================== ===========

* **Select how you want alarms/events to be delivered**
	======= ===========
	Action  Description
	======= ===========
	Ignore  Ignore if an alarm raised.
	Deliver Send notification immediately via email once an alarm raised.
	Digest  Send a summary of alarms raised everyday at *Send digests at*
	======= ===========

Version
````````

Lists out the version of ClusterControl components installed on the node.

Thresholds
++++++++++

Provides thresholds for warnings and critical event. Thresholds specify the threshold level at which an alarm will be triggered and notification will be sent via email to the list of recipients configured in the `Email Notification Settings`_. Set your alarm thresholds for:

* CPU, RAM, disk space and swap utilization
* MySQL server memory utilization
* DataMemory, IndexMemory, tablespace, redo log and buffer utilization (MySQL Cluster)

========= ===========
Level     Description
========= ===========
Warning   Sets your warning threshold in percentage for specific event.
Critical  Sets your critical threshold in percentage for specific event.
========= ===========

Graphs
+++++++

Manages graph settings and data capturing. On each database node, ClusterControl records up to 82 MySQL counters every ``db_stats_collection_interval`` defined in CMON configuration file. The default is 30 seconds. 

You can choose to view up to 20 counters in 2 or 3 columns layout on this page. The settings configured here affects *ClusterControl > Performance > Overview* page.

* **Search**
	- Filters the status variables available in the counter list.

* **VAR**
	- List of the status and variables that ClusterControl tracks. For detailed explanation on status variables of your cluster, you can refer to following pages:
		- MySQL: http://dev.mysql.com/doc/refman/5.6/en/server-status-variables.html
		- Galera Cluster: http://galeracluster.com/documentation-webpages/galerastatusvariables.html
		- MySQL Cluster: http://dev.mysql.com/doc/refman/5.6/en/mysql-cluster-status-variables.html

* **Graph 1-N**
	- Choose the enabled counter to be graphed.
	
Query Monitor
++++++++++++++

Manages how ClusterControl should perform query monitoring. It determines the output of:

* *ClusterControl > Overview > Cluster-Wide Queries*
* *ClusterControl > Query Monitoring > Top Queries*
* *ClusterControl > Query Monitoring > Query Outliers*

Changes happened in this page does not require the CMON service to restart.

* **MySQL Local Query Override**
	- Choose whether you want ClusterControl to override the local MySQL query sampling:
		- Yes - The local MySQL configuration settings inside ``my.cnf`` for ``long_query_time`` and ``log_queries_not_using_indexes`` will be used.
		- No - ClusterControl uses *Long Query Time* and *Log queries not using indexes* will be used across all MySQL Servers.

* **Long Query Time**
	- Collects queries taking longer than *Long Query Time* seconds:
		- 0 - All queries.
		- 0.1 - Only queries taking more than 0.1 seconds will be accounted.
