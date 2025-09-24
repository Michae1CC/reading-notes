
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