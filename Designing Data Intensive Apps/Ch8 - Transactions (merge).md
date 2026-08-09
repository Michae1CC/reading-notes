
pg 285
- Everything between `BEGIN TRANSACTION` and a `COMMIT` statement is considered to be part of the same transaction.
- Retrying a transaction abortion is a simple error-handling mechanism isn't perfect
	- If the transaction actually succeeded, but the network was interrupted while the server tried to acknowledge the successful commit to the client, then retrying the transaction causes it to be performed twice unless you have an additional application-level deduplication mechanism in place
	- If the error is due to overload or high contention between concurrent transactions, retrying the transaction will make the problem worse. Can use exponential backoff to prevent this
	- It is only worth retrying after transient errors - due to deadlock, isolation violation, networking or failover
	- If the client process crashes while retrying, and data it was trying to write to the database will be lost