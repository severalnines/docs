Security
---------

.. Note:: This feature is introduced in version 1.6.2.

Consolidates cluster-wide security functionality on an easily accessible single page. In the previous versions, cluster-wide security configuration fell under `Cluster Actions <overview.html#actions>`_ menu. Supported security functionalities are:

- Client/Server SSL encryption for PostgreSQL-based clusters.

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
	- Pick an exiting certificate, stored by `Key Management <../../user-guide/index.html#key-management>`_.

* **Restart Cluster**
	- Restart Nodes - Automatically perform rolling restart of the nodes after setting up certificate and key.
	- Do Not Restart Nodes - Do nothing after setting up certificate and key. User has to perform the server restart manually.

Change Certificate
``````````````````

Changes the existing certificate for SSL client-server connections for the database node(s). This feature is only available if you already enabled SSL encryption for this cluster. It loads the same options as mentioned in *Create Certificate* and *Use Existing Certificate* respectively.

Disable
````````

Disables SSL encryption for the cluster. This option is only available if you have enabled SSL encryption.
