
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