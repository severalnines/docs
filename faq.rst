.. _faq:

FAQ
===

General
-------

Is ClusterControl free?
+++++++++++++++++++++++

It is free forever for community version with limited features. Please refer to ClusterControl product page for details.

Is ClusterControl open-source?
++++++++++++++++++++++++++++++

Nope. It's closed-source.

Does ClusterControl have a demo site for tryout?
++++++++++++++++++++++++++++++++++++++++++++++++

Nope. We recommend our potential users to book a demo so we can prepare the environment that suits their needs. Usually it takes 24 to 48 hours after the initial enquiry.

Can I purchase the license for the first year only?
+++++++++++++++++++++++++++++++++++++++++++++++++++

License is renew annually. It's your call to decide on the subsequent year.

My 14-days demo license has expired but I haven't had enough time to test ClusterControl. Can I request for extension?
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Of course. Please use our contact us page or email directly to sales[at]severalnines.com 

I would like to subscribe as paid user but have limited budget. What should I do?
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

There is always a room for discussion. Please contact us at sales[at]severalnines.com.

Technical
---------

I'm using ClusterControl community version. Which support channel should I use?
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You can submit a ticket at https://support.severalnines.com/new or open a thread in our public forum, https://support.severalnines.com/forums. The former is recommended because ticketing is a safe place to share your credentials and log files which might contain sensitive information.

Can I install ClusterControl on RHEL based system but my database cluster is running on Debian?
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Yes. Starting from 1.2.11, ClusterControl does not depend on the DB nodes' operating system any longer.

Can I use ClusterControl in an internetless environment?
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Yes. However some features might not be working well, as explained in `this page <installation.html#offline-installation>`_.

Can I have a mixed of SSH user per cluster?
+++++++++++++++++++++++++++++++++++++++++++

It's not recommended and will make the deployment more complex. If you choose root as the SSH user, you must have this user active on all nodes.

What is difference of MySQL Galera Cluster, Percona XtraDB Cluster and MariaDB Galera Cluster?
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The Galera technology is developed by `Codership Oy <http://galeracluster.com/>`_ and is available as a patch for `standard MySQL <http://www.mysql.com>`_ and InnoDB. `Percona <https://www.percona.com>`_ and `MariaDB <http://mariadb.org>`_ leverage the Galera library in Percona XtraDB Cluster (PXC) and MariaDB Galera Cluster respectively.
 
Since they all leverage the same Galera library, replication performance should be fairly similar. The Codership build usually has the latest version of Galera, although that could change in the future.

I found a bug. Where should I report it?
++++++++++++++++++++++++++++++++++++++++

Please report the bug by submiting ticket at https://support.severalnines.com/new. For Enterprise user, if the bug is verified, a bugfix will be provided soonest possible (within one working day).

My cluster is down. What should I do?
+++++++++++++++++++++++++++++++++++++

Please submit a ticket at https://support.severalnines.com/new, login with the registered email address so we can route the support request based on the subscription. Please also include an error report as described in this page.

How can I submit a feature request?
+++++++++++++++++++++++++++++++++++

You can either use our `Feature Request <http://support.severalnines.com/forums/20303403-Feature-Requests>`_ page or submit a ticket to https://support.severalnines.com/new .

