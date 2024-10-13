
- In an OO program, UI, db and other support code often gets written directly into the business objects. Additionally business logic is embedded in the behaviour of UI widgets and db scripts. This happens because it is the easiest way to make things work, in the short term.
- When the domain related code is diffused through this large amount of other code, it becomes extremely difficult to see and reason about. Superficial changes changes to the UI can actually change business logic. Changing business rules may require meticulous tracing of UI code, db code or other program elements. Implementing coherent model-driven objects becomes impractical. Automated testing is awkward. With all those technologies and logic involved in each activity, a program must be kept very simple or it becomes impossible to understand. - pg 52
Week of 19/08
- We need to be able to pick out elements of our model and see them as a system, not to be distracted by the process of picking them out of a much larger mix of objects, like identifying constellations lost in the sky. This requires decoupling the domain objects from other functions of the system so they are not lost in the mass and so the domain concepts are not blurred and confused with concepts related to software tech.
- The essential principle of layering is that an element of a layer has dependencies only on other elements of the layers "beneath" it. Communication upwards must be through some indirect mechanism - pg 53
	- User Interface / Presentation Layer - Used for showing and interpreting user's commands.
	- App Layer - Defines the jobs software is suppose to do and directs the expressive domain objects to work out problems. The tasks this layer is responsible for are meaningful to the business or necessary for interaction with the app layers of other systems. It does not contain business rules or knowledge but only coordinates tasks and delegates work to collaborations of domain objects in the next layer down. It does not have state reflecting the business situation, but it can have state that reflects the progress of a task for the user or the program.
	- Domain Layer - Responsible for representing concepts of the business, information about the business situation and business rules. State that reflects the business situation is controlled and used here, even though the technical details of storing it are delegated to the infra
	- Infra Layer - Provide generic technical capabilities that support the higher layers: message sending for the application, persistence of the domain, drawing widgets for the UI, etc. The infra layer may also support the pattern of interactions between the four layers through an architectural framework.
- Partition a complex program into layers, develop a design within each layer that is cohesive and is dependent only on the layers below. Follow standard architectural patterns to provide loose coupling to the layers above. Concentration the all the code related to the domain model in one layer and isolate it from the user interface, app, and infra code. This allows a model to evolve rich enough and clear enough to effectively capture essential business knowledge and put it to work.

#### Questions

- What do these layers look like in the context of a webapp? Should the front-end and back-end be treated as separate systems?
- What was generally considered infra at the time? Does infra have a split responsibility of also handling domain level logic (AWS event bridge conditions, AWS stepfunctions) 

#### Relating Layers
- Layers are meant to be loosely coupled, with design dependencies in only one direction.
- When an object of a lower level needs to communicate upward (beyond answering a direct query) we need another mechanism, drawing on architectural patterns for relating layers such as callbacks or observers. - pg 55

#### A model expressed in software
- Aspects of the domain that are more clearly expressed as actions/operations are often best expressed as services. A service is something that will be done for a client on request - pg 60

#### Associations
-  There are at least three ways of making associations more tractable
	- Imposition of direction
	- Addition of qualifier
	- Elimination of non-essential associations
- Eliminate associations if they are not required
- Some objects are not defined primarily by their attributes. They represent a thread of identity that runs through time and often across distinct representations. Sometimes an objects must be matched with another object event though attributes differ. An object must be distinguished from other objects event though they might have the same attributes. Mistaken identity can lead to data corruption. Objects defined primarily by its identity is called an 'entity'
- Their class definitions, responsibilities, attributes and associations should revolve around who they are rather than the particular attributes they carry.

#### Modeling Entities
- Entities are defined by their identities. Attributes are attached and changed. Therefore strip the entity object's definition down to the most intrinsic characteristics, particularly those that identify it, or are commonly used to find or match it. Separate other characteristic into other objects associated with the core entity.
- Entities are defined by their identities. Attributes are attached and change. Therefore, strip the Entity object's definition down to the most intrinsic characteristics, particularly those that the identify or are commonly used to find or match it. Separate other characteristics into other objects associated with the core Entity.
- Each entity must have an operational way of establishing its identity with another object; distinguishable even from another object with the same descriptive attributes.
- When there is no true unique key made of the attributes of an object, the most common solution is to attach to each instance a symbol that is unique within the class. Once the symbol is obtained and stored as an attribute of the entity, it is designated immutable

- Since the most conspicuous in a model are usually entities, it is important to track their identity
- IMPORTANT: Tracking the identity of entities is essential, but attaching identity to other objects can hurt system performance, add analytical work, and muddle the model by making all objects look the same. Software design is a constant battle with complexity. We must make distinctions so that any special handling is applied only where necessary. However, if we think of this category of object as just the absence of identity, we haven't added much to our toolbox or vocabulary. In fact, these objects have characteristics of their own, and their own significance to the model. These are the objects that describe the nature of things. pg - 70

#### Value Objects
- An object that represents a descriptive aspect of the domain that has no conceptual identity is called a _value object_. Value objects are instantiated to represent elements of the design that we care about only for what they are, not who they are.
- Colours are are an example of value objects
- Value Objects can reference entities
- Value Objects are often passed are parameters in messages between other objects. They are frequently transient, created for an operation and then discarded
- IMPORTANT: When you care only about the attributes of an element of the model, classify it as a VALUE OBJECT. Making it express the meaning of attributes it conveys and give it related functionality. Treat the Value Object as immutable. Don't give it any identity and avoid the design complexities necessary to maintain entities - pg 71
- Value objects tend to be numerous. If a value object are considered interchangeable, the a group of value objects can be replaced with a single with a reference to an immutable value object this could be a good improvement for optimization.
- The economy of copying versus sharing depends on the implementation environment. While copies may clog up the system with huge numbers of items, sharing can slow a distributed system. When a copy is passed between two machines, one message is sent and a copy is live independently on the receiving machine. But if a single instance is being shared, only a reference is passed, requiring a message back to the object for any interaction.
- Sharing is best restricted to cases where it is most valuable and least troublesome (pg 72):
	- When saving space or object count in the database is critical
	- When communication overhead is low (central server)
	- When the shared object is strictly immutable
- Cases that favor allowing value objects to be mutable
	- If the value is frequently changed
	- If the object creation or deletion is expensive
	- If replacement (rather than modification) will disturb clustering
	- If there is not much sharing of values, or if such sharing is forgone to improve clustering or for some other reason
- If a value's implementation is to be mutable, then it _must not_ be shared
- bi-directional associations with value objects are meaningless

### Services

- Typical mistake is to give up on fitting the behaviour into an appropriate object, gradually slipping towards procedural programming. But when we force an operation into an object that doesn't fit the object's definition, the object loses its conceptual clarity and becomes hard to understand or refactor. These operations often draw together too many domain objects, coordinating them and putting them into action.
- IMPORTANT: Some concepts from the domain aren't natural to model as objects. Forcing them required domain functionality to be assigned as a responsibility of an Entity or Value either distorts the definition of model based object or adds meaningless artificial objects.
- A _Service_ is an operation offered as an interface that stands alone in the model, without encapsulating state as Entities and Value Objects do.
- It is purely defined in terms of what it can do for the client.
- Tends to be named for an activity, rather than an entity (a verb rather than a noun)
- Should not strip the Entities and Value Objects of all their behaviour
- A good service has three characteristics:
	- The operation relates to a domain concept that is not a natural part of an Entity or Value Object
	- The interface is defined in terms of other elements of the domain model
	- The operation is stateless (any client can use any instance of the Service without regard to the instance's history)
- When a significant process or transformation in the domain is not a natural responsibility of an Entity or Value Object, add an operation to the model as a standalone interface declared as a Service. Define the interface in terms of the language of the model and make sure the operation name is part of the Ubiquitous Language. Make the Service stateless. - pg 76
- Many Services are built on top of populations of Entities and Values behaving like scripts that organize the potential of the domain to actually get something done.
- It usually awkward to make a direct interface between a domain object and external resources.
- Medium-grain stateless services can be easier to reuse in large systems because they encapsulate significant functionality behind a simple interface. Also, fine-grained objects lead lead to inefficient messaging in a distributed system. pg 77
- Fine grained domain objects can contribute to knowledge leaks from the domain into the application layer where the domain object's behaviour is coordinated

##### Questions
- How are services uses implemented (refers to a single class (or even a single function) or an entire application?). One example was having a service that controls a transfer between two bank accounts (a domain level service). Another example was send transfer confirmations via email, where external services are 'dressed up' using the facade design pattern to take inputs in terms of domain model.
- "Fine grained domain objects can contribute to knowledge leaks from the domain into the application layer", what do they mean by that?
- Would security be treated separately, to the domain model? What about something like a datamapper version or some sort encryption key that is associated with a business (perhaps used to sign responses)? Should it be considered its own layer?
- If I have functionality that involves many Entities, who/how should that be handled?


### Modules

- **Coupling** is the degree of interdependence between software modules; a measure of how closely connected two routines or modules; the strength of the relationships between modules.
- Cohesion refers to the degree to which the elements inside a module belong together. In one sense, it is a measure of the strength of relationship between the methods and data of a class and some unifying purpose or concept served by that class. In another sense, it is a measure of the strength of relationship between the class's methods and data.
- IMPORTANT: Everyone uses Modules, but few treat them as a full-fledged part of the model. Code gets broken down by all sorts of categories, from aspects of the technical architecture to dev work assignments. Even devs who refactor a lot, tend to content themselves with Modules conceived early in the project. It is a truism that there should be low coupling between modules and high cohesion within them. Explanations of the coupling between modules and cohesion within them. Explanations of coupling and cohesion tend to make them sound like technical metrics, to be judged mechanically based on the distributions of associations and interactions. Yet it isn't just code being divided into modules, but concepts. There is a limit to how many things a person can think about at once (hence low coupling). Incoherent fragments of ideas are even harder to understand than an undifferentiated soup of ideas (hence high cohesion). pg 79
- Whenever two model elements are separated into different modules, the relationship between them becomes less direct than they were, which increases the overhead of understanding their place in the design. Low coupling between modules minimizes this cost and makes it possible to analyze the contents of one module with a minimum of reference to others that interact.
- Well-chosen Modules bring together elements of a model with particularly rich conceptual relationships. pg 79
- IMPORTANT: When choosing Modules, focus on conceptual cohesion and telling the story of the system. If this results in tight coupling between Modules, look to see if an overlooked concept would bring the elements together in a coherent Module, or if a change in model concepts would disentangle them. Seek low coupling in the sense of concepts that can be understood and reasoned about independently of each other. The module should reflect insight into the domain. Refine the model until the concepts partition according to the high-level domain concepts and corresponding code is decoupled as well. Give the Module names that become part of the Ubiquitous Language.
- Look for ways of minimizing the work of refactoring Modules. pg 80
- Choose a minimum of technical rules that are essential to technical environments or actually aid development
- Unless there is a real intention to distribute code on different servers, keep all the code that implements a single conceptual object in the same Module, if not the same object. pg 82
- IMPORTANT: Use packaging to separate the domain layer from other code. Otherwise, leave as much freedom as possible to the domain developers to package the domain objects in ways that support their model and design choices.
- Modules need to be refactored along with the the model and code.
- The cost of elaborate techincally driven packaging schemes are:
- If the framework partitioning conventions pull apart the elements implementing the conceptual objects, the code no longer reveals the model.
- There is only so much partitioning a mind can stitch back together, and if the framework uses it all up, the domain developers lose their ability to chunk the model into meaningful pieces.
- IMPORTANT: Unless there is a real intention to distribute code on different servers, keep all the code that implements a single conceptual object in the same Module, if not the same object. pg 82
- IMPORTANT: Use packaging to separate the domain layer from other code. Otherwise, leave as much freedom as possible to the domain developers to package the domain objects in ways that support their model and design choices.
- A mixture of paradigms allows developers to model particular concepts with the style that best that best fits pg 86
- When the domain model is bridging two paradigms, it is crucial to keep it cohesive showing the relationships between the rules and the objects pg 86
- Rules of thumb for mixing non-objects elements into a predominately OO system pg 87 :
	- Don't fight the implementation paradigm. When modeling, find concepts that fit the paradigm that they will be implemented in
	- Lean on the Ubiquitous Language. Even when there is no rigorous connection between tools, very consistent use of language can keep parts of the design from diverging
	- Be skeptical. Is the tool really pulling its weight? Just because you have some rules, that doesn't necessarily mean you need the overhead of a rules engine. Rules can be expressed as objects if a little less neat, and multiple paradigms complicate matters enormously. 

#### Questions

- What are modules in the language of most languages, just files?

## The Lifecycle of a Domain Object


- Managing these persistent objects presents challenges that can easily derail an attempt at Model-Driven Design, these problems fall into two main camps:
	- Maintaining integrity throughout the lifecycle
	- Preventing the model from getting swamped by the complexity of managing the lifecycle
- Threes patterns will address these issues:
	- First Aggregates tighten up the model itself by defining clear ownership and boundaries, avoiding a chaotic tangled web of objects. This is crucial to maintaining integrity in all phases of the lifecycle.
	- Then we focus on the beginning of the lifecycle, using Factories to create and reconstitute complex objects objects and Aggregates, keeping their internal structure encapsulated
	- Finally Repositories address the middle and end of the life-cycle, providing the means of finding and retrieving persistent objects while encapsulating the immense infrastructure involved.
- With many users consulting and updating different objects in the system we have to prevent simultaneous changes to interdependent objects
- !##############
- IMPORTANT: It is difficult to guarantee the consistency of changes to objects in a model with complex associations. Invariants need to be maintained that apply to closely related groups, not just discrete objects. Yet cautious locking schemes cause multiple users to interfere pointlessly with each other and make a system unusable. pg 89
- An Aggregate is a cluster of associated objects that we treat as a unit for the purposes of data changes. Each Aggregate has a root and boundary. The boundary defines what is inside the Aggregate. The root is a specific Entity contained in the Aggregate. The root is the only member of the aggregate that outside objects are allowed to hold references to, although objects within the boundary may hold references to each other.
- Entities other than the root have local identity, but it only needs to be unique within the aggregate. pg 90
- A deletion operation must remove everything in the entity boundary at once.
- When a change to any object within the Aggregate boundary is committed, all invariants of the whole Aggregate must be satisfied.
- IMPORTANT: Cluster the Entities and Value Objects into Aggregates and define boundaries around each. Choose one Entity to be the "root" of each Aggregate, and control all access to the objects inside the boundary through the root. Only allow references to the root to be held by external objects. Transient references to internal members can be passed out for use within a single operation only. Because the root controls access it cannot be blind-sided by changes to the internals. This makes it practical to enforce all invariants for objects in the Aggregate and for the Aggregate as a whole in any state-change. pg 93
- Aggregates mark off the scope within which invariants have to be maintained at every stage of the lifecycle. pg 97

### Factories

- When creation of an object or an entire Aggregate becomes complicated or reveals too much of the internal structure, Factories provide encapsulation.
- An object that expresses some model concept should be distilled to the point that nothing remains that does not relate to its meaning or support its role in those interactions.
- Problems arise from giving a complex object responsibility for its own creation.
- Since cars and are never assembled and driven at the same time, there is no value in combining both of these functions into the same mechanism. Likewise, assembling a complex compound object is a job that is best separated from whatever job that object will have to do when it is finished.
- A client taking on object creation becomes unnecessarily complicated and blurs its responsibility. It breaches the encapsulation of the domain objects and Aggregates created. Even worse if the client is part of the application layer, then the responsibilities have leaked out of the domain layer pg  99
- IMPORTANT: Creation of an object can be a major operation in itself, but complex assembly operations do not fit the responsibility of the created objects. Combining these responsibilities can produce ungainly designs that are hard to understand. Shifting the responsibility of directing construction to the client muddies the design of the client, breaches encapsulation of the assembled object or Aggregate and overly couples the client to the implementation of the created object. pg 99
- Complex object creation is a responsibility of the domain layer that does not belong to the objects that express the model. It has no meaning in the domain, but is a necessity of the implementation.
- IMPORTANT: Shift the responsibilities for creating instances of complex objects and Aggregates to a separate object, which may itself have no responsibility in the domain model, but is still part of the domain design. Provide and interface that encapsulates all complex assembly and that does not require the client to reference the concrete classes of the objects being instantiated. Create entire Aggregates as a piece, encoding their invariants.
- Several special purpose creation patterns can be used for this: Factory Method, Abstract Factory, and Builder
- Two basic requirements of a factory are pg 100:
	- Each creation method is atomic and enforces all invariants of the created object or aggregate.
	- The Factory should be abstracted to the type desired, rather than the concrete class involved.
- You create a factory to build something whose details you want to hide, and you place the factory where you want the control where you want the control to be
- When there doesn't seem to be a natural host, we must create a dedicated Factory object or Service. pg 101
- Factories can actually obscure simple objects that don't use polymorphism
- Complex assemblies, especially of Aggregates, call for factories.
- The operation must be atomic, creation should fail if all the events are not satisfied
- The Factory will be coupled to its arguments pg 103
- The Factory can delegate invariant checking to the product and this is often best. Under some circumstances there are advantages to placing invariant logic in the Factory and reducing clutter in the product. This is especially appealing with aggregate rules. It is especially unappealing with Factory Methods attached to other domain objects.
- Factories are good for assigning identifiers, although they are usually done at a infra layer
- An Entity Factory used for reconstitution:
	- Does not assign a new tracking id
	- Will handle violation of an invariant differently, will inform and attempt to fix.


## Repositories

- Three ways to get a reference to an object: pg 106
	- Create an object
	- Traverse associations
	- Execute a query to find the object in a database base on its attributes, or find the constituent of an object and then _reconstitute_ it
- IMPORTANT: A client needs a practical means of acquiring references to pre-existing domain objects. If the infra makes it easy to do so, the developers of the client may add more traversable associations, muddling the model. On the other hand, they may use queries to pull the exact data they need from the database, or few specific objects rather than navigating the modeled associations. Domain logic moves into queries and client code, and the Entities and Values Objects become mere data containers. The sheer technical complexity of applying most database access infra quickly swaps the client code, leading to the dumbing-down of the domain layer and the irrelevance of the model. pg 107
- Generally speaking, Value Objects do not need global search access. Persistent Value Objects are usually found by traversal from some Entity that acts as the root of the Aggregate that encapsulates them.
- IMPORTANT: A subset of persistent objects must be globally accessible through a search based on object attributes. Such access is needed for the roots of Aggregates that are not convenient to reach by traversal. They are usually Entities, sometimes Value Objects with complex internal structure, and sometimes Values. Providing access to other objects muddies important distinctions. Free database queries can actually breach the encapsulation of domain objects and Aggregates. Exposure of technical infrastructure and database access mechanisms complicates the client and obscures the Model-Driven design.
- The Repository pattern provides us a simple conceptual framework to encapsulate those solutions and bring back our modal focus.
- A Repository represents all objects of a certain type as a conceptual set (usually simulated). It acts like a collection, except with more elaborate querying capability. Objects of the appropriate type are added and removed, and the machinery behind the Repository inserts them or deletes them from the database.
- The client requests objects from the Repository using querying methods that select objects based on criteria specified by the client, typically specifying the value of certain attributes. The Repository retrieves the requested object, encapsulating the machinery of database queries and metadata mapping. Repositories can implement a variety of queries that select objects based on whatever criteria the client requires.
- IMPORTANT: For each type of object that needs global access, create an object that can provide the illusion of an in-memory collection of all objects of that type. Set up access through a well-known global interface. Provide methods to add and remove objects of that type, which will encapsulate the actual insertion or removal of data in the datastore. Provide methods that select objects based on some criteria, and return fully instantiated objects or collections of objects whose attributes values meets the criteria, thereby encapsulating the actual storage and query technology. Only provide repositories for Aggregate roots that actually need direct access. Keep the client focused on the model, delegating all object storage and access to the repositories.
- Advantages:
	- Simple concept for obtaining persistent objects and managing their lifecycle
	- Decouples application and domain design from persistence technology, multiple database strategies or even multiple data sources
	- Communicates design decisions about objects access
	- Easy substitution of dummy implementation, for use in testing, using in-memory collection.