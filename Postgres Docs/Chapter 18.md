
https://www.postgresql.org/docs/current/postgres-user.html

- A database cluster is a single directory under which all data will be sotred. We call this directory a _data directory_ or _data area_
- To initialize a database cluster manually, run `initdb` and specify the desired file system location of the database cluster with the `-D` option
- The default client authentication setup allows any local user to connect to the db and even become the db su
- `initdb` also initializes the default locale for the db cluster
- It is not advisable to top try to use a secondary volume's topmost directory (mountpoint) as the data directory
- Generally, any file system with POSIX semantics can be used for pg
- It is possible to use an NFS file system for storing the pg SQL data directory