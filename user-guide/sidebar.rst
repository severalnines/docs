.. _Sidebar:

Sidebar
=======

Left-side navigation menu provides shortcuts to manage clusters, roles, users, notifications, integration, reporting, authentication, keys and certificates. The menu can be collapsed	to icon-only menu and expanded by using the `Close Menu`_ function.

.. _Sidebar - Clusters:

Clusters
--------

List of database clusters managed by ClusterControl with summarized status. Database cluster deployed by (or added into) ClusterControl will be listed in this page. See `Database Cluster List <dashboard.html#database-cluster-list>`_ section.

.. _Sidebar - Operational Reports:
	
Operational Reports
-------------------

Generates or schedules operational reports. Operational report is a comprehensive way to understand the state of your database cluster along the time. The reports can be generated in-place or can be delivered to you via email, which make things conveniently easy if you have a reporting silo.

There are 5 types of reports available:

* Availability report - All clusters.
* Backup report - All clusters.
* Schema change report - MySQL/MariaDB-based cluster only.
* Daily system report - Per cluster.
* Package upgrade report - Per cluster.

See `Types of Operational Reports`_ for more details. Check out the operational report examples in this blog post, `Become a ClusterControl DBA: Operational Reports for MySQL, MariaDB, PostgreSQL & MongoDB <https://severalnines.com/blog/become-clustercontrol-dba-operational-reports-mysql-mariadb-postgresql-mongodb>`_.

Generated Reports
+++++++++++++++++

Provides list of generated operational reports. Click on any of the entries will open the generated operational report in a new window.

* **Create**
	- Create an operational report. See `Types of Operational Reports`_.
	- Specify the cluster name and operational type. Optionally, you can click on 'Add Email' button to add recipients into the list. 

* **Delete**
	- Delete the selected operational report.

* **Refresh**
	- Refresh the operational report list.

Schedules
++++++++++

List of scheduled operational report.

* **Schedule**
	- Schedules an operational report at an interval. You can schedule it daily, weekly and monthly. Optionally, you can click on 'Add Email' button to add recipients into the list.

* **Edit**
	- Edit the selected schedule.

* **Delete**
	- Delete the selected schedule.

* **Refresh**
	- Refresh the schedule list.

Types of Operational Reports
++++++++++++++++++++++++++++

Availability Report
```````````````````

*All Clusters*

The report generates the uptime/downtime and availability report for each cluster managed by ClusterControl. You can see information about availability statistics of your databases, the cluster type, total uptime and downtime, current state of the cluster and when that state last changed.

Backup Report
`````````````

*All Clusters*

The report summarizes backups for each cluster managed by ClusterControl. It contains two sections - backup summary and backup details, where the former basically gives you a short summary of when the last backup was created, if it completed successfully or failed, backup verification status, success rate and retention period. The latter section provides the list of backups executed on the cluster with the state, type and size within the specified interval.

ClusterControl also provides examples of backup policy if it finds any of the monitored database cluster running without any scheduled backup or delayed slave configured.

Schema Change Report
````````````````````

*MySQL/MariaDB-based cluster only*

This report compares the selected MySQL/MariaDB database changes in table structure which happened between two different generated reports. In the MySQL/MariaDB older versions, DDL operation is a non-atomic operation (pre 8.0) and requires full table copy (pre 5.6 for most operations) - blocking other transactions until it completes. Schema changes could become a huge pain once your tables get a significant amount of data and must be carefully planned especially in a clustered setup. In a multi-tiered development environment, we have seen many cases where developers silently modify the table structure, resulting in significant impact to query performance.

In order for ClusterControl to produce an accurate report, special options must be configured inside CMON configuration file for the respective cluster:

* ``schema_change_detection_address`` - Checks will be executed using SHOW TABLES/SHOW CREATE TABLE to determine if the schema has changed. The checks are executed on the address specified and is of the format HOSTNAME:PORT. The ``schema_change_detection_databases`` must also be set. A differential of a changed table is created (using diff).
* ``schema_change_detection_databases`` - Comma separated list of databases to monitor for schema changes. If empty, no checks are made.

.. Note:: Only ``CREATE TABLE`` and ``ALTER TABLE`` are detected. ``DROP TABLE`` is not supported yet.

Supposed we have a MariaDB Galera Cluster with cluster ID 27 and we would like to monitor schema changes for database "myapp" and "sbtest". Pick one of the database nodes as the value of ``schema_change_detection_address``. For MySQL replication, this should be the master host, or any slave host that holds the databases (in case partial replication is active). Then, inside ``/etc/cmon.d/cmon_27.cnf``, add the two following lines:

.. code-block:: bash

	schema_change_detection_address=10.0.0.30:3306
	schema_change_detection_databases=myapp,sbtest

Restart CMON service to load the change:

.. code-block:: bash

	$ systemctl restart cmon

Take note only new tables or changed tables are printed in the report. The first report is only for metadata collection for comparison in the subsequent rounds, thus we have to run it for at least twice to see the difference. For the first and foremost report, ClusterControl only returns the result of metadata collection. With the first report as the baseline, the subsequent reports will return the output that we are expecting for.


Daily System Report
```````````````````

*Per Cluster*

The current default report shows a cluster's health and performance at the time it was generated compared to one day ago. Under the summary section, it lists out the nodes in the cluster, their type, role (master or slave), status of the node, uptime and the OS. It also reports the top queries summary as well as node status overview in histogram format like CPU usage, data throughput, load average, disk usage and throughput, RAM usage, network throughput, server load and handler.

Package Upgrade Report
``````````````````````

*Per Cluster*

This report gives a summary of packages available for upgrade by the repository manager on the monitored hosts. It summarizes the total number of packages available for upgrade as well as the related managed service for the cluster like load balancer, virtual IP address and arbitrator. This report can greatly help us plan our maintenance window efficiently. For critical upgrades like security and database packages, we could prioritize it over non-critical upgrades, which could be consolidated with other less priority maintenance windows.

For an accurate reporting, ensure you always use stable and trusted repositories on every host. In some undesirable occasions, the monitored hosts could be configured with an outdated repository after an upgrade (e.g, every MariaDB major version uses different repository), incomplete internal repository (e.g, partial mirrored from the upstream) or bleeding edge repository (commonly for unstable nightly-build packages).

.. _Sidebar - Email Notifications:

Email Notifications
-------------------

Configures global email notifications across clusters.

* **Add Recipient**
	- Creates a new recipient by specifying an email address. A newly created recipient will be listed under 'External' organization.
	
* **Delete Recipient**
	- Removes an existing recipient. 

* **Save**
	- Saves the settings to individual cluster.
	
* **Remove**
	- Unassigns the settings for the individual cluster to the selected recipient.

* **Save to all Clusters**
	- Save the settings to all clusters.

* **Send digests at**
	- Send a digested (summary) email at this time every day to the selected recipient.

* **Time-zone**
	- Timezone for the selected recipient.

* **Daily limit for non-digest email as**
	- The maximum number of non-digest email notification should be sent per day for the selected recipient. -1 for unlimited.

* **Alarm/Event Category**
	====================== ===========
	Event                  Description
	====================== ===========
	All Event Categories   All events.
	Network                Network related messages, e.g. host unreachable, SSH issues.
	CmonDatabase           Internal CMON database related messages.
	Mail                   Mail system related messages.
	Cluster                Cluster related messages, e.g. cluster failed.
	ClusterConfiguration   Cluster configuration messages, e.g. software configuration messages.
	ClusterRecovery        Recovery messages like cluster or node recovery failures.
	Node                   Message related to nodes, e.g. node disconnected, missing GRANT, failed to start HAProxy, failed to start NDB cluster nodes.
	Host                   Host related messages, e.g. CPU/disk/RAM/swap alarms.
	DbHealth               Database health related messages, e.g. memory usage of mysql servers, connections.
	DbPerformance          Alarms for long running transactions and deadlocks
	SoftwareInstallation   Software installation related messages.
	Backup                 Messages about backups.
	Unknown                Other uncategorized messages.
	====================== ===========

* **Select how you want alarms/events delivered**
	======= ===========
	Action  Description
	======= ===========
	Ignore  Ignore if an alarm raised.
	Deliver Send notification immediately via email once an alarm raised.
	Digest  Send a summary of alarms raised everyday at *Send digests at*
	======= ===========

.. _Sidebar - Integrations:

Integrations
-------------

Manages ClusterControl integration modules. Starting from version 1.5.0, there are two modules available:

- 3rd Party Notifications via *clustercontrol-notifications* package.
- Cloud Provider integration via *clustercontrol-cloud* and *clustercontrol-clud* packages.

.. _Sidebar - Integrations - 3rd Party Notifications:

3rd Party Notifications
+++++++++++++++++++++++++

Configures third-party notifications on events triggered by ClusterControl.

.. seealso:: `Introducing the ClusterControl Alerting Integrations <https://severalnines.com/blog/introducing-clustercontrol-alerting-integrations>`_.

Supported services are:

+-------------------------------+-----------------+----------+
|  Incident management services | Chat services   | Others   |
+===============================+=================+==========+
| PagerDuty                     | Slack           | Webhook  |
+-------------------------------+-----------------+          |
| VictorOps                     | Telegram        |          |
+-------------------------------+                 |          |
| OpsGenie                      |                 |          |
+-------------------------------+                 |          |
| ServiceNow                    |                 |          |
+-------------------------------+-----------------+----------+
	
* **Add new integration**
	* Opens the service integration configuration wizard.

* **Select Service**
	* Pick a service that you want to configure. Different service requires different set of options.
	
* **Service Configuration**
	* Specify a name for this integration together with the corresponding service key. The service key can be retrieved from the provider website. Click on the "Test" button to verify if ClusterControl is able to connect with the service provider.

* **Notification Configuration**
	* Specify the cluster name together with ClusterControl events that you would like to trigger for incident. You can define multiple values for both fields. Details on the events is described in the following table:

	====================== ===========
	Event                  Description
	====================== ===========
	All Events             All ClusterControl events including warning and critical events.
	All Warning Events     All ClusterControl warning events, e.g. cluster degradation, network glitch. See `Warning Events`_.
	All Critical Events    All ClusterControl critical events, e.g. cluster failed, host failed. See `Critical Events`_.
	Network                Network related events, e.g. host unreachable, SSH issues.
	CMON Database          Internal CMON database related events, e.g. unable to connect to CMON database, datadir mounted as read-only.
	Mail                   Mail system related events, e.g. unable to send mail, mail server unreachable.
	Cluster                Cluster related events, e.g. cluster failed, cluster degradation, time drifting.
	Cluster Configuration  Cluster configuration events, e.g. SST account mismatch.
	Cluster Recovery       Recovery events, e.g. cluster or node recovery failures.
	Node                   Node related events, e.g. node disconnected, missing GRANT, failed to start HAProxy, failed to start NDB cluster nodes.
	Host                   Host related messages, e.g. CPU/disk/RAM/swap exceeds thresholds, memory full.
	Database Health        Database health related events, e.g. memory usage of mysql servers, connections, missing primary key.
	Database Performance   Alarms for long running transactions, replication lag and deadlocks.
	Software Installation  Software installation related events, e.g. license expiration.
	Backup                 Backups related events, e.g. backup failed.
	====================== ===========

* **Edit**
	- Edit the selected integration.

* **Delete**
	- Remove the selected integration.
	
Warning Events
``````````````

+---------------+-------------------------+------------+---------------------------------------------------------------------------------+
| Area          | Alarms                  | Severity   | Description                                                                     |
+===============+=========================+============+=================================================================================+
| Node          | MySqlReplicationLag     | Warning    | MySQL replication slave lag, default 10 seconds.                                |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | MySqlReplicationBroken  | Warning    | The SQL thread has stopped.                                                     |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | CertificateExpiration   | Warning    | SSL certificate expiration time (<=31 days, >7 days).                           |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | MySqlAdvisor            | Warning    | Raised by ``wsrep_sst_method.js`` and ``wsrep_node_name.js`` advisors.          |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | MySqlTableAnalyzer      | Warning    | Raised by ``schema_check_nopk.js`` advisor.                                     |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | StorageMyIsam           | Warning    | Raised by ``schema_check_myisam.js`` advisor.                                   |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | MySqlIndexAnalyzer      | Warning    | Raised by ``schema_check_dupl_index.js`` advisor.                               |
+---------------+-------------------------+------------+---------------------------------------------------------------------------------+
| Host          | HostSwapV2              | Warning    | If a configurable number of pages has been swapped in/out during a configurable |
|               |                         |            | period of time. Default 20 pages in 10 minutes.                                 |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | HostSwapping            | Warning    | >5% swap space has been used.                                                   |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | HostCpuUsage            | Warning    | >80%, <90% CPU used.                                                            |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | HostRamUsage            | Warning    | >80%, <90% RAM used.                                                            |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | HostDiskUsage           | Warning    | >80%, <90% disk space used on a monitored_mountpoint.                           |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | ProcessCpuUsage         | Warning    | >95 % CPU used in average by a process for 15 minutes.                          |
+---------------+-------------------------+------------+---------------------------------------------------------------------------------+
| Backup        | BackupFailed            | Warning    | Backup job fails.                                                               |
+---------------+-------------------------+------------+---------------------------------------------------------------------------------+
| Recovery      | GaleraWsrepMissing      | Warning    | ``wsrep_cluster_address`` or ``wsrep_provider`` is missing.                     |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | GaleraSstAuth           | Warning    | SST settings (user/pass are wrong).                                             |
+---------------+-------------------------+------------+---------------------------------------------------------------------------------+
| Network       | HostFirewall            | Warning    | Host is not responding to ping after 3 cycles.                                  |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | HostSshSlow             | Warning    | It takes 6-12 seconds to SSH into a host.                                       |
+---------------+-------------------------+------------+---------------------------------------------------------------------------------+
| Cluster       | ClusterTimeDrift        | Warning    | Time drift between ClusterControl and database nodes.                           |
+               +-------------------------+------------+---------------------------------------------------------------------------------+
|               | ClusterLicenseExpire    | Warning    | License is about to expire.                                                     |
+---------------+-------------------------+------------+---------------------------------------------------------------------------------+

Critical Events
````````````````

+---------------+--------------------------+------------+--------------------------------------------------------------------------------------------+
| Area          | Alarms                   | Severity   | Description                                                                                |
+===============+==========================+============+============================================================================================+
| Node          | MySqlDisconnected        | Critical   | Node has disconnected.                                                                     |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | MySqlGrantMissing        | Critical   | Node does not have the correct privileges set for the cmon user.                           |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | MySqlLongRunningQuery    | Critical   | If queries are running for too long time. Only used  if configured, by default it is not.  |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | ProcFailedRestart        | Critical   | A process (HAProxy, ProxySQL, Garbd, MaxScale) could not be restarted after failure.       |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | CertificateExpiration    | Critical   | (<= 7 days), SSL Certificates expiration time.                                             |
+---------------+--------------------------+------------+--------------------------------------------------------------------------------------------+
| Host          | HostSwapV2               | Critical   | If a configurable number of pages has been swapped in/out during a configurable            |
|               |                          |            | period of time. Default 20 pages in 10 minutes.                                            |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | HostSwapping             | Critical   | >20% swap space has been used.                                                             |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | HostCpuUsage             | Critical   | >90% CPU used.                                                                             |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | HostRamUsage             | Critical   | >90% RAM used.                                                                             |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | HostDiskUsage            | Critical   | >90% disk space used on a monitored_mountpoint.                                            |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | ProcessCpuUsage          | Critical   | >99 % CPU used in average by a process for 15 minutes.                                     |
+---------------+--------------------------+------------+--------------------------------------------------------------------------------------------+
| Backup        | BackupVerificationFailed | Critical   | Backup verification fails.                                                                 |
+---------------+--------------------------+------------+--------------------------------------------------------------------------------------------+
| Recovery      | GaleraWsrepMissing       | Critical   | ``wsrep_cluster_address`` or ``wsrep_provider`` is missing, and still missing              |
|               |                          |            | after 20 sample cycles which is ~ 100 seconds in this case)                                |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | GaleraClusterSplit       | Critical   | There is a split brain.                                                                    |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | ClusterRecoveryFail      | Critical   | Recovery has failed.                                                                       |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | GaleraConfigProblem1     | Critical   | A configuration issue preventing the node to start.                                        |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | GaleraNodeRecoveryFail   | Critical   | Automatic recovery has failed 3 consecutive times.                                         |
+---------------+--------------------------+------------+--------------------------------------------------------------------------------------------+
| Network       | HostUnreachable          | Critical   | Host is not responding to ping after 3 cycles.                                             |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | HostSshFailed            | Critical   | Please check SSH access to host. The host may also be down.                                |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | HostSshAuth              | Critical   | Please check whether the configured SSH key is authenticated on the host.                  |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | HostSudoError            | Critical   | ``sudo`` command error on host.                                                            |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | HostSshSlow              | Critical   | It takes >12 seconds to SSH into a host.                                                   |
+---------------+--------------------------+------------+--------------------------------------------------------------------------------------------+
| Cluster       | ClusterFailure           | Critical   | Cluster is failure.                                                                        |
+               +--------------------------+------------+--------------------------------------------------------------------------------------------+
|               | ClusterLicenseExpire     | Critical   | License is expired.                                                                        |
+---------------+--------------------------+------------+--------------------------------------------------------------------------------------------+

.. _Sidebar - Integrations - Cloud Providers:

Cloud Providers
+++++++++++++++++

Manages resources and credentials for cloud providers. Note that this new feature requires two modules called *clustercontrol-cloud* and *clustercontrol-clud*. The former is a helper daemon which extends CMON capability of cloud communication, while the latter is a file manager client to upload and download files on cloud instances. Both packages are dependencies of the *clustercontrol* UI package, which will be installed automatically if do not exist. 

.. seealso:: :ref:`ClusterControl Components <Components>`.

The credentials that have been set up here can be used to:

- Manage cloud resources (instances, virtual network, subnet)
- Deploy databases in the cloud
- Upload backup to cloud storage

To create a cloud profile, click on *Add Cloud Credentials* and follow the wizard accordingly. Supported cloud providers are:

- Amazon Web Service
- Google Cloud Platform
- Microsoft Azure

Amazon Web Services Credential
``````````````````````````````

The stored AWS credential will be used by ClusterControl to list out Amazon EC2 instances, spin new instances when deploying a cluster and uploading/downloading backups to AWS S3. 

To create an access key for your AWS account root user:

1. Use your AWS account email address and password to sign in to the AWS Management Console as the AWS account root user.
2. On the IAM Dashboard page, choose your account name in the navigation bar, and then choose "My Security Credentials".
3. If you see a warning about accessing the security credentials for your AWS account, choose "Continue to Security Credentials".
4. Expand the Access keys (access key ID and secret access key) section.
5. Choose "Create New Access Key". Then choose "Download Key File" to save the access key ID and secret access key to a file on your computer. After you close the dialog box, you can't retrieve this secret access key again.

.. seealso:: `Managing Access Keys for Your AWS Account <http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html>`_.

================== ============
Field              Description
================== ============
Name               Credential name.
AWS Key ID         Your AWS Access Key ID as described on this page. You can get this from AWS IAM Management console.
AWS Key Secret     Your AWS Secret Access Key as described on this page. You can get this from AWS IAM Management console.
Default Region     Choose the default AWS region for this credential.
Comment (Optional) Description of the credential. 
================== ============

AWS Instances
'''''''''''''

Lists out your AWS instances. You can perform simple AWS instance management tasks directly from ClusterControl, which uses your defined AWS credentials to connect to the AWS API.

================= ===========
Field             Description
================= ===========
AWS Credentials   Choose which credential to use to access your AWS resources.
Stop              Shutdown the instance.
Reboot            Restart the instance.
Terminate         Shutdown and terminate the instance.
================= ===========

AWS VPC
'''''''

This allows you to conveniently manage your VPC from ClusterControl, which uses your defined AWS credentials to connect to AWS VPC. Most of the functionalities are dynamically populated and integrated to have the same look and feel as the AWS VPC console. Thus, you may refer to `VPC User Guide <https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html>`_ for details on how to manage AWS VPC.

+-------------------+-----------------------------------------------------------------------------------------------------------------+
| Field             | Description                                                                                                     |
+===================+=================================================================================================================+
| Start VPC Wizard  | Open the VPC creation wizard. Please refer to Getting Started Guide for details on how to start creating a VPC. |
+-------------------+-----------------------------------------------------------------------------------------------------------------+
| AWS Credentials   | Choose which credentials to use to access your AWS resources.                                                   |
+-------------------+-----------------------------------------------------------------------------------------------------------------+
| Region            | Choose the AWS region for the VPC.                                                                              |
+-------------------+-----------------------------------------------------------------------------------------------------------------+
| VPC               | List of VPCs created under the selected region.                                                                 |
|                   |                                                                                                                 |
|                   | * Create VPC - Create a new VPC.                                                                                |
|                   | * Delete - Delete selected VPC.                                                                                 |
|                   | * DHCP Options Set - Specify the DHCP options for your VPC.                                                     |
+-------------------+-----------------------------------------------------------------------------------------------------------------+
| Subnet            | List of VPC subnet created under the selected region.                                                           |
|                   |                                                                                                                 |
|                   | * Create - Create a new VPC subnet.                                                                             |
|                   | * Delete - Delete selected subnet.                                                                              |
+-------------------+-----------------------------------------------------------------------------------------------------------------+
| Route Tables      | List of routing tables created under the selected region.                                                       |
+-------------------+-----------------------------------------------------------------------------------------------------------------+
| Internet Gateway  | List of security groups created under the selected region.                                                      |
+-------------------+-----------------------------------------------------------------------------------------------------------------+
| Network ACL       | List of network Access Control Lists created under the selected region.                                         |
+-------------------+-----------------------------------------------------------------------------------------------------------------+
| Security Group    | List of security groups created under the selected region.                                                      |
+-------------------+-----------------------------------------------------------------------------------------------------------------+
| Running Instances | List of all running instances under the selected region.                                                        |
+-------------------+-----------------------------------------------------------------------------------------------------------------+

Google Cloud Platform Credentials
``````````````````````````````````

To create a service account:

1. Open the "Service Accounts" page in the Cloud Platform Console.
2. Select your project and click "Continue"".
3. In the left navigation, click "Service accounts".
4. Look for the service account for which you wish to create a key, click on the vertical ellipses button in that row, and click "Create key".
5. Select JSON as the "Key type" and click "Create".

================== ============
Field              Description
================== ============
Name               Credential name.
Read from JSON     The service account definition in JSON format.
Comment (Optional) Description of the credential.
================== ============

Microsoft Azure Credentials
````````````````````````````

In order to provide an access to Azure services you need to register an application and grant it access to your Azure resources.

1. Create an application:
	a) Login to *Microsoft Azure portal > Azure Active Directory > App registrations > New application registration*.
	b) Provide a name and URL for the application. Select "Web app / API" for the type of application you want to create. 
	c) After specify the values, click "Create".
2. Get application ID and authentication key:
	a) From App registrations in Azure Active Directory, select your application.
	b) Copy the Application ID. You should pass that value as ``application_id``.
	c) To generate an authentication key, select *Settings > Keys*.
	d) Provide a description of the key, and a duration for the key. When done, select "Save".
	e) After saving the key, the value of the key is displayed. Copy this value because you are not able to retrieve the key later. Pass this value as ``client_secret``.
3. Get tenant ID:
	a) Go to *Microsoft Azure portal > Azure Active Directory > Properties* for your Azure AD tenant.
	b) Copy the "Directory ID". Pass this value as ``tenant_id``.
4. Get subscription ID:
	a) Go to *Microsoft Azure portal > Subscriptions*.
	b) Select your subscription from the list. 
	c) Copy the "Subscription ID". Pass this value as ``subscription_id``.
5. Create a resource group:
	a) Go to *Microsoft Azure portal > Resource groups > Add*.
	b) Fill in the values and click "Create". 
	c) Copy Resource group name and use it as ``resource_group`` in the credentials.
	d) Wait until Resource group is created and click *Go to resource group > Access control (IAM) > Add > Add Role Assignment*.
	e) Select "Contributor" as a Role then put your application's name into search input and select it from the list. 
	f) Click "Save".
7. Create a storage account (for *Upload Backup to Cloud* feature):
	a) Go to *Microsoft Azure portal > Storage accounts > Add*.
	b) Fill the "Name" and select "Blob storage" as Account kind. 
	c) Copy the "Name" value and use it as ``storage_account`` in credentials.
	d) Select "Enabled" for "Secure transfer required", select "Subscription" and "Resource group" (use the same Resource group as in the previous steps). 
	e) Select the storage "Location" and then click "Create". 

Finally, create a new text file on your workstation and copy all the required information retrieved from the previous steps in a JSON format. For example:

.. code-block:: json

	{
	   "application_id":"7f649053-xxxx-xxxx-xxxx-2179c1fa83b8",
	   "client_secret":"jbzW9tj4AyXHDkfO/KoTL9OP5EexpD6jeHROo2S4xxxx",
	   "tenant_id":"ce6b8358-xxxx-xxxx-xxxx-49b8c7a5cbc2",
	   "subscription_id":"6fafe95c-xxxx-xxxx-xxxx-1c33daa1c2c3",
	   "resource_group":"cc",
	   "storage_account":"mybackupazure"
	}

Then, when configuring the Azure credentials, load the above text file under "Read from JSON" field. Take note that `storage_account` value is optional.

The uploaded backup will be available under BLOB CONTAINERS storage. You can verify its existence in the cloud by going to *Microsoft Azure portal > Storage Accounts > [your storage account] > Storage Explorer > BLOB CONTAINERS*.

================== ============
Field              Description
================== ============
Name               Credential name.
Read from JSON     The service account definition in JSON format.
Comment (Optional) Description of the credential.
================== ============

.. _Sidebar - Key Management:

Key Management
--------------

Key Management allows you to manage a set of SSL certificates and keys that can be provisioned on your clusters. This feature allows you to create Certificate Authority (CA) and/or self-signed certificates and keys. Then, it can be easily enabled and disabled for MySQL and PostgreSQL client-server connections using SSL encryption feature.

Manage
++++++

Manage existing keys and certificates generated by ClusterControl.

* **Revoke**
    - Revoke the selected certificate. This will put an end to the validity of the certificate.

* **Generate**
    - Re-generate an invalid or expired certificate. By using this, ClusterControl can generate a new key and certificate by using the same information used when it was generated for the first time.

* **Move**
    - Move the selected certificate to another location. Clicking on this will open another dialog box where you can create/delete a directory under ``/var/lib/cmon/ca``. Use this feature to organize and categorize the generated certificate per directory.


Generate
++++++++

By default, the generated keys and certificates will be created under default repository at ``/var/lib/cmon/ca``. 

* **New Folder**
    - Create a new directory under the default repository.

* **Delete Folder**
    - Delete the selected directory.

* **Refresh**
    - Refresh the list.

Self-signed Certificate Authority and Key
``````````````````````````````````````````

Generate a self-signed Certificate Authority and key. You can use this Certificate Authority (CA) to sign your client and server certificates.

* **Path**
    - Certification repository path. To change the path, click on the file browser left-side menu. Default value is ``/var/lib/cmon/ca``.

* **Certificate Authority and Key Name**
    - Enter a name without extension. For example MyCA, ca-cert

* **Description**
    - Put some description for the certificate authority.

* **Country**
    - Choose a country name from the dropdown menu.

* **State**
    - State or province name.

* **Locality**
    - City name.
    
* **Organization**
    - Organization name.

* **Organization unit**
    - Unit or department name.

* **Common name**
    - Specify server fully-qualified domain name (FQDN) or your name.
    - Common Name value used for the server and client certificates/keys must each differ from the Common Name value used for the CA certificate. Otherwise, the certificate and the key files will not work for the servers compiled using OpenSSL.

* **Email**
    - Email address.

* **Key length (bits)**
    - The key length in bits. 2048 and higher is recommended. The larger the public and private key length, the harder it is to crack.

* **Expiration Date (days)**
    - Certificate expiration in days.

* **Generate**
    - Generate certificate and key.
    
* **Reset**
    - Reset the form.

Client/Server Certificates and Key
````````````````````````````````````

Sign with an existing CA or generate a self-signed certificate. ClusterControl generates certificate and key depending on the type, server or client. The generated server's key and certificate can then be used by `Enable SSL Encryption <mysql/overview.html#enable-ssl-encryption>`_ feature.

* **Certificate Authority**
    - Select an existing CA (by clicking on any existing CA on the left-hand side menu) or leave it empty to generate a self-signed certificate.

* **Type**
    - server - Generate certificate for server usage.
    - client - Generate certificate for client usage.

* **Certificate and Key Name**
    - Enter the certificate and key name. The same name will be used by ClusterControl to generate certificate and key. For example, if you specify the name is "severalnines", ClusterControl will generate ``severalnines.key`` and ``severalnines.crt`` respectively.

* **Description**
    - Put some description for the certificate and key.

* **Country**
    - Choose a country name from the dropdown menu.

* **State**
    - State or province name.

* **Locality**
    - City name.
    
* **Organization**
    - Organization name.

* **Organization unit**
    - Unit or department name.

* **Common name**
    - Specify server fully-qualified domain name (FQDN) or your name.
    - Common Name value used for the server and client certificates/keys must each differ from the Common Name value used for the CA certificate. Otherwise, the certificate and the key files will not work for the servers compiled using OpenSSL.

* **Email**
    - Email address.

* **Key length (bits)**
    - The key length in bits. 2048 and higher is recommended.

* **Expiration Date (days)**
    - Certificate expiration in days.

* **Generate**
    - Generate certificate and key.
    
* **Reset**
    - Reset the form.


Import
++++++

Import keys and certificates into ClusterControl's certificate repository. The imported keys and certificates can then be used to enable SSL encryption for server-client connection, replication or backup at a later stage. Before you perform the import action, bear in mind to:

1. Upload your certificate and key to a directory in the ClusterControl Controller host
2. Uncheck the *Self-signed Certificate* checkbox if the certificate is not self-signed
3. You need to also provide a CA certificate if the certificate is not self-signed
4. Duplicate certificates will not be created

* **Destination Path**
  - Where you want the certificate to be imported to. Click on the file explorer window on the left to change the path.

* **Save As**
  - Certificate name.

* **Certificate File**
  - Physical path to the certificate file. For example: ``/home/user/ssl/file.crt``.

* **Private Key File**
  - Physical path to the key file. For example: ``/home/user/ssl/file.key``.

* **Self-signed Certificate**
  - Uncheck the checkbox if the certificate is not self-signed.

* **Import**
  - Start the import process.

.. _Sidebar - User Management:

User Management
---------------

.. _Sidebar - User Management - Teams: 
  
Teams
+++++

Manage teams (organizations) and users under ClusterControl. Take note that only the first user created with ClusterControl will be able to create the teams. You can have one or more teams and each team consists of zero or more clusters and users. You can have many roles defined under ClusterControl and a user must be assigned with one role.

As a roundup, here is how the different entities relate to each other:

.. image:: img/cc_erd.png
   :align: center

.. Note:: ClusterControl creates 'Admin' team by default.

.. _Sidebar - User Management - Users: 

Users
+++++

A user belongs to one team and assigned with a role. Users created here will be able to login and see specific cluster(s), depending on their team and the cluster they have been assigned to.

Each role is defined with specific privileges under *Access Control*. ClusterControl default roles are Super Admin, Admin and User:

=============== ============
Role            Description
=============== ============
**Super Admin** Able to see all clusters that are registered in the UI. The Super Admin can also create organizations and users. Only the Super Admin can transfer a cluster from one organization to another.
**Admin**       Belongs to a specific organization, and is able to see all clusters registered in that organization.
**User**        Belongs to a specific organization, and is only able to see the cluster(s) that he/she registered.
=============== ============

To create a custom role, see `Access Control`_.

.. _Sidebar - User Management - Access Control: 

Access Control
++++++++++++++

ClusterControl uses Role-Based Access Control (RBAC) to restrict access to clusters and their respective deployment, management and monitoring features. This ensures that only authorized user requests are allowed. Access to functionality is fine-grained, allowing access to be defined by organization or user. ClusterControl uses a permissions framework to define how a user may interact with the management and monitoring functionality, after they have been authorized to do so. 

You can create a custom role with its own set of access levels. Assign the role to specific user under *Teams* tab.

.. Note:: The **Super Admin** role is not listed since it is a default role and has the highest level of privileges in ClusterControl. 

Privileges
``````````

========= ===========
Privilege Description
========= ===========
Allow     Allow access without modification. Similar to read-only mode.
Deny      Deny access. The selected feature will not appear in the UI.
Manage    Allow access with modification.
Modify    Similar to manage, for certain features that required modification.
========= ===========

Features Description
````````````````````

============================ ============
Feature                      Description
============================ ============
**Overview**                 Overview tab - *ClusterControl > Overview*
**Nodes**                    Nodes tab - *ClusterControl > Nodes*
**Configuration Management** Configuration management page - *ClusterControl > Manage > Configurations*
**Query Monitor**            Query Monitor tab - *ClusterControl > Query Monitor*
**Performance**              Performance tab - *ClusterControl > Performance*
**Backup**                   Backup tab - *ClusterControl > Backup*
**Manage**                   Manage tab - *ClusterControl > Manage*
**Alarms**                   Alarms tab - *ClusterControl > Alarms*
**Jobs**                     Jobs tab - *ClusterControl > Jobs*
**Settings**                 Settings tab - *ClusterControl > Settings*
**Add Existing Cluster**     Add Existing Cluster button and page - *ClusterControl > Add Existing Server/Cluster*
**Create Cluster**           Create Database Cluster button and page - *ClusterControl > Create Database Cluster*
**Add Load Balancer**        Add Load Balancer page - *ClusterControl > Actions > Add Load Balancer* and *ClusterControl > Manage > Load Balancer*
**Clone**                    Clone Cluster page (Galera only) - *ClusterControl > Actions > Clone Cluster*
**Access All Clusters**      Access all clusters registered under the same organization.
**Cluster Registrations**    Cluster Registrations page - *ClusterControl > Settings (top-menu) > Cluster Registrations*
**Cloud Providers**          Cloud Providers page - *ClusterControl > Settings (top-menu) > Integrations -> Cloud Providers*
**Search**                   Search button and page - *ClusterControl > Search*
**Create Database Node**     Create Database Node button and page - *ClusterControl > Create Database Node*
**Developer Studio**         Developer Studio page - *ClusterControl > Manage > Developer Studio*
**MySQL User Management**    MySQL user management sections - *ClusterControl > Settings (top-menu) > MySQL User Management* and *ClusterControl > Manage > Schema and Users*
**Operational Reports**      Operational reports page - *ClusterControl > Settings (top-menu) > Operational Reports*
**Integrations**             Integrations page - *ClusterControl > Settings (top-menu) > Integrations*
**Web SSH**                  Web-based SSH on every managed node - *ClusterControl > Nodes > Node Actions > SSH Console*
**Custom Advisor**           Custom Advisors page - *ClusterControl > Manage > Custom Advisors*
**SSL Key Management**       Key Management page - *ClusterControl > Settings (top-menu) > Key Management*
============================ ============

.. _Sidebar - User Management - LDAP Settings: 

LDAP Settings
+++++++++++++

ClusterControl supports :term:`Active Directory`, :term:`FreeIPA` and :term:`LDAP` authentication. This allows users to log into ClusterControl by using their corporate credentials instead of a separate password. LDAP groups can be mapped onto ClusterControl user groups to apply roles to the entire group. It supports up to LDAPv3 protocol based on `RFC2307 <https://www.ietf.org/rfc/rfc2307.txt>`_.

When authenticating, ClusterControl will first bind to the directory tree server ('LDAP Host') using the specified 'Login DN' user and password, then it will check if the username you entered exists in the form of uid, cn or sAMAccountName of the 'User DN'. If it exists, it will then use the username to bind against the LDAP server to check whether it has the configured group as in 'LDAP Group Name' in ClusterControl. If it does, ClusterControl will then map the user to the appropriate ClusterControl role and grant access to the UI.

The following flowchart summarizes the workflow:

.. image:: img/ipaad_flowchart.png
   :align: center

You can map the LDAP group to corresponding ClusterControl role created under *Access Control* tab. This would ensure that ClusterControl authorizes the logged-in user based on the role assigned.

Once the LDAP settings are verified, login into ClusterControl by using the LDAP credentials (uid, cn or sAMAccountName with respective password). User will be authenticated and redirected to ClusterControl dashboard page based on the assigned role. From this point, both ClusterControl and LDAP authentications would work.

.. Attention:: For Active Directory, ensure you configure the exact distinguished name (with proper capitalization) since the LDAP interchange format (LDIF) fields are returned in capital letters.

For example on how to setup OpenLDAP authentication with ClusterControl, please refer to this blog post, `How to Setup Centralized Authentication of ClusterControl Users with LDAP <http://www.severalnines.com/blog/how-setup-centralized-authentication-clustercontrol-users-ldap>`_.

LDAP Group
``````````

If LDAP authentication is enabled, you would need to map ClusterControl roles with their respective LDAP groups. You can configure this by clicking on the '+' icon to add an LDAP group:

+-----------------+-------------------------------------------------------------------------+------------------------------------+
| Field           | Description                                                             | Example                            |
+=================+=========================================================================+====================================+
| Team            | The organization that you want the LDAP group to be assigned to.        | Admin                              |
+-----------------+-------------------------------------------------------------------------+------------------------------------+
| LDAP Group Name | The distinguished name of the LDAP group, relative to the *Group DN*    | cn=Database Administrator,ou=group |
+-----------------+-------------------------------------------------------------------------+------------------------------------+
| Role            | User role in ClusterControl. See `Teams`_.                              | Super Admin                        |
+-----------------+-------------------------------------------------------------------------+------------------------------------+

Settings
``````````

* **Enable LDAP Authentication**
	- Choose whether to enable or disable LDAP authentication.

* **LDAP Host**
	- The LDAP server hostname or IP address. To use LDAP over SSL/TLS, specify LDAP URI instead, for example :samp:`ldaps://{LDAP_host}`.

* **LDAP Port**
	- Default is 389 and 636 for LDAP over SSL. Make sure to allow connections from ClusterControl host for both TCP and UDP protocol.

* **Base DN**
	- The root LDAP node under which all other nodes exist in the directory structure.

* **Login DN**
	- The distinguished name used to bind to the LDAP server. This is often the administrator or manager user. It can also be a dedicated login with minimal access that should be able to return the DN of the authenticating users. ClusterControl must do an LDAP search using this DN before any user can log in. This field is case-sensitive.

* **Password**
	- The password for the binding user specified in *Login DN*.

* **User DN**
	- The user's relative distinguished name (RDN) used to bind to the LDAP server. For example, if the LDAP user DN is CN=userA,OU=People,DC=ldap,DC=domain,DC=com, specify :samp:`OU=People,DC=ldap,DC=domain,DC=com`. This field is case-sensitive.

* **Group DN**
	- The group's relative distinguished name (RDN) used to bind to the LDAP server. For example, if the LDAP group DN is  CN=DBA,OU=Group,DC=ldap,DC=domain,DC=com, specify :samp:`OU=Group,DC=ldap,DC=domain,DC=com`. This field is case-sensitive.
	
.. Attention:: ClusterControl does not support binding against a nested directory group. Ensure each LDAP user that authenticates to ClusterControl has a direct membership to the LDAP group.

FreeIPA
````````

ClusterControl is able to bind to a :term:`FreeIPA` server and perform lookups on compatible schema. Once the :term:`DN` for that user is retrieved, it tries to bind using the full DN (in standard tree) with the entered password to verify the LDAP group of that user.

Thus, for FreeIPA, the user’s and group’s DN should use compatible schema, ``cn=compat`` replacing the default ``cn=accounts`` in ClusterControl LDAP Settings except for the 'Login DN', as shown in following screenshot:

.. image:: img/ipaad_set_ipa.png
   :align: center

For example on integrating ClusterControl with FreeIPA and Windows Active Directory, please refer to this blog post, `Integrating ClusterControl with FreeIPA and Windows Active Directory for Authentication <http://severalnines.com/blog/integrating-clustercontrol-freeipa-and-windows-active-directory-authentication>`_.

.. _Sidebar - User Management - Clusters: 

Clusters
++++++++

Manage database clusters inside ClusterControl.

* **Delete** 
	- Unregister the selected database cluster from the ClusterControl UI. This action will **NOT** delete the actual database cluster.

* **Change Team** 
	- Change the selected database cluster to another organization created under `Teams`_.


Documentation
--------------

Opens ClusterControl `online documentation page <http://www.severalnines.com/docs>`_.

Give us Feedback
----------------

Opens a feedback form which you can use to send feedbacks, report bug, submit feature request or ask us questions. The submitted form will be sent directly to our support system and you will receive the response into your email inbox.

What's New?
-----------

Opens the *What's new* popup. This popup also appears the first time a user logs in after new installation or upgrade.

Support Forum
--------------

Opens Severalnines `community support forums <http://support.severalnines.com/forums>`_. Community users are encouraged to use this support channel. For licensed user, please raise a `support ticket <http://support.severalnines.com/tickets/new>`_.

Switch Theme
-------------
	
A switcher for a dark or light colour background of the side menu.

Close Menu
-----------

Collapses and expands the side menu.