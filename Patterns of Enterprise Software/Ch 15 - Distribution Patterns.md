
### Remote Facade

pg 388

- Provides a coarse-grained facade on fine-grained objects to improve efficiency over a network
- In a object-oriented model, you do your best with small objects that have small methods - this gives you lots of opportunity for control and substitution of behaviour, and to use good intention revealing naming to make applications easier to understand
- All the remote facade does is translate coarse-grained methods onto the underlying fine-grained objects
- In a simple case, like an address object, a remote facade replaces all the getting and setting methods of the regular address object with one getter and one setter, often referred to as bulk accessors.
- In transferring information in bulk like this, you need it to be in a form that can easily move over the wire
- You design a remote facade based on the client's usage
- remote facade has no domain logic
- Use remote facade whenever you need remote access to a fine-grained object model


### Data Transfer Object

pg 401

- An object that carries data between processes in order to reduce the number of method calls
- When you're working with a remote interface, such as a Remote Facade, each call to it is expensive. As a result you need to reduce the number of calls, and that means that you need to transfer more data with each call
- You can do this by using a lot of parameters but this is awkward
- Allows you to move several pieces of information over a network in a single call - essential for distributed systems
- The fields in a Data Transfer Object are fairly simple, being primitives, simple classes like string and dates, or other DTO.