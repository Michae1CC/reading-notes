
https://www.postgresql.org/docs/18/high-availability.html

- Servers that can modify data are called read/write, master or primary servers. Servers that track changes in the primary are called standby or secondary servers. A standby server that cannot be connected to until it is promised to a primary server is called a warm standby server, and one that can accept connections and serves read-only queries is called a hot standby server
- Hot standby is the term used to describe the ability to connect to the server and run read-only queries while the server is in archive or standby mode. This is useful for both replication purposes and for restoring a backup to a desired state with great precision.