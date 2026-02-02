
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