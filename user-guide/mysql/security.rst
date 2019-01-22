.. _MySQL - Security:

Security
---------

.. Note:: This feature is introduced in version 1.6.2.

Consolidates cluster-wide security functionality on an easily accessible single page. In the previous versions, cluster-wide security configuration fell under :ref:`MySQL's Cluster Actions <MySQL - Overview - Actions>` menu. Supported security functionalities are:

- Client/Server SSL encryption for MySQL/MariaDB-based clusters.
- SSL replication traffic encryption for MySQL/MariaDB Galera-based clusters.
- Audit Logging

There are currently under development functionalities for MySQL:

- Transparent Data Encryption, also known as Data-at-Rest Encryption

.. _MySQL - Security - SSL Encryption:

SSL Encryption
+++++++++++++++

Enable
``````

Enables encrypted SSL client-server connections for the database node(s). The transport layer will be encrypted using the Transfer Layer Security (TLS) protocol. The same certificate will be used on all nodes and to enable SSL encryption the nodes must be restarted. Select 'Restart Nodes' to perform a rolling restart of the nodes. All keys and certificates will be generated using OpenSSL.

Create Certificate
'''''''''''''''''''

* **Create Certificate**
	- Create a self-signed certificate immediately and use it to setup SSL encryption.

* **Certificate Expiration (days)**
	- Number of days before the certificate become expired and invalid. Default is 1 year (365 days).
	
Use Existing Certificate
''''''''''''''''''''''''

* **Selected Certificate**
	- Pick an exiting certificate, stored by :ref:`Sidebar - Key Management`.

* **Restart Cluster**
	- Restart Nodes - Automatically perform rolling restart of the nodes after setting up certificate and key.
	- Do Not Restart Nodes - Do nothing after setting up certificate and key. User has to perform the server restart manually.

Change Certificate
``````````````````

Changes the existing certificate for SSL client-server connections for the database node(s). This feature is only available if you already enabled SSL encryption for this cluster. It loads the same options as mentioned in *Create Certificate* and *Use Existing Certificate* respectively.

Disable
````````

Disables SSL encryption for the cluster. This option is only available if you have enabled SSL encryption.

.. _MySQL - Security - Galera SSL Encryption:

Galera SSL Encryption
++++++++++++++++++++++

Enable
``````

Exclusive for Galera Cluster. This feature configures Galera replication to use SSL instead of plain replication between Galera nodes. The SSL key and certificate will be created on the Galera nodes. During this operation the cluster will be stopped and started again.

Create Certificate
''''''''''''''''''

* **Certificate is to be expired in (days)**
    - Number of days before the certificate become expired and invalid. Default is 1 year (365 days).
		
Use Existing Certificate
''''''''''''''''''''''''

* **Selected Certificate**
	- Pick an exiting certificate, stored by :ref:`Sidebar - Key Management`.
	
Change Certificate
``````````````````

Changes the existing certificate for Galera replication SSL connections. This feature is only available if you already enabled Galera SSL encryption for this cluster. It loads the same options as mentioned in *Create Certificate* and *Use Existing Certificate* respectively.

Disable
````````

Disables Galera SSL encryption for the cluster. This option is only available if you have enabled Galera SSL encryption.

.. _MySQL - Security - Audit Log:

Audit Log
+++++++++

Enable
``````

Only for MariaDB-based clusters. Enable Audit Logging on MariaDB-based clusters using policy-based monitoring and logging of connection and query activity. This feature will enable audit logging on all nodes in the cluster. 

* **Log Path**
	- The filename to store the audit log in the log directory ``/var/log/mysql/``. Alternatively, it can contain a path relative to the data directory or an absolute path.

* **Rotation Size**
	- Log file size in MB before log rotation happens. Changing this default value will require a cluster restart.

* **Rotations**
	- Number of log files to keep after rotation.

* **Show Advanced Options**
	- Expands the advanced options.

* **Events**
	- Specify MySQL/MariaDB audit events that you would like to capture. ClusterControl preloads the audit events as you type. Multiple values are allowed.

* **Exclude Users**
	- Exclude the specify MySQL/MariaDB user(s) from the auditing. ClusterControl preloads all database users in a dropdown. Multiple values are allowed.

Disable
````````

Disables Audit Log for the cluster. This option is only available if you have enabled Audit Log.