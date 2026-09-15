
https://www.postgresql.org/docs/18/monitoring-ps.html

- Since the collection of statistics adds some overhead to query execution, the system can be configured to collect or not collection information. This is controlled by parameters that are normally set in `postgresql.conf`. Generally applies to all server processes.
- Tool for monitoring database activity is the `pg_locks` system table. It allows the database admin to view information about the outstanding locks in the lock manager. Can be used to:
	- View all locks currently outstanding, all the locks on relations in a particular database, on a relation, held by a pg session
	- Determine the relation in the current db with the most ungranted locks
	- Determine the effect of lock contention on overall db performance