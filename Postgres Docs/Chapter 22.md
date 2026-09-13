
https://www.postgresql.org/docs/18/manage-ag-overview.html

- PostgreSQL database templates are pre-exisiting databases that PostgreSQL can use as a starting point when you create a new database.
- The key idea is creating a database can be done by cloning an existing database
- `template0` represents the original database state created when the PostgreSQL cluster was created. You generally don't modify it.
- `template1` this is a customisable default template. PostgreSQL normally uses `template1` when you execute `CREATE DATABASE mydb;`
- Templates are actual databases - not just a special configuration file. Data also gets copied from templates.
- Databases are destroyed using the `DROP DATABASE` command

https://www.postgresql.org/docs/18/manage-ag-tablespaces.html

- Tablespaces in PostgreSQL allow database admins to define in the file system where the representing database objects can be stored. Once created, a tablespace can be referred to by name when creating database objects.
- The location must be an exisiting, empty directory that is owned by the PostgreSQL operating system user. All objects subsequently created within the tablespace will be sotred underneath this directory. This location must not be a removable of transient storage.
- Created as follows: `CREATE TABLESPACE fastspace LOCATION '/ssd1/postgresql/data';`