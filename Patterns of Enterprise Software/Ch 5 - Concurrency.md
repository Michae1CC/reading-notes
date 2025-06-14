
pg 63

- Lost updates - Updates that don't include/don't take into account intermittent modifications
- Inconsistent read - Occurs when you read two things that are correct pieces of information but not correct at the same time
- liveness - How much concurrent activity can go on


### Execution Contexts

pg 65

- request - Corresponds to a single call from the outside world which it optionally sends back a response
- session - A long running interaction between a client and server. It may consist of a single request, but more commonly it consists of a series of requests that the user regards as a consistent logical sequence
- process - is a, usually heavyweight execution context that provides a lot of isolation for the internal data it works
- thread - a lighter-weight active agent that's set up so that multiple threads can operate in a single process which is a good utilization of resources.
- Some frameworks start a new process for each request. People tend to avoid this that now because starting processes tire up a lot of resources, but it's common for systems to have a process handle only one request at a time
- Transaction - Pull together several requests that the client wants treated as if they were a single request


### Isolation and Immutability

pg 67

- To solve the problem of concurrency, there are two main solutions: isolation and immutability.
- Concurrency problems occur when more than one active agent, such as a process or thread has access to the same data.
- Isolation - partition the data so that any piece of data can only be accessed by one active agent
- With isolation you arrange things so that programs enters an isolation zone, within which you don't have to worry about concurrency. Good concurrency design is thus to find ways of creating such zones and to ensure that as much programming as possible is done in one of them


### Optimistic and Pessimistic Concurrency Control

pg 68

- Optimistic locking - Conflict detection
	- Pros: Better progress because the lock is only held during the commit
	- Usually base their conflict detection on some kind of version marker on the data. This can be a timestamp or sequential marker. To detect lost updates the system checks the version marker of your update with the version marker of the shared data. If they're the same, the system allows the update and updates the version marker. Any differences indicate a conflict.
- Pessimistic locking - Conflict prevention
	- Cons: reduces concurrency
- The choice between optimistic and pessimistic locks is the frequency and severity of conflicts. If conflicts are sufficiently rare, or if the consequence are no big deal, you should usually pick optimistic locks because they given you better concurrency and are usually easier to implement
- Controlling access to every bit of data that's read often causes unnecessary problems due to conflicts or waits on data that doesn't actually matter that much
- Temporal reads - These prefix each read of data with some kind of timestamp or immutable label, and the database returns the data as it was according to that time of label. The problem is that the data source needs to provide a full temporal history of changes, which takes time and space to process

### Deadlocks

- Deadlocks - multiple entities acquiring locks preventing the entities to make progress
- When a deadlock occurs, you can pick a victim to throw away work done as well as their lock
- Another deadlock prevention system is to use a timeout - once you hit a limit you loose the lock and become a victim - some people may become victims without a deadlock occurring


### Transactions

pg 71

- general advice is to never make a transaction span multiple requests
- A transaction that spans multiple requests is generally known as a long transaction
- Request transaction - start a transaction at the beginning of the request and complete it at the end
- Lock escalation - if a transaction locks a lot of rows in a table then the database has more locks that it can handle and escalates the locking to the entire table - locking out other transactions

### Reducing Transaction Isolation for Liveness

pg 73

- Serializable transactions - transactions that can be executed concurrently and you can get a result that's the same as you get from some sequence of executing the transactions serially


### Patterns for Offline Concurrency Control

pg 77

- As much as possible, you should let your transaction system deal with concurrency problems
- Handling concurrency control that spans system transactions plonks you firmly in the murky waters
- If you can make all your business transactions fit into a system transaction by ensuring that they fit within a single request, then do that


### Application Server Concurrency

pg 78

- Simplest way to handle concurrency issues is to use one process per session
- The problem with processes-per-session is that it uses up a lot of resources
- To be more efficient you can pool the resources such that each one only handles a single request at one time but can handle multiple requests from different sessions in a sequence
- This approach of pooled process-per-request will use many fewer processes to support a given number of sessions
- With thread-per-request, each request is handled by a single thread within a process. Since threads use much fewer server resources than processes, you can handle more requests with less hardware this way