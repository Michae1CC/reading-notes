
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


### Separated Interface

pg 476

- Defines an interface in a separate package from its implementation
- As you develop a system, you can improve the quality of its design by reducing the coupling between the system's parts - a good way to do this is to group the classes into packages and control the dependencies between them
- Use a separated interface to define in one package but implement it in another - this way a client that needs the dependency to the interface can be completely unaware of the implementation
- One awkward thing about separate interfaces is how to instantiate the implementation. I usually requires knowledge of the implementation class. The common approach is to use a separate factory object, where again there is a Separated Interface for the factory
- Use separated interface when you need to break a dependency between two parts of the system
	- You've built some abstract code for common cases into a framework package that needs to call some particular application code
	- You have some code in one layer that needs to call code in another layer that it shouldn't see, such as domain code calling a Data Mapper
	- You need to call functions developed by another development group but don't want a dependency into their APIs

### Registry

pg 480

- A well-known object that other objects can use to find common objects and services
- A registry is essentially a global object, or at least it looks like one - even if it isn't as global as it may appear
- You can have different Registries for different scopes, but you can also have a single Registry in which different methods are at different scopes.
- Singletons are widely used in single-threaded apps, but can be a problem for multi-threaded apps. This is because it's too easy for multiple threads to manipulate the same object in unpredictable ways
- A common kind of registry is thread scoped

### Value Object

pg 486

- A small simple object, like money or a date range, whose equality isn't based on identity
- Value Objects are small objects, such as a money object or a date, while reference objects are large, such as a an order or a customer. The key difference is how they deal with equality. A reference object uses identity as the basis for equality - maybe the identity within the programming system, such as the built-in identity of OO programming languages, or maybe some kind of ID number
- Treat something as a Value Object when you're basing equality on something other than identity. It's worth considering this for any small object that's easy to create.

### Money

pg 488

- Represents a monetary value
- You can store the amount as either an integral type or a fixed decimal type. Decimal type is easier for some manipulations, the integral for others
- You should avoid any kind of floating point type, as that will introduce the kind of rounding problems that Money is intended to avoid
- Used for pretty much all numeric calculation in object-oriented environments

### Special Case

pg 496

- A subclass that provides special behaviour for particular cases.
- The basic idea is to create a subclass to handle the Special Case
- IEEE 754 floating-point arthimetic offers good examples of Special Case with positive infinity, negative infinity and not-a-number
- Use whenever you have multiple places in the system that have the same behaviour after a conditional check for a particular class instance or the same behaviour after a null check


### Plugin

pg 499

- Links classes during configuration rather than compilation
- Configuration shouldn't be scattered throughout your application, nor should it require a rebuild or redeployment - plugin solves both problems by providing centralized, runtime configuration
- The first thing to do is define with a Separated Interface any behaviours that will have different implementations based on runtime env. Beyond that we use the basic factory pattern, only with a few special requirements
- The plugin factory requires its linking instructions to be stated at a single, external point in order that configuration can be easily managed
- Additionally, the linking to implementations must occur dynamically at runtime rather than during compilation, so that reconfiguration won't require a rebuild.
- A text file works quite well as the means of stating linking rules. The Plugin factory will simply read the text file, look for an entry specifying the implementation of a requested interface, and return that implementation
- Use a plugin whenever you have behaviours that require different implementations based on runtime environment.

### Record Set

pg 508

- In memory representation of tabular data
- The idea of a Record Set is to provide an in-memory structure that looks exactly like the result of an SQL query but can be generated and manipulated by the other parts of the system
- Generally provided by the vendor of the software platform you're working with
- The first essential element is that it looks exactly like the result of a database query
- Most Record Set implementations use an implicit interface - this means that to get information out of a Record Set you invoke a generic method with an argument to indicate which field you want
- An explicit interface requires a real reservation class with define methods and properties
- Implicit interfaces are flexible in that you can use a generic Record Set for any kind of data. This saves you having to write a new class every time you define a new kind of Record Set
- The value of Record Set comes from having an environment that relies on it as a common way of manipulating data