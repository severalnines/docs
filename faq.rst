.. _FAQ:

FAQ
===

General
-------

Is ClusterControl free?
+++++++++++++++++++++++

ClusterControl has a Community Edition that is free forever, available features include deployment and monitoring of clusters/servers. Please see the `ClusterControl product page <http://www.severalnines.com/pricing>`_ for details on features. 

After installation, you will get a 30-day free trial license (no credit card or signing required) with all enterprise features enabled. After the trial license expires, it will default back to community edition.

Is there a live demo site where I can try out ClusterControl?
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

There is no live demo site available at the moment, although we have some webinars that show the product in details. We recommend users to `install ClusterControl <http://www.severalnines.com/getting-started>`_ on a test server/VM and give it a try. 

You can also contact Severalnines so we can prepare a demo for you - usually we can do this within 24 to 48 hours after the initial enquiry.

Can I purchase the license for the first year only?
+++++++++++++++++++++++++++++++++++++++++++++++++++

Subscriptions are annual, and include software license, all updates and technical support. At the end of the subscription period, you can choose to renew the subscription or continue with the Community Edition. When the license expires, ClusterControl defaults to the Community Edition. 

My 30-day demo license has expired but I haven't had enough time to test ClusterControl. Can I request for extension?
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Of course. Please use our contact us page or email directly to sales[at]severalnines.com 

I would like to subscribe as paid user but have limited budget. What should I do?
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

We’ll be happy to understand your specific requirements. Please contact us at sales[at]severalnines.com.

Can I resell your product?
++++++++++++++++++++++++++

Yes, we have a reseller program. Please email us at sales[at]severalnines.com

Who uses ClusterControl?
++++++++++++++++++++++++

We have over 100 paying customers, including the likes of Cisco, BT, CNRS, Ping Identity, Technicolor and Eurovision. A list of references and case studies can be found `Severalnines Customers <http://severalnines.com/customers>`_ page.

Technical
---------

Do I need to be a DBA to use ClusterControl?
++++++++++++++++++++++++++++++++++++++++++++

No. We build ClusterControl to be used by DBAs as well as Sysadmins/DevOps to easily manage their databases.

Does ClusterControl run on Windows?
+++++++++++++++++++++++++++++++++++

No. ClusterControl only runs on RHEL-based and Debian-based with x86_64 architecture.

How do I upgrade ClusterControl?
++++++++++++++++++++++++++++++++

Our official upgrade instruction is available at `our documentation page <administration.html#upgrading-clustercontrol>`_ and `our support page <http://support.severalnines.com/hc/en-us/articles/212425903>`_.

I'm using ClusterControl community version. Which support channel should I use?
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You can open a thread in our `public forum <https://support.severalnines.com/forums>`_. In case you have to share files or logs that might contain sensitive information, please submit a `support ticket <https://support.severalnines.com/new>`_.

Can I install ClusterControl on RHEL based system but my database cluster is running on Debian?
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Yes. Starting from 1.2.11, ClusterControl does not depend on the DB nodes' operating system any longer.

Can I use ClusterControl in an internetless environment?
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Yes. However some features might not be working well, as explained in `Offline Installation <installation.html#offline-installation>`_ page.

Can I have a mix of different SSH users per cluster?
++++++++++++++++++++++++++++++++++++++++++++++++++++

It's possible but not recommended since it will make the deployment more complex. If you choose root as the SSH user, you should use root user for other clusters as well.

Can I use non-privileged SSH user for ClusterControl?
+++++++++++++++++++++++++++++++++++++++++++++++++++++

Unfortunately, no. ClusterControl requires super-privileged SSH user to perform tasks like deployment, administration and management of the database nodes.

What is difference of MySQL Galera Cluster, Percona XtraDB Cluster and MariaDB Galera Cluster?
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The Galera technology is developed by `Codership Oy <http://galeracluster.com/>`_ and is available as a patch for `standard MySQL <http://www.mysql.com>`_ and InnoDB. `Percona <https://www.percona.com>`_ and `MariaDB <http://mariadb.org>`_ leverage the Galera library in Percona XtraDB Cluster (PXC) and MariaDB Galera Cluster respectively.
 
Since they all leverage the same Galera library, replication performance should be fairly similar. The Codership build usually has the latest version of Galera, although that could change in the future.

I found a bug. Where should I report it?
++++++++++++++++++++++++++++++++++++++++

Please report the bug by `submitting a support request <http://support.severalnines.com/hc/en-us/requests/new>`_. For Enterprise customers, bug fixes are provided soonest possible.

My cluster is down. What should I do?
+++++++++++++++++++++++++++++++++++++

Please submit a ticket at our `support site <https://support.severalnines.com/new>`_. If you are a paying customer, please do this with your company email address so the ticket can be assigned to the appropriate support SLA. Please also include an `error report <troubleshooting.html#error-reporting>`_.

How can I submit a feature request?
+++++++++++++++++++++++++++++++++++

You can either use our `Feature Request <http://support.severalnines.com/hc/en-us/community/topics/200447603-Feature-Requests>`_ page or submit a ticket at our support site `<https://support.severalnines.com/new>`_.

