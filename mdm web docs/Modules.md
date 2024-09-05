
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