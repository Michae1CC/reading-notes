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

Further reading: https://www.typescriptlang.org/docs/handbook/modules/theory.html

- Node will consider every `.js` file to be written using the CommonJS syntax by default, therefore, if we use the ESM syntax  inside a `.js` file, the interpreter will throw an error.
- There are several ways to tell the `Node.js` interpreter to consider a give module as an ES module rather than a CommonJS module:
	- Give the module file the extension `.mjs`
	- Add to the nearest parent package.json a field called `type` with a value of `module`
- es module allows us to export functionality through the `export` keyword
- It's good practice to stick with named exports, especially when you to expose more than one functionality
- Module Identifiers
	- Relative specifiers like `./logger.js`
	- Absolute specifiers `file:///opt/nodejs/config.js`
	- Bare specifiers `http`, represent modules available in the `node_modules` folder and generally installed through a package manger
	- Deep import specifiers like `fastify/lib/logger.js`

### Loading phases
- From the entry point the the interpreter will find and follow all the import statements recursively in a depth-first fashion
	- Phase 1 - Find all the imports and recursively load the content of every module from the respective file
	- Phase 2 - For every exported entity, keep a named reference in memory, but don't assign any value just yet. Also, references are created for all import and export statements tracking the dependency relationship between them.
	- Phase 3 - Node finally executes the code so that all the previously instantiated entities can get an actual value.

pg 49

- In Es modules, when an entity is imported in the scope, the binding to its original value cannot be changed (read-only binding) unless the bound value changes within the scope of the original module itself.
- We can't change the bindings of the default export of an existing module from another module, but, if one of these bindings is an object, we can still mutate the object itself by reassigning some of the object properties - p 56
- *named exports* involves assigning the values we want to make public to properties of the object referenced by exports.
- reassigning the whole `module.exports` variable to a function. The main strength of this pattern is the fact that it allows you expose only a single functionality which provides a clear entry point for the module, making it simpler to use and understand.
```javascript
// file logger.js
module.exports = (message) => {
	console.log(`info: ${message}`)
}
module.exports.verbose = (message) => {
	console.log(`verbose: ${message}`)
}

// file main.js
const logger = require('./logger')
logger('This is an informational message')
logger.verbose('This is a verbose message')
```

- In ESM, some important references (`require, exports, module.exports, __filename, __dirname`). If we try to get any of them in a ES module, we will get a ReferenceError - pg 60
- In the global scope of ES module, this is `undefined`, while in CommonJS, `this` is a reference to exports.