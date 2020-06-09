.. include:: <isotech.txt>

s9s user
--------

Manage users.

**Usage**

.. code-block:: bash

	s9s user {command} {options}


**Command**

====================================== ===========
Name, shorthand                        Description
====================================== ===========
|minus|\ |minus|\ add-key              Registers a new public key for an existing user. After the command, the user will be able to authenticate with the private part of the registered public key, no password will be necessary.
|minus|\ |minus|\ add-to-group         Adds the user to a group.
|minus|\ |minus|\ change-password      Modifies the password of an existing user. The password is not a simple property, so it can not be change using the ``--set`` option, this special command line option has to be used.
|minus|\ |minus|\ create               Registers a new user (create a new user account) on the controller and grant access to the ClusterControl system. The user name of the new account should be the command line argument.
|minus|\ |minus|\ delete               Deletes existing user.
|minus|\ |minus|\ disable              Disables the user (turn on the "disabled" flag of the user). The users that are disabled are not able to log in.
|minus|\ |minus|\ enable               Enables the user. This will clear the "disabled" flag of the user so that the user will be able to log in again. The "suspended" flag will also be cleared, the failed login counter set to 0 and the date and time of the last failed login gets deleted, so users who are suspended for failed login attempts will also be able to log in.

|minus|\ |minus|\ whoami               Prints out the current user only.
|minus|\ |minus|\ list                 List the users registered for the ClusterControl controller.
|minus|\ |minus|\ list-groups          List the user groups maintained by the ClusterControl controller.
|minus|\ |minus|\ list-keys            Lists the public keys registered in the controller for the specified user. Please note that viewing the public keys require special privileges, ordinary users can not view the public keys of other users.
|minus|\ |minus|\ password-reset       Resets the password for the user using the "forgot password" email schema. This option must be used twice to change the password, once without a token to send an email about the password reset and once with the token received in the email.
|minus|\ |minus|\ set                  Changes the specified properties of the user.
|minus|\ |minus|\ remove-from-group    Removes the user from a group.
|minus|\ |minus|\ set-group            Sets the primary group for the specified user. The primary group is the first group the user belongs to. This option will remove the user from this primary group and add it to the group specified by the ``--group`` command line option.
|minus|\ |minus|\ stat                 Prints detailed information about the specified user(s).
|minus|\ |minus|\ whoami               Same as ``--list``, but only lists the current user, the user that authenticated on the controller.
====================================== ===========

**Options**

============================================= ===========
Name, shorthand                               Description
============================================= ===========
|minus|\ |minus|\ cmon-user, -u               Username on the CMON system.
|minus|\ |minus|\ group=GROUPNAME             Set the name of the group. For example when a new user is created this option can be used to control what will be the primary group of the new user. It is also possible to filter the users by the group name while listing them.
|minus|\ |minus|\ create-group                If this command line option is provided and the group for the new user does not exist the group will be created together with the new user.
|minus|\ |minus|\ first-name=NAME             Sets the first name of the user.
|minus|\ |minus|\ last-name=NAME.             Sets the last name of the user.
|minus|\ |minus|\ public-key-file=FILENAME    The name of the file where the public key is stored. Please note that this currently only works with the ``--add-key`` option.
|minus|\ |minus|\ title=TITLE                 The title prefix (e.g. Dr.) for the user.
|minus|\ |minus|\ email-address=ADDRESS       The email address for the user.
|minus|\ |minus|\ new-password=PASSWORD       The new password when changing the password.
|minus|\ |minus|\ old-password=PASSWORD       The old password when changing the password.
|minus|\ |minus|\ user-format[=FORMATSTRING]  The string that controls the format of the printed information about the users. See `User Formatting`_.
============================================= ===========

Creating the First User
+++++++++++++++++++++++

To  use the s9s command line tool a Cmon User Account is needed to authenticate on the Cmon Controller. These user accounts can be created using the s9s program itself by either authenticating with a pre-existing user account or bootstrapping the user management creating the very first user. The following section describes the authentication and the creation of the first user in detail.

If there is a username specified either in the command line using the ``--cmon-user`` (or ``-u``) options or  in  the  configuration file (either the ``~/.s9s/s9s.conf`` or the ``/etc/s9s.conf`` file using the cmon_user variable name) the program will try to authenticate with this username. Creating the very first user account is of course not possible this way. The ``--cmon-user`` option and the ``cmon_user`` variable is not for specifying what user we want to create, it is for setting what user we want to use for the connection.

If no user name is set for the authentication and a user creation is requested the s9s will try to send a request to the controller through a named pipe. This is the only way a user account can be created without authenticating with an existing user account, this is the only way the very first first user can be created. Here is an example:

.. code-block:: bash

	 $ s9s user --create \
	 	--group=admins \
		--generate-key \
		--controller=https://192.168.1.127:9501 \
		--new-password="MyS3cr3tpass" \
		--email-address="laszlo@severalnines.com" \
		admin

Please consider the following:

1. There is no ``--cmon-user`` specified, this is the first user we create, we do not have any pre-existing user account. This  command line is for creating the very first user. Please check out the examples section to see how to create additional users.
2. This is the first run, so we assume no ``~/.s9s/s9s.conf`` configuration file exists, there is no user name there either.
3. In the example, we create the user with the username "admin", and it is the command line argument of the program.
4. The command specifies the controller to be at https://192.168.1.127:9501. The HTTPS protocol will be used later, but to create this first user the s9s will try to use SSH and sudo to access the controller's named pipe on the specified host. For this to work the UNIX user running this command has to have a passwordless SSH and sudo set up to the remote host. If the specified host is the localhost the user do not need SSH access, but still needs to be root or have a passwordless sudo access because of course the named pipe is not accessible for anyone.
5. Since the UNIX user has no s9s configuration file it will be created. The controller URL and the user name will be stored in it under ``~/.s9s/s9s.conf``. The next time this user runs the program, it will use this "admin" user unless other user name is set in the command line and will try to connect this controller unless other controller is set in the command line.
6. The password will be set for the user on the controller, but the password will never be stored in the configuration file.
7. The ``--generate-key`` option is provided, so a new private/public key pair will be generated, then stored in the ``~/s9s/`` directory and the public key will be registered on the controller for the new user. The next time the program run it will find the username in the configuration file, find the private key in place for the user and will automatically authenticate without a password. The command line options will always have precedence, so this automatic authentication is simply the default way, the password authentication is always available.
8. The group for the new user is set to "admins", so this user will have special privileges. It is always a good idea to create the very first user with special privileges, then other users can be created by this administrator account.

User List
+++++++++

Using the ``--list`` and ``--long`` command line options a detailed list of the users can be printed. Here is an example of such a list:

.. code-block:: bash
	
	$ s9s user --list --long worf jadzia
	A ID UNAME  GNAME EMAIL           REALNAME
	- 11 jadzia ds9   dax@ds9.com     Lt. Jadzia Dax
	A 12 worf   ds9   warrior@ds9.com Lt. Worf
	Total: 12

Please note that there are a total of 12 users defined on the system, but only two of those are printed because we filtered the list with the command line arguments.

The list contain the following fields:

========= =====
Field     Description
========= =====
A         Shows the authentication status. If this field shows the letter 'A' the user is authenticated with the current connection.
ID        Shows the user ID, a unique numerical ID identifying the user.
UNAME     The username.
GNAME     The name of the primary group of the user. All user belongs to at least one group, the primary group.
EMAIL     The email address of the user.
REALNAME  The real name of the user that consists first name, last name and some other parts, printed here as a single string composed all the available components.
========= =====

User Formatting
+++++++++++++++

When this command line option is used the specified information will be printed instead of the default columns. The format string uses the ``%`` character to mark variable fields and flag characters as they are specified in the standard ``printf()`` C library functions. The '%' specifiers are ended by field name letters to refer to various properties of the users. 

The ``%+12I`` format string for example has the ``+12`` flag characters in it with the standard meaning: the field will be 12 character wide and the ``+`` or ``-`` sign will always be printed with the number. The properties of the user are encoded by letters. The in the ``%16N`` for example the letter ``N`` encodes the ``username`` field, so username of the user will be substituted. Standard ``\`` notation is also available, ``\n`` for example encodes a new-line character.

The s9s-tools support the following fields:

====== =====
Field  Description
====== =====
d      The distinguished name of the user. This currently has meaning only for users originated from an LDAP server.
F      The full name of the user.
f      The first name of the user.
G      The names of groups the given user belongs to.
I      The unique numerical ID of the user.
j      The job title of the user.
l      The last name of the user.
M      The email address of the user.
m      The middle name of the user.
N      The username for the user.
o      The origin of the user, the place what used to store the original instance of the user. The possible values are "CmonDb" for users from the Cmon Database or "LDAP" for users from the LDAP server.
P      The CDT path of the user.
t      The title of the user (e.g. "Dr.").
====== =====

**Examples**

Create a remote s9s user and generate a SSH key for the user:

.. code-block:: bash

	$ s9s user --create \
		--generate-key \
		--cmon-user=dba \
		--controller="https://10.0.1.12:9501"

List out all users:

.. code-block:: bash

	$ s9s user --list --long
	A ID UNAME      GNAME  EMAIL REALNAME
	-  1 system     admins -     System User
	-  2 nobody     nobody -     -
	A  3 dba        users  -     -
	-  4 remote_dba users  -     -


Register a new public key for the active user:

.. code-block:: bash

	$ s9s user \
		--add-key \
		--public-key-file=/home/pipas/.ssh/id_rsa.pub \
		--public-key-name=The_SSH_key
		
Add user "myuser" into group "admins":

.. code-block:: bash

	$ s9s user \
		--add-to-group \
		--group=admins \
		myuser

Set a new password for user "pipas":

.. code-block:: bash

	$ s9s user \
		--change-password \
		--new-password="MyS3cr3tpass" \
		pipas

Create a new user called "john" under group "dba":

.. code-block:: bash

	$ s9s user \
		--create \
		--group=dba \
		--create-group \
		--generate-key \
		--new-password=s3cr3tP455 \
		--email-address=johndoe@somewhere.com \
		--first-name=John \
		--last-name=Doe \
		--batch \
		john


Delete an existing user called "mydba":

.. code-block:: bash

	$ s9s user \
		--delete \
		mydba

Disable user nobody:

.. code-block:: bash

	$ s9s user \
		--cmon-user=system \
		--password=secret \
		--disable \
		nobody
		
Enable user nobody:

.. code-block:: bash

	$ s9s user \
		--cmon-user=system \
		--password=secret \
		--enable \
		nobody

Reset password for a user called "dba". One has to obtain the one-time token which will be sent to the registered email address of the corresponding user, followed by the actual password modification command with ``--token`` and ``--new-password`` parameters:

.. code-block:: bash

	$ s9s user \
		--password-reset \
		dba
	$ ## check the mail inbox of the respective user
	$ s9s user \
		--password-reset \
		--token="98197ee4b5584cedba88ef1f583a1258" \
		--new-password="newp455w0rd"
		dba
									
Set a primary group for user "dba":

.. code-block:: bash

	$ s9s user \
		--set-group \
		--group=admins \
		dba