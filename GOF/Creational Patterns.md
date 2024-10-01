
## Factory Method

- Use the Factory Method pattern when
	- A class can't anticipate the class of objects it must create
	- A class wants it subclasses to specify the objects it creates
	- classes delegate responsibility to one of several helper classes, and you want to localize the knowledge of which helper subclass is the delegate
- The factory method eliminates the need to bind application-specific classes into your code. The code only deals with the Product interface; therefore it can work with any user-defined ConcreteProduct classes
- Creating objects inside a class with a factory method is always more flexible then creating an object directly. Factory Method gives subclasses a hook for providing an extended version of an object.
- Factory methods don't have to be just called by Creators. Clients can find factory methods useful, especially in the case of parallel class hierarchies. Parallel class hierarchies result when a class delegates some of its responsibilities to a separate class.

#### Implementation

- Two major varieties:
	- 1: The Creator class is an abstract class and does not provide an implementation for the factory method it declares
	- 2: The Creator is a concrete class and provides a default implementation for the factory method.
- Parameterized factory methods: The factory method takes a parameter that identifies the kind of object to create. All objects the factory method creates will share the Product interface.
- Store the class to be created as a class variable of Application. That way the we don't have to subclass Application to vary the product.