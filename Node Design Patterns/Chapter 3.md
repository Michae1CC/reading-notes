### Callbacks and Events
- A callback is a function invoked by by the runtime with the result of an async operation - pg 64
- In functional programming, continuation style passing refers to when a callback is passed as an argument to another function and is invoked with the result when the operation completes.
- Every time the event loop takes a full trip we call it a tick. When we pass a function to `process.nextTick()`, we instruct the engine to invoke this function at the end of the current operation, before the next event loop tick starts. The event loop is busy processing the current function code. When this operation ends, the JS engine runs all the functions passed to nextTick calls during that operation. It's the way we can tell the JS engine to process a function async (after the current function), but as soon as possible, not to queue it.
- Calling `setTimeout(() => {}, 0)` will execute the function at the end of the next tick, much later than using `nextTick` which prioritizes the call and executes it just before the beginning of the next tick.
pg 72
- Callbacks are typically the last argument. pg 73
- In CPS, error propagation is done by passing the error to the next callback in the chain

### Observer Pattern

- The `EventTransmitter` class allows us to register one or more functions as listeners, which will be involved when a particular event type is fired. pg 78
- The `EventEmitter` is exported from the events core module.
- The essential methods of the `EventEmitter` are as follows:
	- `on(event, listener)`: This method allows us to register a new listener (a function) for the given event type.
	- `once(event, listener)`: This method registers a new listener, which is then removed after the event is emitted for the first time.
	- `emit(event, [arg1], [...])`: This method produces a new event and provides additional arguments to be passed to the listeners.
	- `removeListeners(event, listener)`: This method removes a listener for the specified event type.

### Callbacks

- Don't abuse in-place function definitions when defining callbacks

### Callback Hell

- Basic principles that help keep the nesting level low and improve the context, to immediately exit the current statement instead of writing complete if/else statements
- Create named functions for callbacks, keeping them out of closures and passing intermediate results as arguments. Naming our functions will also make them look better in stack traces.
- Modularize the code. Split into smaller reusable functions - pg 95
