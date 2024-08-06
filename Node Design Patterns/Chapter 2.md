- Every browser scripts runs in the global namespace
- A popular technique to solve is the revealing module pattern
```javascript
const myModule = (() => {
	const privateFoo = () = {}

	const exported = {
		publicFoo: () => {},
	}

	return exported
})()
```

#### Common JS Modules
- require is a function that allows you to import a module from the local file system
- export and module.exports are special variables that can be used to export public functionality

pg 22