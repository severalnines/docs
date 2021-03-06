.. _TimeScaleDB - Performance:

Performance
-----------

Database performance monitoring and advisors.

.. _TimeScaleDB - Performance - DB Variables:

DB Variables
++++++++++++

DB Variables provide a quick overview of TimeScaleDB variables that are set across all your database nodes. You can use the *Search* text field to filter the result.

.. Note:: Red text means that the variable setting is different. In some cases that is acceptable (e.g., IP address of the node).

.. _TimeScaleDB - Performance - Advisors:

Advisors
++++++++

Lists of scheduled Advisors created in Developer Studio using :ref:`ClusterControl DSL`.

* **Expand all**
	- Expands all advisors with detailed.

* **Collapse all**
	- Lists the advisors summary.

* **Filter by status**
	- Shows advisors based on status - Ok, Warning, Critical.
	
* **DB Instance**
	- The database server the advisor running on.

* **Status**
	- Advisor status - Ok, Warning, Critical.

* **Advice**
	- The advisor's decision based on the justification.

* **Justification**
	- The result of advisors' execution.
	
DB Growth
+++++++++

Provides a summary of your database and table growth on daily basis so you can track the dataset growth on your databases. Click on listed database will further detail the growth summary per table in the bottom section. Click on "Refresh Data" to trigger a job to re-calculate the database size.

Schema Analyzer
+++++++++++++++

Analyzes database tables for missing primary keys and redundant indexes. ClusterControl will periodically check the schemas for these (default every 8 hours or every after CMON restart), and raise an alarm if necessary.