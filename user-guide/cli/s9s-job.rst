.. include:: <isotech.txt>

s9s job
-------

View jobs.

**Usage**

.. code-block:: bash

	s9s job {command} {options}

**Command**

====================================== ===================
Name, shorthand                        Description
====================================== ===================
|minus|\ |minus|\ clone                Creates a copy of the job to re-run it. The clone will have all the properties the original job had and will be executed the same way as new jobs are executed. If the ``--cluster-id`` command line option is used the given cluster will execute the job, if not, the clone will have the same cluster the original job had.
|minus|\ |minus|\ delete               Deletes the job referenced by the job ID.
|minus|\ |minus|\ fail                 Creates a job that does nothing and fails.
|minus|\ |minus|\ kill                 This  command line option can be used to send a signal to a running Cmon Job in order to abort its execution. The job subsystem is not preemptive in the controller, so the job only will be actually aborted if and when the job supports aborting what it is doing.
|minus|\ |minus|\ list, -L             Lists the jobs. See `Job List`_.
|minus|\ |minus|\ log                  Prints the job messages of the specified job.
|minus|\ |minus|\ success              Creates a job that does nothing and succeeds.
|minus|\ |minus|\ wait                 Waits for the specified job to end. While waiting, a progress bar will be shown unless the silent mode is set.
====================================== ===================

**Options**

============================================= ===========
Name, shorthand                               Description
============================================= ===========
*NEWLY CREATED JOB*
---------------------------------------------------------
|minus|\ |minus|\ job-tags=LIST               List of one of more strings separated by either ``,`` or ``;`` to be added as tags to a newly created job if a job is indeed created.
|minus|\ |minus|\ log                         Waits for the specified job to end. While waiting, the job logs will be shown unless the silent mode is set.
|minus|\ |minus|\ recurrence=CRONTABSTRING    Creates recurring jobs, jobs that are repeated over and over again until they are manually deleted. See `Crontab`_.
|minus|\ |minus|\ schedule=DATETIME           The job will not be executed now but it is scheduled to execute later. The datetime string is sent to the backend, so all the formats are supported that is supported by the controller.
|minus|\ |minus|\ timeout=SECONDS             Sets the timeout for the created job. If the execution of the job is not done before the timeout counted from the start time of the job expires the job will fail. Some jobs might not support the timeout feature, the controller might ignore this value.
|minus|\ |minus|\ wait                        Waits for the specified job to end. While waiting, a progress bar will be shown unless the silent mode is set.
--------------------------------------------- -----------
*JOB RELATED OPTIONS*
---------------------------------------------------------
|minus|\ |minus|\ job-id=ID                   The job ID of the job to handle or view.
|minus|\ |minus|\ from=DATE&TIME              Controls the start time of the period that will be printed in the job list.
|minus|\ |minus|\ limit=NUMBER                Limits the number of jobs printed.
|minus|\ |minus|\ offset=NUMBER               Controls the relative index of the first item printed.
|minus|\ |minus|\ show-aborted                Turn on the job state filtering and show jobs that are in aborted state. This command line option can be used while printing job lists together with the other ``--show-*`` options.
|minus|\ |minus|\ show-defined                Turn on the job state filtering and show jobs that are in defined state. This command line option can be used while printing job lists together with the other ``--show-*`` options.
|minus|\ |minus|\ show-failed                 Turn on the job state filtering and show jobs that are failed. This command line option can be used while printing job lists together with the other ``--show-*`` options.
|minus|\ |minus|\ show-finished               Turn on the job state filtering and show jobs that are finished. This command line option can be used while printing job lists together with the other ``--show-*`` options.
|minus|\ |minus|\ show-running                Turn on the job state filtering and show jobs that are running. This command line option can be used while printing job lists together with the other ``--show-*`` options.
|minus|\ |minus|\ show-scheduled              Turn  on  the job state filtering and show jobs that are scheduled. This command line option can be used while printing job lists together with the other ``--show-*`` options.
|minus|\ |minus|\ until=DATE&TIME             Controls the end time of the period that will be printed in the job list.
|minus|\ |minus|\ log-format=FORMATSTRING     The string that controls the format of the printed log and job messages. See `Log Format Variables`_.
|minus|\ |minus|\ with-tags=LIST              List of one of more strings separated by either ``,`` or ``;`` to be used as a filter when printing information about jobs. When this command line option is provided only the jobs that has any of the tags will be printed.
|minus|\ |minus|\ without-tags=LIST           List of one of more strings separated by either ``,`` or ``;`` to be used as a filter when printing information about jobs. When this command line option is provided the jobs that has any of the tags will not be printed.
============================================= ===========

Crontab
+++++++

Every time the job is repeated a new job will be instantiated by copying the original recurring job and starting the copy. The option argument is a crontab style string defining the recurrence of the job.

The crontab string must have exactly five space separated fields as follows:

================ ========
Field            Value
================ ========
minute           0 - 59
hour             0 - 23
day of the month 1 - 31
month            1 - 12
day of the week  0 - 7
================ ========

All the fields may be a simple expression or a list of simple expression separated by a comma (``,``). So to clarify the fields are separated by space can contain subfields separated by comma.

The simple expression is either a star (``*``) representing "all the possible values", an integer number representing the given minute, hour, day or month (e.g. 5 for the fifth day of the month), or two numbers separated by a dash representing an interval (e.g. 8-16 representing every hour from 8 to 16). The simple expression can also define a "step" value, so for example ``*/2`` might stand for "every other hour" or  ``8-16/2``  might stand for "every other hour between 8 and 16 or ``*/2`` might say "every other hours".

Please check `crontab man page <https://linux.die.net/man/5/crontab>`_ for more details.

Job List
++++++++

Using the ``--list`` command line option a detailed list of jobs can be printed (the ``--long`` option results in even more details). Here is an example of such a list:

.. code-block:: bash

	 $ s9s job --list
	 ID CID STATE    OWNER  GROUP  CREATED             RDY  TITLE
	 1   0 FINISHED pipas  users  2017-04-25 14:12:31 100% Create MySQL Cluster
	 2   1 FINISHED system admins 03:00:15            100% Removing Old Backups
	 Total: 2

The list contains the following fields:

========= ===================
Field     Description
========= ===================
ID        The numerical ID of the job. The ``--job-id`` command line option can be used to pass such ID numbers.
CID       The cluster ID. Most of the jobs are related to one specific cluster so those have a cluster ID in this field. Some of the jobs are not related to any cluster, so they are shown with cluster ID 0.
STATE     The state of the job. The possible values are DEFINED, DEQUEUED, RUNNING, SCHEDULED, ABORTED, FINISHED and FAILED.
OWNER     The user name of the user who owns the job.
GROUP     The name of the group owner.
CREATED   The  date  and time showing when the job was created. The format of this timestamp can be set using the ``--date-format`` command line option.
RDY       A progress indicator showing how many percent of the job was done. Please note that some jobs has no estimation available and so this value remains 0% for the entire execution time.
TITLE     A short, human readable description of the job.
========= ===================

Log Format Variables
++++++++++++++++++++

The format string uses the ``%`` character to  mark variable fields, flag characters as they are specified in the standard ``printf()`` C library functions and its own field name letters to refer to the various properties of the messages. 

The ``%+12I`` format string for example has the "+12" flag characters in it with the standard meaning: the field will be 12 character wide and the ``+`` or ``-`` sign will always be printed with the number. Standard ``\`` notation is also available, ``\n`` for example encodes a new-line character.

The properties of the message are encoded by letters. The in the ``%-5L`` for example the letter ``L`` encodes the "line-number" field, so the number of the source line that produced the message will be substituted. The program supports the following fields:

========= ===================
Variable  Description
========= ===================
B         The base name of the source file that produced the message.
C         The creation date and time that marks the exact moment when the message was created. The format of the date&time substituted can be set using the ``--date-format`` command line option.
F         The name  of the source file that created the message. This is similar to the ``B`` fields, but instead of the base name the entire file name will be substituted.
I         The ID of the message, a numerical ID that can be used as a unique identifier for the message.
J         The Job ID.
L         The line number in the source file where the message was created. This property is implemented mostly for debugging purposes.
M         The message text.
S         The severity of the message in text format. This field can be "MESSAGE", "WARNING" or "FAILURE".
T         The creation time of the message. This is similar to the ``C`` field, but shows only hours, minutes and seconds instead of the full date and time.
%         The ``%`` character itself.
========= ===================

**Examples**

List jobs:

.. code-block:: bash

	$ s9s job --list
	10235 RUNNING  dba     2017-01-09 10:10:17   2% Create Galera Cluster
	10233 FAILED   dba     2017-01-09 10:09:41   0% Create Galera Cluster

The s9s client will send a job that will be executed in the background by cmon. It will printout the job ID, for example:
"Job with ID 57 registered."

It is then possible to attach to the job to find out the progress:

.. code-block:: bash

	$ s9s job --wait --job-id=57

View job log messages of job ID 10235:

.. code-block:: bash

	$ s9s job --log  --job-id=10235
	
Delete the job that has the job ID 41:

.. code-block:: bash

	$ s9s job --delete --job-id=42

Create a job that runs in every 5 minutes and does nothing at all. This can be used for testing and demonstrating the recurring jobs without doing any significant or dangerous operations.

.. code-block:: bash

	$ s9s job --success --recurrence="*/5 * * * *"

Clone job ID 14, run it in as a new job and see the job messages:

.. code-block:: bash

	$ s9s job --clone \
		--job-id=14 \
		--log
		--clone

Kill a running job with job ID 20:

.. code-block:: bash

	$ s9s job --kill \
		--job-id=20
