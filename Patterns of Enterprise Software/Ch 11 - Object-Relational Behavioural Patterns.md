
### Unit of Work

pg 184

- Unit of Work - an object that keeps track of object created and updates/deletions to existing ones. As soon as you start doing something that may affect a database, you create a Unit of Work to keep track of the changes.
- You can also let it know about objects you've read so that it can check for inconsistent reads by verifying that none of the objects changed on the database during the business transaction.
- When it comes time to commit, the Unit of Work decides what to do. It opens a transaction, does any concurrency checking (using Pessimistic Offline Lock or Optimistic Offline Lock), and writes changes out to the database- application programmers never explicitly call methods for database updates.
- Unit of Work needs to know what objects it should keep track of. You can do this either by the caller doing it or by getting the object to tell the Unit of Work.
- Caller registration - the user of an object has to remember to register the object with the Unit of Work for changes
- Object registration - The onus is removed from the caller
- Unit of work works with any transactional resource, not just databases. Also useful to coordinate with message queue and transaction monitors

- Unit of Work does not track object's we've read and want to check for inconsistent read errors upon commit (Optimistic Offline Lock)

Example use of Unit of Work

```java
class EditAlbumScript {

	public static void updateTitle(Long albumId, String title) {
		UnitOfWork.newCurrent();
		Mapper mapper = MapperRegistry.getMapper(Album.class);
		Album album = (Album) mapper.find(albumId);
		album.setTitle(title);
		UnitOfWork.gtCurrent().commit();
	}
}
```


### Identity Map

pg 196

- A series of maps containing objects that have been pulled from the database
- An explicit Identity Map is accessed with distinct methods for each kind of object, such as `findPerson(1)`
- A generic map uses a single method for all kinds of objects, with perhaps a parameter to indicate which kind of object you need, such as `find("Person", 1)`
- Used to manage any object brought from a database and modified. You don't want a situation where two in-memory objects correspond to a single database record - you might modify the two records inconsistently and thus confuse the database mapping.
- The identity maps also acts as a cache for database reads
- Identity Map helps avoid conflicts within a single session, but it doesn't do anything to handle conflicts that cross session


### Lazy Load

pg 200

- Lazy Load interrupts this loading process for the moment, leaving a marker in the object structure so that if the data is needed it can be loaded only when it is used

#### Methods for implementation
- Lazy initialization - the simplest approach. Uses a null to signal a field hasn't been loaded yet. Does force a dependency between the object and the database and best works for Active Record, Table Data Gateway and Row Data Gateway.
```java
class Supplier...
	public List getProducts() {
		if (products == null) products = Product.findForSupplier(getID());
		return products;
	}
}
```
- Virtual Proxy - An object that looks like the object that should be in the field that doesn't actually contain anything. It looks exactly like the object that's suppose to be there
- Ghost - The real object in a partial state, think every field is lazy-initialized in one fell swoop
- Lazy Load is all about deciding how much you want to pull back from the database as you load an object and how many database calls that will require. It's usually pointless to use Lazy Load on a field that's stored in the same row as the rest of the object, because most of the time it doesn't cost any more to bring back extra data in a call