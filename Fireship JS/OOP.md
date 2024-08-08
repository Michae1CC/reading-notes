- constructors in JS provide us with something like a class definition, enabling us to define the "shape" of an object
- The prototype chain is a natural way to implement inheritance

- when a subclass is instantiated, a single object is created which combines the properties defined in the subclass with properties defined further up in the hierarchy
- with prototyping, each level of the hierarchy is represented by a separate object and they are linked via the `__proto__` property