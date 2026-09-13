
https://www.postgresql.org/docs/current/postgres-user.html

- A database cluster is a single directory under which all data will be sotred. We call this directory a _data directory_ or _data area_
- To initialize a database cluster manually, run `initdb` and specify the desired file system location of the database cluster with the `-D` option
- The default client authentication setup allows any local user to connect to the db and even become the db su
- `initdb` also initializes the default locale for the db cluster
- It is not advisable to top try to use a secondary volume's topmost directory (mountpoint) as the data directory
- Generally, any file system with POSIX semantics can be used for pg
- It is possible to use an NFS file system for storing the pg SQL data directory

https://www.postgresql.org/docs/current/kernel-resources.html

- Important resources limits:
	- Number of processes per user
	- number of open files per process
	- amount of mem available to each process
- Each of these have a "hard" and "soft" limit. The soft limit is what actually counts but is can be changes by the user up to the hard limit
- The hard limit can only be changed by the root user
- The system call `strlimit` is responsible for setting these parameters

https://www.postgresql.org/docs/current/upgrading.html

- For major releases of PostgreSQL, the internal data storage format is subject to change. Traditional methods for moving data to a new version is to dump and restore the database, though this can be slow. A faster method is `pg_upgrade`.