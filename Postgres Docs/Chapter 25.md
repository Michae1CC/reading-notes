
https://www.postgresql.org/docs/18/backup.html

- The idea behind a SQL sump is to generate a file with SQL commands that, when fed back to the server, will re-create the database in the same state at the time of the dump
- PostgreSQL provides a utility program `pg_dump` for this purpose. Basic usage: `pg_dump dbname > dumpfile`
- `pg_dumo` is a regular PostgreSQL client application - this means you can perform this backup procedure from any remote host that has access to the database.
- In order to back up the entire database you almost always have to run it as a database superuser.
- Text files created by `pg_dump` are intended to be read by the psql program using its default setting: `psql -X dbname < dumpfile`

https://www.postgresql.org/docs/18/backup-file.html

- An alternative strategy is to directly copy files that PostgreSQL uses to store data in the database, example: `tar -cf backup.tar /usr/local/pgsql/data`
- Two restrictions make this method impractical/inferior to `pg_dump`
	- The database server must be shutdown in order to get a usable backup
	- File system backups only work for complete backup and restoration of an entire database cluster.