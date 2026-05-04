
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