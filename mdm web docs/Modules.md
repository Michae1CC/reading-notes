
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#creating_a_module_object

- Import each module's feature into a module object
```javascript
import * as Moduel from "./modules/module.js";
```

- You can also export and import classes; useful for when your module code is written in an OO style.
```javascript
class Sqaure {
	...
}

export { Square }
```

- Several sub modules can be combined into one parent module
```javascript
export * from "x.js";
export { name } from "x.js";
```

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#dynamic_module_loading

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#import_declarations_are_hoisted

- Import declarations are hoisted, which means that the imported values are available in the modules code even before the places that declares them, and that the imported module's side effects are produced before the rest of the module's code starts running.
- Keep module "isomorphic" exhibits the same behaviour in any environment (a browser, node, etc)
	- Separate your modules into "core" and "binding". For core focus on pure JS logic without DOM or file system access.
	- For binding, you can read and write to the global context
	- Detect whether a particular global exists before using it, eg `typeof window === "undefined"`
	- Use a polyfill to provide a fallback for missing features