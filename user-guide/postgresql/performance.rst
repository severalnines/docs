.. _PostgreSQL - Performance:

Performance
-----------

Database performance monitoring and advisors.

.. _PostgreSQL - Performance - DB Variables:

DB Variables
++++++++++++

DB Variables provide a quick overview of PostgreSQL variables that are set across all your database nodes. You can use the *Search* text field to filter the result.

.. Note:: Red text means that the variable setting is different. In some cases that is acceptable (e.g., IP address of the node).

.. _PostgreSQL - Performance - Advisors:

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