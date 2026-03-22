
### 6.1 - Overview of the Design Process

pg 241

- _Conceptual Design phase_ - provides a detailed overview of the enterprise
- _Specification of functional requirements_ - users describe the kinds of operations that will be performed on the data.
- _Logical design phase_ - the designer maps the high-level conceptual schema onto the implementation data model is typically the relational data model
- _Physical design phase_ - physical features of the database are specified
- In designing a database schema, we must ensure that we avoid two major pitfalls:
	- Redundancy - Repeating information. The biggest problem is that storing redundant information may cause inconsistency
	- Incompleteness - A bad design may make certain aspects of the enterprise difficult or impossible to model

### 6.2 - The ER Model

pg 244

- _ER data model_ was developed to facilitate database design by allowing specification of an enterprise schema that represents the overall logical structure of a database
- _ER diagram_ can express the overall logical structure of a database graphically
- An _entity_ is a "thing" or "object" in the real world that is distinguished from all other objects, for example a person in a university
- An _entity set_ is a set of entities of the same type that share the same properties, for example all the people who are instructors at a university
- An entity is represented by a set of _attributes_. Attributes are descriptive properties possessed by each member of an entity set.
- Each entity has a _value_ for each of its attributes.
- A _relationship_ is an association among several entities
- A _relationship set_ is a set of relationships of the same type
- The function that an entity plays in a relationship is called a entity's _role_

### 6.3 - Complex Attributes

pg 249

- For each attribute, there is a set of permitted values, called the _domain_, or _value set_
- _Simple Attributes_ - can't be divided into subparts
- _Composite Attributes_ can be divided into subparts