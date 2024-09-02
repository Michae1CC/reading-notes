
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