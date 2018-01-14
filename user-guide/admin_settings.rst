.. _admin_settings:


Settings
========

Provides interface to manage clusters, repositories and subscriptions inside ClusterControl.

Clusters
--------

Manage database clusters inside ClusterControl.

* **Delete** 
	- Unregister the selected database cluster from the ClusterControl UI. This action will **NOT** delete the actual database cluster.

* **Change Team** 
	- Change organization for selected database cluster from the organization list created in *Organizations/Users* tab.


Repositories
------------

Manages provider's repository for database servers and clusters. You can have three types of repository when deploying database server/cluster using ClusterControl:

1. Use Vendor Repositories
	- Provision software by setting up and using the database vendor's preferred software repository. ClusterControl will always install the latest version of what is provided by database vendor repository.
2. Do Not Setup Vendor Repositories
	- Provision software by using the pre-existing software repository already setup on the nodes. User has to set up the software repository manually on each database node and ClusterControl will use this repository for deployment. This is good if the database nodes are running without internet connections.
3. Use Mirrored Repositories (Create new repository)
	- Create and mirror the current database vendor's repository and then deploy using the local mirrored repository.
	- This allows you to "freeze" the current versions of the software packages used to provision a database cluster for a specific vendor and you can later use that mirrored repository to provision the same set of versions when adding more nodes or deploying other clusters.
	- ClusterControl sets up the mirrored repository under ``{Apache Document root}/cmon-repos/``, which is accessible via HTTP at :samp:`http://{ClusterControl_host}/cmon-repos/`.

Only Local Mirrored Repository will be listed and manageable here. 

* **Remove Repositories**
	- Remove the selected repository.

* **Filter by cluster type**
	- Filter the repository list by cluster type.

For reference purpose, following is an example of yum definition if Local Mirrored Repository is configured on the database nodes:

.. code-block:: bash

	$ cat /etc/yum.repos.d/clustercontrol-percona-5.6-yum-el7.repo
	[percona-5.6-yum-el7]
	name = percona-5.6-yum-el7
	baseurl = http://10.0.0.10/cmon-repos/percona-5.6-yum-el7
	enabled = 1
	gpgcheck = 0
	gpgkey = http://10.0.0.10/cmon-repos/percona-5.6-yum-el7/localrepo-gpg-pubkey.asc

	
Cluster Registrations
---------------------

From a ClusterControl UI instance, this enables the user to register a database cluster managed by ClusterControl. For each cluster, you need to provide a ClusterControl API URL and token. This effectively establishes the communication between the UI and the controller. The ClusterControl UI can connect to multiple CMON Controller servers (via the CMON REST API) and provide a centralized view of all databases. Users need to register the CMONAPI token and URL for each cluster. 

.. Note:: The CMONAPI token is critical and hidden under asterisk values. This token provides authentication access for ClusterControl UI to communicate with the CMON backend services directly. Please keep this token in a safe place.

You can retrieve the CMONAPI token manually at ``{wwwroot}/cmonapi/config/bootstrap.php`` on line containing ``CMON_TOKEN`` value, where ``{wwwroot}`` is location of Apache document root.

Subscriptions
-------------

For users with a valid subscription (Standalone, Pro, Advanced, Enterprise), enter your license information here to unlock additional features based on the subscription. 

This functionality is also accessible per individual cluster under *Settings > Subscription*. Following screenshot shows example on filing up the license information:

.. image:: img/subscription.png

.. Attention:: Make sure you copy the subscription information as they are, with no leading/trailing spaces.

The license key is validated during runtime. Reload your web browser after registering the license.

.. Note:: When the license expires, ClusterControl defaults back to the Community Edition. For features comparison, please refer to `ClusterControl product page <http://www.severalnines.com/pricing>`_.
