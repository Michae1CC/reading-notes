### Prototype Chaining
- Every object has a builtin property called an object
- When a propert can't be found in an object, the prototype is searched for the property
- Object can inherit behaviours and proprties of another object
- Every object can have exactly one prototype, when you get to the end of the chain, the value is null

- prototype provides a mechanism to support an inhertiance-based paradiagm in js
- It allows object to inherit and use properties of object which preceed it in the chain
- Acts a bit like __mro__ in python

Resources: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes