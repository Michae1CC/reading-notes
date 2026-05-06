
### 17.1 Transaction Concepts

pg 797

- Transaction - A unit of program execution that accesses and possibly updates various data items. Usually initiated by a user program written in a high-level data manipulation language

### 17.3 Storage Structure

pg 804

- Volatile Storage - Does not usually survive system crashes. Examples: main memory, and cache memory
- Non-volatile storage - Survives system crashes. Examples: magnetic disk, flash drive.
- Stable Storage - Information residing in stable storage should never be lost

### 17.4 Transaction Atomcity and Durability

pg 805

- Once the changes of caused by an aborted transaction have been undone, we say that transaction has been rolled back
- A transaction that completes its execution successfully has said to be committed
- A transaction must be in one of the following states:
	- *Active* - The initial state; the transaction stays in this state while it's executing
	- *Partially Committed* - After the final statement
	- *Failed* - After the discovery that normal execution can no longer proceed
	- *Aborted* - After the transaction has been rolled back and the database has been restored to its state prior to the start of the transaction
	- *Committed* - After successful completion
- A transaction is *terminated* if it is either in a *committed* or *aborted* state

### 17.8 Transaction Isolation Levels

pg 821

- Serializable - Usually ensures serializable execution
- Repeatable Read - Allows only committed data to be read and further requires that between two reads of a data item by a transaction, no other transaction is allowed to update it
- Read Committed - Allows only committed data to be read, but does not require repeatable reads
- Read Uncommitted - Allows uncommitted data to be read. It is the lowest isolation level allowed by SQL

Consider the simple following task

```SQL
-- Source - https://stackoverflow.com/a/4036063
-- Posted by Remus Rusanu, modified by community. See post 'Timeline' for change history
-- Retrieved 2026-05-07, License - CC BY-SA 4.0

BEGIN TRANSACTION;
SELECT * FROM T;
WAITFOR DELAY '00:01:00'
SELECT * FROM T;
COMMIT;
```

- under *Read Committed*, the second SELECT may return any data. A concurrent transaction may update the record, delete it, insert new records. The second select will always see new data.
- under *Repeatable Read* the second SELECT is guaranteed to display at least the rows that were returned from the first SELECT unchanged. New rows may be added by a concurrent transaction in that one minute, but existing rows cannot be deleted nor changed
- under *Serializable* reads the second select is guaranteed to see exactly the same rows as the first. No row can change, nor deleted, nor new rows could be inserted by a concurrent transaction
