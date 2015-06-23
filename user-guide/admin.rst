Admin
=====

Admin page provides interface to manage clusters, organizations, users, roles and authentication options inside ClusterControl.

.. image:: img/admin_index.png

Clusters
--------

Manage database clusters inside ClusterControl.

* **Delete** 
	- Unregister selected database cluster from the ClusterControl UI. This action will **NOT** delete the actual database cluster.

* **Change Organization** 
	- Change organization for selected database cluster from the organization list created in *Organizations/Users* tab.

* **Create DB User** 
	- Copy an existing MySQL user (which explicitly created through ClusterControl) to other cluster or create a new MySQL user for the selected cluster. This user will be allowed to connect from the specified host to all mysql servers. 

* **Assign DB User Privileges** 
	- Assign privileges to a database user for the selected cluster.

* **Show DB Users and Privileges** 
	- Show the existing database user and privileges created by ClusterControl for the selected cluster.

.. Note:: You can retrieve the ClusterControl API Token value directly from ``[wwwroot]/cmonapi/config/bootstrap.php`` under ``CMON_TOKEN`` variable or from 'dcps' database.

Cluster Job
------------

List of jobs that have been performed related to the cluster (e.g., deploying a new cluster, adding an existing cluster and cloning). This is different from *ClusterControl > Logs > Jobs* under each database cluster. Pick a job to see its running messages.

* **Delete**
	- Delete or kill the selected cluster job.

* **Restart**
	- Restart the selected cluster job.

* **Copy**
	- Open a new pop-up so the user can select the command's text and copy.

* **Job**
	- JSON formatted job command.

* **Status**
	+------------+--------------------------------------+
	| Job status | Description                          |
	+============+======================================+
	| FINISHED   | The job is successfully executed.    |
	+------------+--------------------------------------+
	| FAILED     | The job is executed but failed.      |
	+------------+--------------------------------------+
	| RUNNING    | The job is started and in progress.  |
	+------------+--------------------------------------+
	| ABORTED    | The job is started but terminated.   |
	+------------+--------------------------------------+
	| DEFINED    | The job is defined but yet to start. |
	+------------+--------------------------------------+

* **Time**
	- Time stamp on the reported status.

Organizations/Users
--------------------

Manage organization and users under ClusterControl. You can have one or more organization and each organization consists of zero or more clusters and users. You can have many roles defined under ClusterControl and a user must be assigned with one role.

As a roundup, here is how the different entities relate to each other:

.. image:: img/cc_erd.png
   :align: center

.. Note:: ClusterControl creates 'Admin' organization by default.

Users
'''''

A user belongs to one organization and assigned with a role. Users created here will be able to login and see specific cluster(s), depending on their organization and the cluster they have been assigned to.

Each role is defined with specific privileges under *Access Control* tab. ClusterControl default roles are Super Admin, Admin and User:

=============== ============
Role            Description
=============== ============
**Super Admin** Able to see all clusters that are registered in the UI. The Super Admin can also create organizations and users. Only the Super Admin can transfer a cluster from one organization to another.
**Admin**       Belongs to a specific organization, and is able to see all clusters registered in that organization.
**User**        Belongs to a specific organization, and is only able to see the cluster(s) that he/she registered.
=============== ============

Access Control
--------------

ClusterControl uses Role-Based Access Control (RBAC) to restrict access to clusters and their respective deployment, management and monitoring features. This ensures that only authorised user requests are allowed. Access to functionality is fine-grained, allowing access to be defined by organisation or user. ClusterControl uses a permissions framework to define how a user may interact with the management and monitoring functionality, after they have been authorised to do so. 

You can create a custom role with its own set of access levels. Assign the role to specific user under *Organizations/Users* tab.

.. Note:: The **Super Admin** role is not listed since it is a default role and has the highest level of privileges in ClusterControl. 

Privileges
''''''''''

========= ===========
Privilege Description
========= ===========
Allow     Allow access without modification. Similar to read-only mode.
Deny      Deny access. The selected feature will not appear.
Manage    Allow access with modification.
Modify    Similar to manage, for certain features that required modification.
========= ===========

Feature Description
''''''''''''''''''''

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
**Clone**                    Clone Cluster page (Galera) - *ClusterControl > Actions > Clone Cluster*
**Access All Clusters**      Access all clusters registered under the same organzation.
**Cluster Registrations**    Cluster Registrations page - *ClusterControl > Cluster Registrations*
**Service Providers**        Service Providers page - *ClusterControl > Service Providers*
**Search**                   Search button and page - *ClusterControl > Search*
**Create Database Node**     Create Database Node button and page - *ClusterControl > Create Database Node*
**Developer Studio**         Developer Studio page - *ClusterControl > Manage > Developer Studio*
============================ ============

LDAP Settings
--------------

ClusterControl supports :term:`Active Directory`, :term:`FreeIPA` and :term:`LDAP` authentication. This allows users to log into ClusterControl by using their corporate credentials instead of a separate password. LDAP groups can be mapped onto ClusterControl user groups to apply roles to the entire group. It supports up to LDAPv3 protocol based on `RFC2307 <https://www.ietf.org/rfc/rfc2307.txt>`_.

When authenticating, ClusterControl will first bind to the directory tree server ('LDAP Host') using the specified 'Login DN' user and password, then it will check if the username you entered exists in the form of uid, cn or sAMAccountName of the 'User DN'. If it exists, it will then use the username to bind against the LDAP server to check whether it has the configured group as in 'LDAP Group Name' in ClusterControl. If it has, ClusterControl will then map the user to the appropriate ClusterControl role and grant access to the UI.

The following flowchart summarizes the workflow:

.. image:: img/ipaad_flowchart.png
   :align: center

You can map the LDAP group to corresponding ClusterControl role created under *Access Control* tab. This would ensure that ClusterControl authorizes the logged-in user based on the role assigned.

Once the LDAP settings are verified, login into ClusterControl by using the LDAP credentials (uid, cn or sAMAccountName with respective password). User will be authenticated and redirected to ClusterControl dashboard page based on the assigned role. From this point, both ClusterControl and LDAP authentications would work.

Users and Groups
''''''''''''''''

If LDAP authentication is enabled, you need to map ClusterControl roles with their respective LDAP groups. You can configure this by clicking on ‘+’ icon to add a group:

+-----------------+-------------------------------------------------------------------------+------------------------------------+
| Field           | Description                                                             | Example                            |
+=================+=========================================================================+====================================+
| Organization    | The organization that you want the LDAP group to be assigned to.        | Admin                              |
+-----------------+-------------------------------------------------------------------------+------------------------------------+
| LDAP Group Name | The distinguished name of the LDAP group.                               | cn=Database Administrator,ou=group |
+-----------------+-------------------------------------------------------------------------+------------------------------------+
| Role            | User role in ClusterControl. Please refer to Organization/User section. | SuperAdmin                         |
+-----------------+-------------------------------------------------------------------------+------------------------------------+

Settings
'''''''''

* **Enable LDAP Authentication**
	- Choose whether to enable or disable LDAP authentication.

* **LDAP Host**
	- The LDAP server hostname or IP address. To use LDAP over SSL/TLS, specify LDAP URI, ldaps://[hostname/IP address]

* **LDAP Port**
	- Default is 389 and 636 for LDAP over SSL. Make sure to allow connections from ClusterControl host for both TCP and UDP protocol.

* **Base DN**
	- The root LDAP node under which all other nodes exist in the directory structure.

* **Login DN**
	- The distinguished name used to bind to the LDAP server. This is often the administrator or manager user. This user needs to have read access to all LDAP users that require authentication. ClusterControl must do an LDAP search before any user can log in.

* **Password**
	- The password for the binding user specified in 'Login DN'.

* **User DN**
	- The user's distinguished name used to bind to the LDAP server.

* **Group DN**
	- The group's distinguished name used to bind to the LDAP server.
	
.. Attention:: ClusterControl does not support binding against a nested directory group. Ensure each LDAP user that authenticates to ClusterControl has direct relationship to the LDAP group.

FreeIPA
'''''''

ClusterControl is able to bind to a :term:`FreeIPA` server and perform lookups on compatible schema. Once the :term:`DN` for that user is retrieved, it tries to bind using the full DN (in standard tree) with the entered password to verify the LDAP group of that user.

Thus, for FreeIPA, the user’s and group’s DN should use compatible schema, ``cn=compat`` replacing the default ``cn=accounts`` in ClusterControl LDAP Settings except for the 'Login DN', as shown in following screenshot:

.. image:: img/ipaad_set_ipa.png
   :align: center

Active Directory
''''''''''''''''

Please make sure :term:`Active Directory` runs with 'Identity Management for UNIX' enabled. You can enable this under *Server Manager > Roles > Active Directory Domain Services > Add Role Services*. Detailed instructions on how to do this is explained in `this article <http://technet.microsoft.com/en-us/library/cc731178.aspx>`_.

Once enabled, ensure that each group you want to authenticate from ClusterControl has a Group ID, and each user you want to authenticate from ClusterControl has a UID and is assigned with a GID.

.. Attention:: For Active Directory, ensure you configure the exact distinguished name (with proper capitalization) since the LDAP interchange format (LDIF) fields are returned in capital letters.

For example on how to setup OpenLDAP autentication with ClusterControl, please refer to `this blog post <http://www.severalnines.com/blog/how-setup-centralized-authentication-clustercontrol-users-ldap>`_.

For example on integrating ClusterControl with FreeIPA and Windows Active Directory, please refer to `this blog post <http://severalnines.com/blog/integrating-clustercontrol-freeipa-and-windows-active-directory-authentication>`_.