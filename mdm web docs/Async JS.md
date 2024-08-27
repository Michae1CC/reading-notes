
https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous

Consider the following:
```javascript
function doStep1(init) {
	return init + 1;
}

function doStep2(init) {
	return init + 2;
}

function doStep3(init) {
	return init + 3;
}

function doOperation() {
	let result = 0;
	result = doStep1(result);
	result = doStep2(result);
	result = doStep3(result);
	console.log(`result: ${result});
}

doOperation();
```

- we have a single operation that's split into three steps, where each step depends on the last step
- as a synchronous program, this is very straight forward. But what if we implemented the steps using callbacks?

```javascript
function doStep1(init, callback) {
	const result = init + 1;
	callback(result);
}

function doStep2(init, callback) {
	const result = init + 2;
	callback(result);
}

function doStep3(init, callback) {
	const result = init + 3;
	callback(result);
}

function doOperation() {
	doStep1(0, (result1) => {
		doStep2(result1, (result2) => {
			doStep3(result2, (result3) => {
				console.log(`result: ${result3}`);
			});
		});
	});
}

doOperation();
```

- when callbacks are nested like this it can get very hard to handle errors
- for this reason most modern async APIs don't use callbacks and instead use `Promise`


### How to use Promises
- A promise is an object returned by an async function, which represents the current state of the operation.
- A promise can be in one of the following states:
	- pending: The promise has been created and the async function it's associated with has not succeeded of failed yet
	- fulfilled: the async function has succeeded. When the promise is fulfilled, its `then()` handler is called
	- rejected: the async function has failed. When a promise is rejected, it's catch handler is called
- The promise returned by `Promise.all` is
	- fulfilled when and if all the promises in the array are fulfilled. In this case, then `then()` handler is called with an array of all the responses in the same order they where passed in
	- rejected if **any** of the promises in the array are rejected. In this case, the `catch()` handler is called with the error thrown by the promise that rejected.
- Using `await` on an async function forces the operations to be completed in series.


https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing_workers