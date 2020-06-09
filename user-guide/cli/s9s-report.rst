.. include:: <isotech.txt>

s9s report
----------

Manage operational reports.

**Usage**

.. code-block:: bash

	s9s report {command} {options}


**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ cat                  Prints the report text to the standard output.
|minus|\ |minus|\ create               Creates and print a new report. Please provide the ``--type`` command line option with the report type (that was chosen from the template list printed with the ``--list-templates`` option) and the ``--cluster-id`` with the cluster ID.
|minus|\ |minus|\ delete               Deletes a report that already exists. The report ID should be used to specify which report to delete.
|minus|\ |minus|\ list                 Lists the reports that already created.
|minus|\ |minus|\ list-templates       Lists the report templates showing what kind of reports can be created. This command line option is usually used together with the ``--long`` option.
====================================== ===========

**Options**

============================================= ===========
Name, shorthand                               Description
============================================= ===========
|minus|\ |minus|\ cluster-id=ID               This command line option passes the numerical ID of the cluster.
|minus|\ |minus|\ report-id=ID                This command line option passes the numerical ID of the report to manage. When a report is deleted (``--delete`` option) or printed (``--cat`` option) this option is mandatory.
|minus|\ |minus|\ type=NAME                   This command line option controls the type of the report. Use the ``--list-templates`` to list what types are available.
============================================= ===========

**Examples**

Lists already created reports for cluster ID 1:

.. code-block:: bash

	$ s9s report \
		--list \
		--long \
		--cluster-id=1

Print the report ID 1 for cluster ID 1:

.. code-block:: bash

	$ s9s report \
		--cat \
		--report-id=1 \
		--cluster-id=1

Create a database growth report for cluster ID 1:

.. code-block:: bash

	$ s9s report \
		--create \
		--type=dbgrowth \
		--cluster-id=1


Delete an operation report with report ID 11:

.. code-block:: bash

	$ s9s report \
		--delete \
		--report-id=11

Print the supported templates that can be used with ``type`` value when creating a new report:

.. code-block:: bash

	$ s9s report \
		--list-templates \
		--long