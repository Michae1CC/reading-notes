
- A variable declared with `let`, `const`, or `class` is said to be in a temporal dead zone from the start of the block until the code execution reaches the place where the variable is declared and initialized.
- While inside the TDZ, the variable has not been initialised with any value and any attempts to access it will result in a `ReferenceError`.

### Closures

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures

- A closure is the combination of a function bundled together with references to it's surrounding state (the lexical environment). It is a combination of a function and a lexical environment within which that function was declared. This environment consists of any variables that were in-scope at the time the closure was created
- Closures are useful because they let you associate data (the lexical env) with a function that operates on that data. This has parallels to object-oriented programming, where objects allow you to association data with one or more methods. Consequently, you can use a closure anywhere that you might normally use an object with only a single method.
- A nested function's access to the outer functions scopes includes the enclosing scope of the outer function - effectively creating a chain of function scopes.

```js
// myModule.js
let x = 5;
export const getX = () => x;
export const setX = (val) => {
  x = val;
};
```

```js
import { getX, setX } from "./myModule.js";

console.log(getX()); // 5
setX(6);
console.log(getX()); // 6
```

- Here the module exports a pair of getter-setter functions, which close over the module scoped variable `x`. Even when `x` is not directly accessible from other modules, it can be read and written within the functions.

### Scope
https://developer.mozilla.org/en-US/docs/Glossary/Scope
- The current context of execution in which values and expressions are visible or can be referenced.
- Kinds of scopes:
	- Global scope: The default scope for all code running in the script mode.
	- Module scope: The scope for code running in module mode.
	- Function scope: The scope created with a function
	- Block scope: Identifiers created with `let`, `const`, `class`, or `function` can belong to a block scope, created with a pair of braces (a block).

Valid
```js
{
	 var x = 1;
}
console.log(x);
```

Invalid
```js
{
	const x = 1;
}
console.log(x); // ReferenceError: x is not defined
```