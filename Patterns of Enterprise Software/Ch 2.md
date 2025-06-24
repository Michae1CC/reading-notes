
## Ch 2 - Organizing Domain Logic

pg 25

- Three primary patterns for organizing domain logic: *Transaction Script*, *Domain Model* and *Table Module*

- A Transaction Script is essentially a procedure that takes the input from the presentation, processes it with validations and calculations, stores data in the database, and invokes any operations from other systems. It then replies with more data to the presentation, perhaps doing more calculation to help organize format the reply. 
- Advantages include
	- It's a simple procedure model that most dev understand
	- Works well with a simple data source
	- It's obvious how to set the transaction boundaries
- Disadvantages include
	- There will be duplicated code as several transactions need to do similar things (some of this can be dealt with by factoring out common subroutines)

pg 26
- A Domain Model we build a model of our domain which, at least on a first approximation, if organized primarily around the nouns in the domain. The logic for handling validations and calculations would be placed into this domain model.
- The value of the Domain Model lies in the fact that once you've gotten used to things, there are many techniques that allow you to handle increasingly complex logic in a well-organized way.

pg 28
- A Table Model is in many ways a middle ground between a Transaction Script and a Domain Model. Organizing the domain logic around tables rather than straight procedures provides more structure and makes it easier to find and remove duplication

### Service Layer

pg 30

- A common approach in handling domain logic is to spilt the domain layer in two. A service layer is placed over an underlying Domain Model or Table Model
- The presentation logic interacts with the domain purely through the Service Layer which acts like an API for the application
