
### 2.1 - Structure of Relational Databases

pg 37

- _Relation_ - Used to refer to a table
- _Tuple_ - Used to refer to a row
- _Attribute_ - Refers to a column
- _Atomic Domain_ - A domain is atomic if elements of the domain are considered to be indivisible
- _Database Instance_ - A snapshot of the data in the database at a given moment
- _Relation Schema_ - Consists of a list of attributes and their corresponding domains
- _Super Key_ - A set of one or more attributes that, taken collectively, allow us to identify uniquely a tuple in the relation
- _Primary Key_ - A candidate key chosen as the principal means of identifying tuples in a relation
- _Foreign Key_ - Attribute/s A of relation r_1 to the primary-key B of relation r_2 states that on any database instance, the value of A for each tuple in r_1 must also be the value of B for some tuple in r_2. The relation r_1 is also called the _Referencing Relation_ of the foreign key constraint, and r_2 is called the _Referenced Relation_ 

### 2.5 - Relational Query Languages

pg 47

- _Query Language_ - A language in which a user requests information from the database
- _Imperative Query Language_ - The user instructs the system to perform a specific sequence of operations on the database
- _Functional Query Language_ - The computation is expressed as the evaluation of functions that may operate on data in the database or on the results of other functions; functions are side-effect free
- _Declarative Query Language_ - The user describes the desired information without giving specific sequence of steps or functions calls for obtaining that information

### 2.6 - The Relation Algebra

pg 48

- _select_ - Selects tuples that satisfy a given predicate
- _project_ - A unary operation that returns its arguments relation, with certain attributes left out
- _arity_ - The number of attributes of a relation