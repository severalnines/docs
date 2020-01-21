.. _glossary:

Glossary
========

.. glossary::

	Active Directory
		A directory service that Microsoft developed for Windows domain networks and is included in most Windows Server operating systems as a set of processes and services.
		
		.. seealso:: http://en.wikipedia.org/wiki/Active_Directory

	Ansible
		Ansible is a free-software platform for configuring and managing computers, combines multi-node software deployment, ad hoc task execution, and configuration management. It manages nodes (Linux nodes must have Python 2.4 or later installed on them, Windows nodes require PowerShell 3.0 or later) over SSH or over PowerShell.
        
		.. seealso:: http://www.ansible.com/

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

	cron
		A time-based job scheduler in Unix-like computer operating systems.
		
		.. seealso:: https://en.wikipedia.org/wiki/Cron

	daemon
		Daemon turns other processes into daemons.
		
		.. seealso:: http://libslack.org/daemon/

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
		
		.. seealso:: https://www.freeipa.org/page/Main_Page

	garbd
		Galera arbitrator daemon.

		.. seealso:: http://galeracluster.com/documentation-webpages/arbitrator.html

	gzip
		A file format and a software application used for file compression and decompression.
		
		.. seealso:: http://www.gzip.org/
		
	HAproxy
		HAProxy is a free, very fast and reliable solution offering high availability, load balancing, and proxying for TCP and HTTP-based applications.
	
		.. seealso:: http://www.haproxy.org/

	innobackupex
		A Perl script that acts as a wrapper for the xtrabackup C program. It is a patched version of the innobackup Perl script that Oracle distributes with the InnoDB Hot Backup tool.

		.. seealso:: https://www.percona.com/doc/percona-xtrabackup/2.1/innobackupex/innobackupex_script.html
	
	innodb_buffer_pool_size
		The size in bytes of the memory buffer InnoDB uses to cache data and indexes of its tables. The default value is 8MB.
	
		.. seealso:: http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size

	IOPS
		Input/output operations per second.
		
		.. seealso:: https://en.wikipedia.org/wiki/IOPS

	keepalived
		A routing software written in C to provide simple and robust facilities for loadbalancing and high-availability to Linux system and Linux based infrastructures. Widely used for IP failover between two load balancer servers.
	
		.. seealso:: http://keepalived.org/

	knife
		Knife is a command-line tool that provides an interface between a local chef-repo and the Chef server.
	
		.. seealso:: https://docs.chef.io/knife.html

	LDAP
		The Lightweight Directory Access Protocol (LDAP) is a directory service protocol that runs on a layer above the TCP/IP stack. It provides a mechanism used to connect to, search, and modify Internet directories. The LDAP directory service is based on a client-server model.
	
		.. seealso:: http://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol

	libssh
		libssh is a multiplatform C library implementing the SSHv2 protocol on client and server side.
		
		.. seealso:: https://www.libssh.org/

	MaxScale
		MariaDB MaxScale is a next-generation database proxy that manages security, scalability and high availability in scale-out deployments.
		
		.. seealso:: https://mariadb.com/products/mariadb-maxscale

	mongodb-consistent-backup
		Tool for getting consistent backups from MongoDB Clusters and Replica Set.
		
		.. seealso:: https://github.com/Percona-Lab/mongodb_consistent_backup

	mongodump
		A utility for creating a binary export of the contents of a database. Consider using this utility as part an effective backup strategy. 
		
		.. seealso:: http://docs.mongodb.org/v2.6/reference/program/mongodump

	mongostat
		A utility to provide a quick overview of the status of a currently running mongod or mongos instance.
	
		.. seealso:: http://docs.mongodb.org/manual/reference/program/mongostat/

	MyISAM
		Previous default storage engine for MySQL for versions prior to 5.5. It doesn’t fully support transactions but in some scenarios may be faster than InnoDB. Each table is stored on disk in 3 files: .frm, .MYD, .MYI

	MySQL Group Replication
		MySQL Group Replication is a recent MySQL plugin that brings together group communication techniques and database replication, providing both a high availability and a multi-master update everywhere replication solution.
	
		.. seealso:: http://mysqlhighavailability.com/tag/mysql-group-replication/

	mysqldump
		The mysqldump client is a utility that performs logical backups, producing a set of SQL statements that can be run to reproduce the original schema objects, table data, or both. It dumps one or more MySQL database for backup or transfer to another SQL server.
		
		.. seealso:: https://dev.mysql.com/doc/refman/5.6/en/mysqldump.html

	netcat
		A computer networking service for reading from and writing to network connections using TCP or UDP.
		
		.. seealso:: http://en.wikipedia.org/wiki/Netcat

	OpenSSL
		OpenSSL is a robust, commercial-grade, and full-featured toolkit for the Transport Layer Security (TLS) and Secure Sockets Layer (SSL) protocols.
		
		.. seealso:: https://www.openssl.org/

	pg_basebackup
		A utility to take base backups of a running PostgreSQL database cluster. These are taken without affecting other clients to the database, and can be used both for point-in-time recovery and as the starting point for a log shipping or streaming replication standby servers.
		
		.. seealso:: https://www.postgresql.org/docs/9.5/static/app-pgbasebackup.html

	pg_dumpall
		A utility for writing out all PostgreSQL databases of a cluster into one script file. The script file contains SQL commands that can be used as input to psql to restore the databases. It does this by calling pg_dump for each database in a cluster
		
		.. seealso:: http://www.postgresql.org/docs/9.2/static/app-pg-dumpall.html

	pgBackRest
		A reliable backup and restore solution that can seamlessly scale up to the largest databases and workloads by utilizing algorithms that are optimized for database-specific requirements.
		
		.. seealso:: https://pgbackrest.org/

	php.ini
		PHP configuration file where you declare changes to your PHP settings.
		
		.. seealso:: http://php.net/manual/en/configuration.file.php

	Primary Component
		In addition to single node failures, the cluster may be split into several components due to network failure. In such a situation, only one of the components can continue to modify the database state to avoid history divergence. This component is called the Primary Component (PC).
		
		.. seealso:: http://galeracluster.com/documentation-webpages/weightedquorum.html

	Prometheus
		An open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach.
		
		.. seealso:: https://prometheus.io/

	ProxySQL
		ProxySQL is an open source high-performance MySQL proxy with a GPL license.
	
		.. seealso:: http://www.proxysql.com/

	Puppet
		Puppet is an open source configuration management utility.
	
		.. seealso:: https://puppetlabs.com/

	pv
		pv allows a user to see the progress of data through a pipeline, by giving information such as time elapsed, percentage completed (with progress bar), current throughput rate, total data transferred, and ETA.
		
		.. seealso:: https://linux.die.net/man/1/pv

	qpress
		qpress is a portable file archiver using QuickLZ and designed to utilize fast storage systems to their max. It's often faster than file copy because the destination is smaller than the source.
		
		.. seealso:: http://www.quicklz.com/

	socat
		Socat is a command line based utility that establishes two bidirectional byte streams and transfers data between them.
		
		.. seealso:: http://www.dest-unreach.org/socat/doc/socat.html

	s9s-admin tools
		ClusterControl helper scripts provided by Severalnines. The source code can be accessible at Severalnines Github repository.
		
		.. seealso:: https://github.com/severalnines/s9s-admin

	top
		Displays processor activity of your Linux box and also displays tasks managed by kernel in real-time.

	xinetd
		An open-source super-server daemon, runs on many Unix-like systems and manages Internet-based connectivity.
		
		.. seealso:: https://en.wikipedia.org/wiki/Xinetd

	xtrabackup
		Percona XtraBackup is an open-source hot backup utility for MySQL - based servers that doesn’t lock your database during the backup.
		
		.. seealso:: https://www.percona.com/doc/percona-xtrabackup/2.2/
	
	XtraDB
		Percona XtraDB is an enhanced version of the InnoDB storage engine, designed to better scale on modern hardware, and including a variety of other features useful in high performance environments. It is fully backwards compatible, and so can be used as a drop-in replacement for standard InnoDB.
		
		.. seealso:: https://www.percona.com/doc/percona-server/5.5/percona_xtradb.html?id=Percona-XtraDB:start