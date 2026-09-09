pg 280
- _Atomicity_ - If writes are grouped together into an atomic transaction, and the transaction cannot be completed because of a fault, then the transaction is aborted and the database must discard or undo any writes it has done so far. If a transaction is was aborted, the application can be sure that it didn't change anything, so it can safely be retried.
- _Consistency_ - You have certain statements about your data that must always be true - for example, in an accounting system, credits and debits across all accounts must always be balanced. If a transaction starts with a database that is valid according to these invariants and any writes during the transaction preserve the validity - i.e. invariants are always satisfied.
- _Isolation_ - Several client may be attempting reads/writes to the same part of the database. Isolation means that concurrently executing transactions are isolated from each other; they cannot step on each other's toes. Ensures that when the transactions have committed the result is the same as if they had run serially, even though they have run concurrently.
- _Durability_ - A promise that after a transaction has been committed successfully any data it has written will not be forgotten, even if there is a hardware fault.

pg 285
- Everything between `BEGIN TRANSACTION` and a `COMMIT` statement is considered to be part of the same transaction.
- Retrying a transaction abortion is a simple error-handling mechanism isn't perfect
	- If the transaction actually succeeded, but the network was interrupted while the server tried to acknowledge the successful commit to the client, then retrying the transaction causes it to be performed twice unless you have an additional application-level deduplication mechanism in place
	- If the error is due to overload or high contention between concurrent transactions, retrying the transaction will make the problem worse. Can use exponential backoff to prevent this
	- It is only worth retrying after transient errors - due to deadlock, isolation violation, networking or failover
	- If the client process crashes while retrying, and data it was trying to write to the database will be lost

#### Read Committed

pg 290
- Makes two guarantees:
	- When your reading from the database, you will only see data that has been committed
	- When writing to the database, you will overwrite only data that has been committed
- No dirty reads which means that any writes by a transaction become visible to others only when that transaction commits
- Dirty writes happens when an earlier write that is part of a transaction has not yet committed and a later write overwrites an uncommitted value. Transactions running at the read-committed isolation level must prevent dirty writes.
- Databases prevent dirty writes by using row-level locks. When a transaction want to modify a particular row, it must first acquire a lock on that row. It must hold that lock until the transaction is committed or aborted. Only one transaction can hold the lock for any given row.


#### Multiversion Concurrency Control

pg 295

- Snapshot Isolation - each transaction reads from a consistent snapshot of the database - that is, it sees all the data that was committed in the database at the start of that. Even if the data is subsequently changed by another transaction, each transaction sees only the old data from that particular point in time. aka. "Repeatable Read", "Serializable"
- Implementations of snapshot isolation typically use write locks to prevent dirty writes, which means that a transaction that makes a write can block the progress of another transaction that writes to the same row
- Reads do not require any locks
- The database must potentially keep several committed versions of a row, because various in-progress transactions may need to see the state of the database at different points in time.

#### Preventing Lost Updates

pg 299

- The lost updates problem can occur if an application reads a value from the database, modifies it, and writes it back the modified value. If two transactions do this concurrently, one of the modifications can be lost.
- Atomic write operations - Many databases provide atomic update operations, which remove the need to implement read-modify-write cycles in application code. They are usually the best solution if your code can be expressed in terms of those operations.
- For example: `UPDATE counters SET value = value + 1 WHERE key = 'foo'`
- Explicit Locking - If the databases built-in atomic operations don't provide the necessary functionality, is for the application to explicitly lock objects that are going to be updated. Then the application can perform a read-modify-write cycle, and if any other transactions tries to concurrently update or lock the same object, it is forced to wait until the first read-modify-write cycles has completed.
- Automatically detecting lost updates - Atomic operations and locks are ways of preventing lost updates by forcing the read-modify-write cycles to happen sequentially. An alternative is to allow them to execute in parallel and, if the transaction manager detects a lost update, abort the transaction in question and force it to retry its read-modify-write cycle. Doesn't require application code to use any special database features. You have to retry aborted transactions at the application level.

#### Write Skew and Phantoms

pg 303

General pattern
- A `SELECT` query checks whether a requirement is satisfied by searching for rows that match a search condition
- Depending on the result of the first query, the application code decides how to continue
- If the application decides to go ahead, it makes a write (`INSERT`, `UPDATE` or `DELETE`) to the database and commits the transaction. The effect of this write changes the precondition of the descision of step 2. In other words, if you were to repeat the `SELECT` query from step 1 after committing the write, you would get a different result, because the write changed the set of rows matching the search condition.