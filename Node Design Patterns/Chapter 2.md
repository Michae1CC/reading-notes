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

#### Resolving Algorithm
- File modules: If the moduleName starts with /, it is already a considered an absolute path which is calculated from the directory of the requiring module
- Core modules: If moduleNames is not prefixed with / or ./, the algorithm will first try to search within the core Node.js modules
- Package modules: If no module is found matching moduleName then the search continues by looking in the first node_modules directory that is found navigating up the directory structure
- For file and package modules, the algorithm will try to match the following:
	- `<moduleName>.js`
	- `<moduleName>/index.js`
	- The directory/file specified in the main property of `<moduleName>/package.json`

- You can re-assign the `module.exports` variable to a function. The main strength of this pattern is that it allows to expose only a single functionality which provides a clear entry point. It honours the principle of small surface area.
- Monkey patch refers to the practice of modifying the existing objects at runtime to change or extend their behaviour or to apply temporary fixes
- We can add a new function to another module using the following technique
```javascript
require('./logger').customMessage = function () {
	console.log('This is new functionality');
}
```
- This is usually not great practice since what if two modules tried to set the same global variable?

### ECMAScript modules
- Node will consider every `.js` file to be written using the CommonJS syntax by default, therefore, if we use the ESM syntax  inside a `.js` file, the interpreter will throw an error.
- There are several ways to tell the `Node.js` interpreter to consider a give module as an ES module rather than a CommonJS module:
	- Give the module file the extension `.mjs`
	- Add to the nearest parent package.json a field called `type` with a value of `module`
- es module allows us to export functionality through the `export` keyword
- It's good practice to stick with named exports, especially when you to expose more than one functionality 