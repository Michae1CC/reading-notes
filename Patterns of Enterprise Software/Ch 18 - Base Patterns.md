
### Gateway

pg 466

- An object that encapsulates access to an external system or resource
- When accessing external resources, you'll usually get APIs for them. However, these APIs are naturally going to be complicated because they take the nature of the resource into account
- Create a simple API for your usage and use the Gateway to translate to the external source
- Keep a gateway as simple as you can. Focus on the essential roles to adapting the external service and providing a good point for stubbing
- Sometimes a good strategy is to build the Gateway in terms of more than one object, obvious form is to use two objects:
	- back end - acts as a minimal overlay to the external resource and doesn't simplify the resources API at all
	- front end - transforms the awkward API into a more convenient one for your application to use
- you should consider Gateway whenever you have an awkward interface to something that feels external
- Gateway usually makes a system easier to test by giving you a clear point at which to deploy Service Stubs
- Any change to the resource means you only have to alter the Gateway class - the change doesn't have to ripple through the rest of the system
- Facade differences - while a Facade simplifies a more complex API, it's usually done by the writer of the service for general use. A Gateway is written by the client for its particular use.
- Adapter alters an implementation's interface to match another interface you need to work with
- Mediator usually separates multiple objects so that they don't know about each other but do know about the mediator


### Mapper

pg 473

- An object that sets up a communication between two independent objects
- Sometimes you need to setup communications between two subsystems that still need to stay ignorant of each other
- A mapper is an insulating layer between subsystems. It controls the details of the communication between them without either subsystem being aware of it
- How a mapper works depends on the kind of layer's it's mapping. The most common case of a mapping layer that we run into is in a Data Mapper
- Essentially a Mapper decouples different parts of the system.
- You should only use mapper when you need to ensure that neither subsystem has a dependency on this interaction. The only time this is really important is when the interaction between the subsystems is particularly complicated and somewhat independent to the main purpose of both subsystems.
- Thus, in enterprise applications we mostly find Mapper used for interactions with a database, as in Data Mapper.

### Layer Supertype

pg 475

- A type that acts as the supertype for all types in its layer
- It's not uncommon for all objects in a layer to have methods you don't want to have duplicated throughout the system. You can move all of this behaviour into a common Layer Supertype
- All you need is a super class for all objects in a layer - for example, a Domain Object superclass for all domain objects in a Domain Model
- Common features, such as storage and handling of Identity Fields, can go there
- Use Layer Supertype when you have common features from all objects in a layer, I often do this because I make a lot of use of common features
- Domain objects can have a common superclass for ID handling

```java
class DomainObject {
	private Long ID;
	public Long getID() {
		return ID;
	}
	public void setID(Long ID) {
		Assert.notNull("Cannot set a null ID", ID);
		this.ID = ID;
	}
	public DomainObject(Long ID) {
		this.ID = ID;
	}
	public DomainObject() {}
}
```