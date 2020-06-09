.. _UserGuide:

==========
User Guide
==========

ClusterControl provides two user interfaces to interact with ClusterControl Controller (CMON) service:

1) ClusterControl GUI - Web application
2) ClusterControl CLI - Command line client called "s9s"

Not all functionalities are available on both user interfaces. For instances, there is no practicality for the command line client (ClusterControl CLI) to have all the advanced monitoring features offered by the web application client (ClusterControl UI). The command line client is heavily focused on automation and adoption in management, deployment and scaling operations while for the web UI client, it focuses more on structural visualization with guided approach. Occasionally, new management features will be introduced in the CLI before being incorporated into ClusterControl UI to turn it into a polished, finalized feature.

Starting from ClusterControl 1.4.2, installation using the installer script will include ClusterControl CLI installation as well. You can verify this by running the following commands on ClusterControl server after the installation process completes:

.. code-block:: bash

	$ s9s --version | grep Version
	s9s Version 1.7.6
	$ s9s cluster --ping
	PING Ok  11 ms

For more details on how to install, configure and manage these two clients, see the following guides:

* :ref:`Installation`
* :ref:`Components - ClusterControl UI`
* :ref:`Components - ClusterControl CLI`

.. include:: ui.rst
.. include:: cli.rst