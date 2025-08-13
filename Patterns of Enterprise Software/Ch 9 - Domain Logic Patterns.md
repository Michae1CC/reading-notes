
pg 110

- Most business applications can be thought of as a series of transactions
- A Transaction Script organizes all this logic primarily as a single procedure, making calls directly to the database or through a thin database wrapper
- You can organize your Transaction Scripts into classes in two ways. The most common is to have several Transactions Scripts in a single class, where each class defines a subject area. The other way is to have each Transaction Script in its own class, using the Command Pattern
- One particular problem to watch for is its duplication between transactions. Since the whole point is to handle one transaction, any common code tends to be duplicated. Careful refactoring can alleviate many of these problems, but more complex business domains need to build a Domain Model.

pg 133

- The service layer defines a an applications boundary with a layer of services that establishes a set of available operations and coordinates the applications response in each operation.
- Enterprise applications typically require different kinds of interfaces to the data they store and the logic they implement: data loaders, user interfaces, integration gateways, and others.

Layers
- Service Layer
- Domain Model
- Data Source Layer

- Business logic can be divided into two kinds: "domain logic" and "business logic" having to do purely with the problem domain and "application logic" having to do with application responsibilities
- "Application Logic" sometimes referred to as "workflow logic"
- Implementation Variations:
	- In the domain facade approach a _Service Layer_ is implemented as a set of this facades over a Domain model. The classes implementing the facades don't implement and business logic. Rather, the _Domain Model_ implements all of the business logic. The thin facades establish a boundary and set of operations through which client layers interact with the application, exhibiting the defining characteristics of Service Layer.
	- In the operations script approach a Service Layer is implemented as a set of thicker classes tat directly implement application logic but delegate to encapsulated domain domain object classes for domain logic.
- The interface of a service layer class is course grained by definition, since it declares a set of operations applications operations available to interfacing client layers. Therefore Service Layer classes are well suited to remote invocation from an interface granularity perspective.
- Operations needed by the Service Layer are identified by the needs of the Service Layers clients - typically a user interface.

pg 137

- The benefit of the Service Layer is that it defines a common set of application operations available to many kinds of clients and it coordinates and application's response in each operation
- You probably don't need a service layer if your applications business logic will only have one kind of client - say a user interface - and its use case responses don't involve multiple transactional resources.