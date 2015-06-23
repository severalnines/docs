.. _glossary:

Glossary
========

.. glossary::

	Active Directory
		A directory service that Microsoft developed for Windows domain networks and is included in most Windows Server operating systems as a set of processes and services.
		
		.. seealso:: http://en.wikipedia.org/wiki/Active_Directory

	arbiter
		MongoDB instances that are part of a replica set but do not hold data. Arbiters participate in elections in order to break ties.
		
		.. seealso:: http://docs.mongodb.org/manual/tutorial/add-replica-set-arbiter/

	Chef
		Chef is a configuration management tool written in Ruby and Erlang.
		
		.. seealso:: https://www.chef.io/

	chef-client
		A chef-client is an agent that runs locally on every node that is under management by Chef. When a chef-client is run, it will perform all of the steps that are required to bring the node into the expected state.
		
		.. seealso:: https://docs.chef.io/chef_client.html

	CMON
		ClusterControl controller backend service.

	DN
		A Distinguished Name (DN) is used to uniquely name a directory object.
		
		.. seealso:: https://tools.ietf.org/html/rfc4514

	Docker
		An open-source project that automates the deployment of applications inside software containers, by providing an additional layer of abstraction and automation of operating-system-level virtualization on Linux.
		
		.. seealso:: https://www.docker.com/

	FQDN
		A fully qualified domain name (FQDN), sometimes also referred to as an absolute domain name, is a domain name that specifies its exact location in the tree hierarchy of the Domain Name System (DNS).
		
		.. seealso:: https://en.wikipedia.org/wiki/Fully_qualified_domain_name

	FreeIPA
		FreeIPA is a Red Hat sponsored open source project which aims to provide an easily managed Identity, Policy and Audit (IPA) suite primarily targeted towards networks of Linux and Unix computers. It is easy to install/configure, and is an integrated security information management solution combining Linux (Fedora), 389 Directory Server, MIT Kerberos, NTP, DNS, Dogtag (Certificate System).

	garbd
		Galera arbitrator daemon.

		.. seealso:: http://galeracluster.com/documentation-webpages/arbitrator.html
		
	HAproxy
		HAProxy is a free, very fast and reliable solution offering high availability, load balancing, and proxying for TCP and HTTP-based applications.
	
		.. seealso:: http://www.haproxy.org/

	innobackupex
		A Perl script that acts as a wrapper for the xtrabackup C program. It is a patched version of the innobackup Perl script that Oracle distributes with the InnoDB Hot Backup tool.

		.. seealso:: https://www.percona.com/doc/percona-xtrabackup/2.1/innobackupex/innobackupex_script.html
	
	innodb_buffer_pool_size
		The size in bytes of the memory buffer InnoDB uses to cache data and indexes of its tables. The default value is 8MB.
	
		.. seealso:: http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size

	keepalived
		A routing software written in C to provide simple and robust facilities for loadbalancing and high-availability to Linux system and Linux based infrastructures. Widely used for IP failover between two load balancer servers.
	
		.. seealso:: http://keepalived.org/

	knife
		Knife is a command-line tool that provides an interface between a local chef-repo and the Chef server.
	
		.. seealso:: https://docs.chef.io/knife.html

	LDAP
		The Lightweight Directory Access Protocol (LDAP) is a directory service protocol that runs on a layer above the TCP/IP stack. It provides a mechanism used to connect to, search, and modify Internet directories. The LDAP directory service is based on a client-server model.
	
		.. seealso:: http://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol

	mongodump
		A utility for creating a binary export of the contents of a database. Consider using this utility as part an effective backup strategy. 
		
		.. seealso:: http://docs.mongodb.org/v2.6/reference/program/mongodump

	mongostat
		A utility to provide a quick overview of the status of a currently running mongod or mongos instance.
	
		.. seealso:: http://docs.mongodb.org/manual/reference/program/mongostat/

	MyISAM
		Previous default storage engine for MySQL for versions prior to 5.5. It doesn’t fully support transactions but in some scenarios may be faster than InnoDB. Each table is stored on disk in 3 files: .frm, .MYD, .MYI

	mysqldump
		The mysqldump client is a utility that performs logical backups, producing a set of SQL statements that can be run to reproduce the original schema objects, table data, or both. It dumps one or more MySQL database for backup or transfer to another SQL server.
		
		.. seealso:: https://dev.mysql.com/doc/refman/5.6/en/mysqldump.html

	netcat
		A computer networking service for reading from and writing to network connections using TCP or UDP.
		
		.. seealso:: http://en.wikipedia.org/wiki/Netcat

	php.ini
		PHP configuration file where you declare changes to your PHP settings.
		
		.. seealso:: http://php.net/manual/en/configuration.file.php

	Primary Component
		In addition to single node failures, the cluster may be split into several components due to network failure. In such a situation, only one of the components can continue to modify the database state to avoid history divergence. This component is called the Primary Component (PC).
		
		.. seealso:: http://galeracluster.com/documentation-webpages/weightedquorum.html

	Puppet
		Puppet is an open source configuration management utility.
	
		.. seealso:: https://puppetlabs.com/
		
	s9s-admin tool
		ClusterControl helper scripts provided by Severalnines. The source code can be accessible at Severalnines Github repository.
		
		.. seealso:: https://github.com/severalnines/s9s-admin

	top
		Displays processor activity of your Linux box and also displays tasks managed by kernel in real-time.

	xtrabackup
		Percona XtraBackup is an open-source hot backup utility for MySQL - based servers that doesn’t lock your database during the backup.
		
		.. seealso:: https://www.percona.com/doc/percona-xtrabackup/2.2/
	
	XtraDB
		Percona XtraDB is an enhanced version of the InnoDB storage engine, designed to better scale on modern hardware, and including a variety of other features useful in high performance environments. It is fully backwards compatible, and so can be used as a drop-in replacement for standard InnoDB.