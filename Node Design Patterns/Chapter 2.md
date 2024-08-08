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

pg 27