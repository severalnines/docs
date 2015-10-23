Performance
-----------

Database performance monitoring and advisors.

DB Status
``````````

DB Status provides a quick overview of MySQL status across all your database nodes. You can use the *Search* text field to filter the result.

.. Note:: You can check *Hide all zero values* to filter out any status that returned 0.

DB Variables
````````````

DB Variables provide a quick overview of MySQL variables that are set across all your database nodes. You can use the *Search* text field to filter the result.

.. Note:: Red text means that the variable setting is different. In some cases that is acceptable (e.g., IP address of the node).

Advisors
````````

Lists of scheduled Advisors created in Developer Studio using `ClusterControl DSL <../../dsl.html>`_.

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