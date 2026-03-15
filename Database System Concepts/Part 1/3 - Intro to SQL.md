
### 3.1 - Overview of SQL

pg 65

- _Data-definition language_ - The SQL that provides commands for defining relation schemas
- _Data-manipulation language_ - The SQL DML provides the ability to query information from the database and to insert tuples into, delete tuple from
- _Integrity_ - SQL DDL commands for specifying integrity constraints that the data stored in the database must satisfy
- _View Definition_ - The SQL DDL includes commands for specifying views
- _Transactional control_ - SQL used to specify beginning and end points of a transaction
- _Embedded SQL_ - Define how SQL statements can be embedded within general-purpose programming languages, such as, C, Java
- _Authorization_ - The SQL DDL includes commands for specifying access rights to relations and views


### 3.6 - Null Values

pg 89

- SQL treats the result as `unknown` for any comparison that involves a `null` value

### 3.7 - Aggregate Functions

pg 91

- _Aggregate Functions_ - Functions that take a collection of values as input and return a single value
- The `distinct` keyword can be used to eliminate duplicates before aggregation
- It is important to ensure that only the attributes that appear in the `select` statement without being aggregated are those present in the group by clause
- `having` clause applies a condition to aggregated groups