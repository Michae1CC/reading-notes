pg 33

- It would be better to access data using mechanisms that fit in with the application development language. 
- It's a good idea to separate SQL access from domain logic and place it in separate classes. A good way of organizing these classes is to base them on table structure of the database so that you have one class per database table. These classes form a Gateway to the table
- In simple applications the Domain Model is an uncomplicated structure that actually corresponds pretty closely to the database structure, with one domain class per database table.
- As you start moving toward a rich domain model, the simple approach of an Active Record starts to break down. The one-to-one match of domain classes to tables start to fail as you factor domain logic into smaller classes.
pg 36
- A better route is to isolate the Domain Model from the database completely, by making your indirection layer entirely responsible for the mapping between domain objects and the database tables. This Data Mapper handles all of the loading and storing between the database and the Domain Model and allows both to vary independently. The most complicated of the db mapping architectures, but its benefit is complete isolation of the two layers

### The Behavioral Problem

pg 39

- As you read objects and modify them, you have to ensure that the database state you're working with stays consistent. If you read some object's, it's important to ensure that the reading is isolated so that no other process changes any of the objects you've read while you're working on them. Otherwise, you could have inconsistent and invalid data in you're objects
- Unit of work acts as the controller of the database mapping. Without the Unit of Work, typically the domain layer acts as the controller; deciding when to read and write to the database.
- As you load in objects, you have to be wary about loading the same one twice


### Reading in Data

pg 40

- Methods as finders wrap SQL select statements with a method-structured interface (eg `findForCustomer(customer)`)