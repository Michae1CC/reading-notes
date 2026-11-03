
https://www.postgresql.org/docs/18/wal-intro.html

- Write-Ahead Logging is a standard method for ensuring data integrity
- Changes to data files must be written only after those changes have been logged, that is, after WAL records describing the changes have been flushed to permanent storage

https://www.postgresql.org/docs/18/wal-async-commit.html

- Async commit is an option that allows transactions to complete more quickly, at the cost that the most recent transactions may be lost is the database should crash
- Transaction commit is normally sync, the server waits for the transaction WAL records to be flushed to permanent storage before returning a success indication to the client\=
- Selecting async commit mode means that the server returns success as soon as the transaction is logically completed, before the WAL records it generated have actually made their way to disk
- The user can select the commit mode of each transaction, so that it is possible to have both sync and async commit transactions running concurrently
- The commit mode is controlled by the user-settable parameter `synchronous_commit`, which can be changed in any of the ways that a configuration parameter can be set