
### Metadata Mapping

pg 306

- Allows developers to define the mappings in a simple tabular form, which can be processed by generic code to carry out the details of reading, inserting and updating the data
- Two main routes to take
	- Code gen - you write a program whose input is the metadata and whose output is the source code of classes that do the mapping
	- Reflective program - May ask an object for a method named setName and then run an invoke method as data the reflective program can read in field and method names from a metadata file and use them to carry out the mapping


### Query Object

pg 316

- A Query Object is an interpreter, that is, a structure of objects that can form itself into a SQL query
- A Query object is an application of the Interpreter pattern geared to represent a SQL query
- Allows a client to form queries of various kinds and to turn those object structures into the appropriate SQL string
- You only really need them when you're using Domain Model and Data Mapper - you also really need Metadata Mapping to make serious use of them