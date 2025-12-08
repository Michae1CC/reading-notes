
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

### Serialized LOB

pg 272