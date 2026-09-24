
https://www.postgresql.org/docs/current/transaction-iso.html

- The SQL standard defines four levels of transaction isolation
- The most strict is Serializable, which is defined by the standard that any concurrent execution of a set of Serializable transactions is guaranteed to produce the same effect as running them one at a time in some order
- Phenomena prohibited at various levels arre:
	- Dirty reads - A transaction reads data written by a concurrent uncommited transaction
	- Non-repeatedable read - A transaction re-reads data that it has previously reads and finds that data has been modified by another transaction
	- Phantom read - A transaction re-executes a query returning a set of rows that satisfy a search condition and finds that the set of rows satisfying the condition has chaged due to recently-committed transaction
	- Serialization anomaly - The result of a successfully committing a group of transaction is inconsistent with all possible orderings of running those transaction one at a time
- To set the transaction isolation level of a transaction, use the command `SET TRANSACTION`

- Read commit is the default isolation level. When a transaction uses this isolation level, a `SELECT` query sees only data committed before the query began; it never sees either uncommitted data or changes committed by concurrent transactions during the query's execution. In effect, a `SELECT` query sees a snapshot of the database as of the instance the query begins to run.