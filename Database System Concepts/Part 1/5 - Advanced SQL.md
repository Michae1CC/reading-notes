

### 5.0 - Accessing SQL from a Programming Language

pg 183

- Not all queries can be expressed in SQL, since SQL does not provide the full expressive power of a general purpose language
- Two approaches to access SQL from a general-purpose programming language:
	- _Dynamic SQL_ A general-purpose program can connect to and communicate with a database server using a collection of functions.
	- _Embedded SQL_ Like dynamic SQL, embedded SQL provides a means by which a program can interact with a database server.

### 5.1 - Accessing SQL

pg 194

- _Open Database Connectivity_ The standard which defines an API that applications can use to connect to a database
- SQL standard defines an embedding of SQL in a variety of programming languages
- A language in which SQL queries are embedded is referred to as a host language, and the structures permitted in the host constitute embedded SQL

### 5.3

pg 206

- _Trigger_ a statement that the system executes automatically as a side effect of a modification to the database
- To define a trigger we must
	- Specify when a trigger must be executed. This is broken up into an event that causes the trigger to be checked and a condition that must be satisfied for trigger execution to proceed
	- Specify the actions to be taken when the trigger executes
- A problem with triggers is that they may be executed when a replicate is promoted to primary
- Triggers should be written with great care, since trigger error detected at runtime causes failure of the action statement that set off the trigger