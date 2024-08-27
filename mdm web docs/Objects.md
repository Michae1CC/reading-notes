
https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics

- `this` refers to the current object the code is executed in. In the context of a method, `this` refers to the object that the method was called on
- A constructor is just a function called using the `new` keyword, when you call the constructor it will
	- create a new object
	- bind `this` to the new object, so you can refer to this in the code
	- run the code in the constructor
	- return the new object


### Classes and Objects

https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Classes_in_JavaScript

```javascript
class Person {
	name = '';
	constructor(name) {
		this.name = name;
	}

	introduceSelf() {
		console.log(`Hi! I'm ${this.name}`)
	}
}
```

This declares a class called `Person`, with:
- a `name` property
- a constructor that takes a parameter that is used to initialise the new object's `name` property
- an introduceSelf() method that can refer to the object's properties using this