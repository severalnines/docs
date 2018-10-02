.. _PostgreSQL - Dashboards:

Dashboards
----------

.. Note:: This feature is introduced in version 1.7.0.

This feature is only applicable for agent-based monitoring using Prometheus. You will be presented with a notification panel to enable agent-based monitoring if it has not been activated.

.. seealso:: To understand how ClusterControl performs monitoring jobs, see :ref:`Components - ClusterControl Controller - Monitoring Operations`.

Enable Agent-Based Monitoring
+++++++++++++++++++++++++++++

Opens a side-menu pop-up to configure a Prometheus server. You can re-use an existing Prometheus server for multiple clusters within one ClusterControl instance. For a new Prometheus deployment, ClusterControl will install Prometheus server on the target host and configure exporters on all monitored hosts according to their role for that particular cluster. If you choose an existing Prometheus server, ClusterControl will connect to the data source and also configure the Prometheus exporters accordingly.

* **Select Host**
	- Pick a host to be configured as the Prometheus server. If you already have a running Prometheus server, the dropdown will list out the existing instance and you can pick it up from the list. Ensure ClusterControl can perform passwordless SSH to the target node.

* **Scrape Interval**
	- How frequently to scrape targets by default. This is a Prometheus global configuration. See `Global Configuration <https://prometheus.io/docs/prometheus/latest/configuration/configuration/>`_.

* **Data Retention(days)**
	- Monitoring data retention period. See `Global Configuration <https://prometheus.io/docs/prometheus/latest/configuration/configuration/>`_.

Monitoring Dashboards
++++++++++++++++++++++

Dashboards are composed of individual monitoring panels arranged on a grid. For PostgreSQL, ClusterControl pre-configures a number of dashboards as shown in the following table:

========================= ============================ ===================
Dashboard                 Cluster Type                 Description
========================= ============================ ===================
System Overview           All Clusters                 Provides panels of host metrics and usage for individual host.
Cross Server Graphs       All Clusters                 Provides selected host metrics for all hosts for comparison.
PostgreSQL Overview       PostgreSQL                   Provides panels of general database metrics and usage for individual database host.
========================= ============================ ===================

* **Dashboard**
	- Dropdown list of pre-configured dashboards.

* **Node**
	- List of database nodes hostname (or IP address) and its role for the particular cluster. Only for non-global dashboards.

* **Range Selection**
	- Opens a pop-up to configure time range for all panels. It provides common quick ranges, refresh rate and also date-time picker at the bottom of the window.

* **Zoom Out**
	- Decreases the magnification of the graphs.

* **Refresh Rate**
	- Opens a pop-up to configure time range for all panels. It provides common quick ranges, refresh rate and also date-time picker at the bottom of the window.

* **Configuration**
	- Opens a pop-up to show exporters grouped by host and Prometheus global configuration settings.

* **Re-enable Agent Based Monitoring**
	- Reconfigures agent-based monitoring approach. You can use this function to configure this cluster to use a different Prometheus server.

* **Disable Agent Based Monitoring**
	- Disable this monitoring operation mode and back to the agentless approach. You have the options whether to keep the Prometheus running and continue to pull stats or stop the exporters and Prometheus altogether. Regardless of the chosen option, the Dashboards will also be disabled once the job completes.

For more info on how ClusterControl performs monitoring jobs, see :ref:`Components - ClusterControl Controller - Monitoring Operations`. To learn more about Prometheus monitoring system and exporters, please refer to `Prometheus Documentation <https://prometheus.io/docs/introduction/overview/>`_.