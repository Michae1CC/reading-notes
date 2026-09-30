
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
- However, `SELECT` does see different data, even though they are within a single transaction, if other transactions commit changes after the first `SELECT` starts and before the second `SELECT` starts.

- Repeatable Read isolation level only sees data committed before the transaction began; it never sees either uncommitted data or changes committed by concurrent transactions during the transaction's execution.

- Serializable Isolation - emulates serial transaction execution for all committed transaction; as if transactions has been executed one after another, serially, rather than concurrently.

https://www.postgresql.org/docs/current/applevel-consistency.html

- Read/Write conflicts - if one transaction writes data and a concurrent transaction attempts to read the same data, it cannot see the work of the other transaction. The reader then appears to have executed first regardless of which started first or which committed first. If the reader also writes data which is read by a concurrent transaction there is now a transaction which appears to have run before either of the previously mentioned transactions.
- If the Serializable transaction isolation level is used for all writes and for all readds which need a consistent view of the data, no other effort is required to ensure consistency.