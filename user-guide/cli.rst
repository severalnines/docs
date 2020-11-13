.. _ClusterControl CLI:

ClusterControl CLI
==================

.. include:: cli/index.rst
.. include:: cli/s9s-account.rst
.. include:: cli/s9s-alarm.rst
.. include:: cli/s9s-backup.rst
.. include:: cli/s9s-cluster.rst
.. include:: cli/s9s-container.rst
.. include:: cli/s9s-controller.rst
.. include:: cli/s9s-job.rst
.. include:: cli/s9s-maintenance.rst
.. include:: cli/s9s-metatype.rst
.. include:: cli/s9s-node.rst
.. include:: cli/s9s-process.rst
.. include:: cli/s9s-replication.rst
.. include:: cli/s9s-report.rst
.. include:: cli/s9s-script.rst
.. include:: cli/s9s-server.rst
.. include:: cli/s9s-tree.rst
.. include:: cli/s9s-user.rst


.. _ClusterControl CLI - Limitations:

Limitations
-----------

Currently the s9s command line tool has a user management module that is not yet fully integrated with ClusterControl UI and ClusterControl Controller. For example, there is no RBAC (Role-Based Access Control) for a user (see Setup and Configuration how to create a user). This means that any user created to be used with the s9s command line tool has a full access to all clusters managed by the ClusterControl server.

Users created by the command line client will be shown in Job Messages, but it is not possible to use this user to authenticate and login from the UI. Thus, the users created from the command line client are all super admins.