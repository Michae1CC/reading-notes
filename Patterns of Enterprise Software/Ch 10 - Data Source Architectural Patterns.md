
### Table Data Gateway

pg 144

- Mixing SQL in application logic can cause several problems. Many devs aren't comfortable with SQL, and many who comfortable may not write it well.
- A _Table Data Gateway_ holds all the SQL for accessing a single table or view: selects, inserts, updates, and deletes. Other code calls its methods for all interaction with the database

- A _Table Data Gateway_ has a simple interface, usually consisting of several find methods to get data from the database and update, insert, and delete methods.
- Each methods maps the input parameters into a SQL call and executes the SQL against a database connection.
- Is usually stateless, as its role is to push data back and forth

- Well suited for Transaction Scripts

pg 150
- Abstracts away how the data is stored, could come from a database or an in-memory form of storage

### Row Data Gateway

pg 152

- An object that acts as a Gateway to a single record of data
- Embedding database access code in in-mem object can have disadvantages eg. if your in mem objects have business logic of their own, adding db manipulation code increases complexity. Testing is also awkward
- The Row Data GW will usually do any type conversion from the data source types to the in-memory types, but this conversion is pretty simple
- It often makes sense to have separate finder objects so that each table in a relational database will have one finder class and one gateway class for the results
- Active Records hold domain logic, Row Data Gateway should contain only access logic and no domain logic

- Most often used when using a Transaction Script

### Active Record

- Is responsible for saving and loading to the database and also for any domain logic that acts on the data. This may be all the domain logic in the application, or you may find that some domain logic is held in Transaction Scripts with common data-oriented code in the Active Record
- Generally has the following methods:
	- Construct an instance of the Active Record from a SQL result set row
	- Construct a new instance for later insertion into the table
	- Static finder methods to wrap commonly use SQL queries and return Active Record objects
	- Update the database and insert into it the data in the Active Record
	- Get and set fields
	- Implement some pieces of business logic
- Row Data Gateway contains only database access while and Active Record contains both data source and domain logic
- You can separate out the find methods into a separate class (better for testing)
- A Good choice when the domain logic isn't too complicated


### Data Mapper

pg 165

- Objects and relational databases have different mechanisms for structuring data. Many parts of an object, such as collections and inheritance, aren't present in relational databases.
- The Data Mapper is a layer of software that separates the in-memory objects from the database. Its responsibility is to transfer data between the two and also to isolate them from each other.
- Data Mapper itself is even unknown to the domain layer

pg 166

- When it comes to inserts and updates, the database mapping needs to understand what objects have been changed, which new ones have been created, and which ones have been destroyed. It also has to fit the whole workload into a transactional framework.

pg 169

- Handling Finders - In order to work with an object, you have to load it from the database. Usually the presentation layer will initiate things by loading some initial objects

#### Mapping Data to Domain Fields

- When you create an object you two two options:
	- Create a rich constructor so that it's at least created with all the mandatory data
	- Create an empty object and then populate it with the mandatory data
- Former is usually preferred since it's nice to have well-formed object from the start
- The problem with a rich constructor is that you have to be aware of cyclic references

- The primary occasion for using a Data Mapper is when you want the database schema and the object to evolve independently. The most common case for this is with a Domain Model.