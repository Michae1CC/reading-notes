
## Identity Field

pg 216
- Databases tell one row from another using a key, in-memory object don't require a key. In order to write data back you need to tie the database to the in-memory object system.
- Identity Field - Store the primary key of the relational database table in the object's fields
- Choosing your key
	- First concern is whether to use a meaningful key or not (e.g. social security number). The danger with a meaningful key is that, while in theory they make good keys, in practice they don't. To work at all, they need to be immutable, to work well, they need to be immutable.
	- Simple vs compound keys. A simple key only uses one database field; a compound key uses more than one. If you use simple keys everywhere you can use the same code for all key manipulation. Compound keys require special handling in concrete classes.
- For the key type, make sure equality checks are fast. Other important operation is getting the next key.
- table-unique key - is unique across every row in every table in the database
- database-unique key - is unique across every row in every table in the database
pg 220
- Getting a New Key - Three basic choices: database auto-gen, GUID, generate your own
	- Auto-gen - most easy and simple but can cause problems for object relational mapping
	- GUID - disadvantage is that equality checks are slow
- Use Identity Field when there's a mapping between objects in memory and rows in the database.
#### When to use it

pg 221
- For a small object with value semantics, such as a money of date range object that won't have it's own value, it better to use Embedded Value
- For complex graph of objects that doesn't need to be queried within the relational database, Serialized LOB is usually easier to write and gives faster performance.

- Identity Field - An integral field in the database that maps to an integral field in an in-memory object
- If your database supports a database counter and you're not worried about being dependent on database-specific SQL, you should use the counter. Even if you're worried about being dependent on a database you should still consider it - as long as your key generation code is nicely encapsulated, you can always change it to a portable algorithm later.


### Foreign Key Mapping

pg 236

- If two objects are linked together with an association, this association can be replaced by a foreign key in the database.
- A more complicated case turns up when you have a collection of objects. You can't save a collection in the database, so you have to reverse the direction of the reference.


### Association Table Mapping

pg 248

- Objects can handle multivalued fields quite easily by using collections as field values. Relational databases don't have this feature and are constrained to single-valued fields only
- The basic idea behind Association Table Mapping is using a link table to store the association. This table has foreign key IDs for the two tables that are linked together, it has one row for each pair of associated objects.
- Association Table Mapping is a many-to-many association, since there are really no other alternatives for that situation
- Sometimes you may need to link two existing tables, but you aren't able to add columns to those tables. In this case you can make a new table and use Association Table Mapping

```java
class Employee {
	public IList Skills {
	get {return ArrayList.ReadOnly(skillsData);}
	set {skillsData = new ArrayList(value);}
	}
	public void AddSkill (Skill arg) {
	skillsData.Add(arg);
	}
	public void RemoveSkill (Skill arg) {
	skillsData.Remove(arg);
	}
	private IList skillsData = new ArrayList();
}
```

- To load an employee from the database, we need to pull in the skills using an employee mapper
- Each employee mapper class has a find method that creates an employee object. All mappers are subclasses of the abstract mapper class that pulls together common services for the mappers


### Dependent Mapping

pg 262

- The basic idea behind Dependent Mapping is that one class (the dependent) relies upon some other class (the owner) for its database persistence. Each dependent can have only one owner and must have one owner.

### Embedded Value

pg 268

- An Embedded Value maps the values of an object to fields in the record of the object's owner
- For example, if an employment object the result fields in those objects map to fields in the employment table rather than make new records
- The simplest cases for Embedded Value are clear, simple Value Objects like money and date range. Since Value Objects don't have identity, you can create and destroy them easily without worrying about such things as Identity Maps to keep them all in sync. Indeed, all Value Objects should be persisted as Embedded Value, since you never want a table for them there.
- The grey area is in whether it's worth storing reference objects, such as an order and a shipping object, using Embedded Value. The principal question here is whether the shipping data has any relevance outside the context of the order. One issue is the loading and saving. If you only load the shipping data into mem when you load the order, that's an argument for saving both in the same table

### Serialized LOB

pg 272

- Saves a graph of objects by serializing them into a single large object, which stores in a database field.
- There are two way to do serialization, as a binary BLOB or as textual characters CLOB
- BLOB
	- Advantages - Uses a small amount of space
	- Disadvantages - Your database must support a binary data type and you can't reconstruct the graph without the object.
- CLOB
	- Serialize information into a text string that carries all the information you need
	- Disadvantages - Requires more space. Duplication issues.
- Works best when you can chop out a piece of the object model and use it to represent the LOB. Think of a LOB as a way to take a bunch of objects that aren't likely to be queried from any SQL route outside the application.

### Single Table Inheritance

pg 278

- When mapping to a relation database, we try to minimize the joins that can quickly mount up when processing an inheritance structure in multiple tables
- Single table inheritance maps all fields of all classes of an inheritance structure into a single table
- Strengths
	- There's only a single table to worry about on the database
	- There are no joins in retrieving data
	- Any refactoring that pushes fields up or down the hierarchy doesn't require you to change the database
- Weaknesses
	- Fields are sometimes relevant and sometimes not, which can be confusing to people using the table directly
	- The single table may end up being too large, with many indexes and frequent locking, which may hurt performance
	- You only have a single namespace for fields, so you have to be sure that you don't use the same name for different fields. Compound names with the name of the class as a prefix of suffix helps here
- You don't need to use one form of inheritance mapping for your whole hierarchy. It's perfectly fine to map half a dozen similar classes in a single table, as long as use Concrete Table Inheritance for any classes that have a lot of specific data

### Class Table Inheritance

pg 285

- Class Table Inheritance support database structures that map clearly to the objects and allow links anywhere in the inheritance structure by using on database table per class in the inheritance structure
- Has one table per class in the domain model - the fields in the domain class map directly to fields in the corresponding tables
- A common primary key is used to link is used to link corresponding rows of the database tables
- Strengths
	- All columns are relevant for every row so tables are easier to understand and don't waste space
	- The relationship between the domain model and database is very straight forward
- Weaknesses
	- You need to touch multiple tables to load the object, which means a join or multiple queries and sewing memory
	- Any refactoring of fields up or down the hierarchy causes database changes
	- The supertype tables may become a bottleneck because they have to be accessed frequently
	- The high normalization may make it hard to understand for ad queries

### Concrete Table Inheritance

pg 293

- Represents an inheritance hierarchy of classes with one table per concrete class in the hierarchy
- Uses one db table for each concrete class in the hierarchy
- Strengths
	- Each table is self-contained and has no irrelevant fields. As a result is makes good sense when used by other applications that aren't using the objects
	- There are no joins to do when reading the data from the concrete mappers
	- Each table is accessed only when that class is accessed, which can spread the access load
- Weaknesses
	- Primary keys can be difficult to handle
	- You can't enforce database relationships to abstract classes
	- If fields on the domain classes are pushed up or down the hierarchy you have to alter the table definitions
	- If a super class field changes, you need to change each table that has this field because the superclass field are duplicated across the tables


### Inheritance Mappers

pg 302

- A structure to organize db mappers that handle inheritance hierarchies
	- Disadvantages - Your database must support a binary data type and you can't reconstruct the graph without the object. Another problem is versioning, if you change the department class, you may not be able to read all its previous serializations; since data can live in the database for a long time
