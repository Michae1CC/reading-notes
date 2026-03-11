
### 4.3 - Transactions

pg 143

- _Transaction_ - Consists of sequence of query/or update statements. The SQL standard specifies that a transaction begins implicitly when a SQL statement is executed
- _Commit Work_ - Commits the current transaction; that is, makes the updates performed by the transaction become permanent in the database.
- _Rollback work_ - Causes the current transaction to be rolled back; that is, it undoes all the updates performed by the SQL statements in the transaction.
- Once a transaction has executed *commit work*, its effects can no longer be undone by *rollback work*
- SQL:1999 standard is to allow multiple SQL statements to be enclosed between the keywords **begin atomic ... end**. All the statements between the keywords then form a single transaction, which is committed by default. However, in several other database databases, such as MySQL and PostgreSQL, support a begin statement, but do not support an end statement; instead the transaction must be ended by either a **commit work** or a **rollback word** command

### 4.4 - Integrity Constraints

pg 145

- _Integrity Constraints_ - Ensures that changes made to the database by authorised users do not result in a loss of data consistency

### 4.7 - Authorization

pg 165

- We may assign a user several forms of authorization on parts of the database, which include:
	- Authorization to read data
	- Authorization to insert new data
	- Authorization to update data
	- Authorization to delete data
- Each type of authorizations is called a privilege