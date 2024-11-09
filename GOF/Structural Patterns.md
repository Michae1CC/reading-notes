
## Bridge Pattern

### Participants

- Abstraction
	- Defines the abstraction's interface
	- Maintains a reference to an object of type Implementor
- Refined Abstraction
	- Extends the interface defined by an Abstraction
- Implementor
	- Defines the interface for implementation classes. This interface doesn't have to correspond exactly to Abstraction's interface; in fact the two interfaces can be quiet different. Typically the Implementor interface provides only primitive operations, and Abstraction defines higher-level operations based on these primitives.
- ConcreteImplementor
	- Implements the Implementor interface and defines its concrete implementation.