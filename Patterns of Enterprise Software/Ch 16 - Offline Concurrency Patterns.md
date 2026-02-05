
pg 415

- Optimistic Offline Lock solves this problem by validating that the changes about to be committed by one session don't conflict with the changes of another session
- A successful pre-commit validation is, in sense, obtaining a lock indicating it's okay to go ahead with the changes to the record data
- Optimistic Offline Lock assumes that the chance of conflict is low
- An Optimistic Offline Lock is obtained by validating that, in the time since a session loaded a record, another session hasn't altered it. It can be acquired at any time, but is valid only during the system transaction in which it is obtained
- The most common implementation is to associate a version number with each record in your system with each record in your system
- Once verification succeeds, all changes, including an increment of the version, can be committed
- The version increment is what prevents inconsistent record data
- It's a bad idea to use the modification timestamp rather than a version count for your optimistic check because system clocks are simply too unreliable, especially if your optimistic checks because system clocks are simply too unreliable, especially if you're coordinating across multiple servers
- Often implementing Optimistic Offline Lock is left at including the version in UPDATE and DELETE statements, but this fails to address the problem of an inconsistent read
- Concurrency management is as much a domain issue as it is a technical one
- Appropriate when the chance of conflict between any two business transactions is low


### Pessimistic Offline Lock

pg 426

- Prevents conflicts between concurrent business transactions by allowing only one business transaction at a time to access data
- Implemented in three phases:
	- Determining what type of lock you need
	- Building a lock manager
	- Defining procedures for a business transaction with these locks
- Exclusive write lock - require only that a business transaction acquire a lock in order to edit session data. This avoids conflict by not allowing two business transactions to make changes to the same record simultaneously. Ignores reading of data
- It if becomes critical that a business transaction must always have the most recent data, regardless of its intention to edit, use the exclusive read lock
- Read/write lock
	- Are mutually exclusive, i.e. a record can't be write-locked if any business transaction owns a read lock on it; it can't be read-locked if any business transaction owns a write lock on it
	- Concurrent read locks are acceptable. The existence of single read lock prevents any business transaction from editing the record.
- It's not a bad idea to include Pessimistic Offline Lock in your Domain Model
- What is usually locked is actually the ID, or primary key that we use to find the objects
- The simplest rule for releasing a lock prior to completion might be allowable, depending on your lock type and your intention to use that object again within the transaction
- If a client machine crashes in the middle of a transaction, that lost transaction is unable to complete and release anu owned locks
	- Timeouts can be implemented by registering a utility object that releases all locks when the HTTP session becomes invalid
	- Another option is to associate a timestamp wit each lock and consider invalid any lock older than a certain age
- Pessimistic Offline Lock is appropriate when the chance of conflict between concurrent sessions is high - a user should never have to throw away work


### Course grained lock

pg 438

- Locks a set of related objects with a single lock
- Simplifies the locking action and frees you from having to load all the members of a group in order to lock them
- First step in implementing Coarse-Grained Lock is to create a single point of contention for locking a group of objects.
- This makes only one lock necessary for locking the entire set, then you provide the shortest path possible to finding that single lock point in order to minimize the group members that must be identified and possibly loaded into memory
- Aggregate - a cluster of associated objects that we treat as a unit for data changes. Each aggregate has a root that provides the only access point to members of the set and a boundary that defines what's included in the set