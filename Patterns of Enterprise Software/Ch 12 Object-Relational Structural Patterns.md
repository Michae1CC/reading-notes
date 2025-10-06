
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