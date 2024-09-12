- JS is designed to be non-blocking on the "main" thread, where views are rendered
- State dependency arises when the input or other variable of a function relies on an outside function
https://nodejs.org/en/learn/asynchronous-work/asynchronous-flow-control#state-management

- You will be able to perform almost all of your operations with the following 3 patterns:
	- In series - functions will be executed in a strict sequential order, this one is the most similar to `for` loops
	- Full parallel - when ordering does not matter, such as emailing 1,000,000 staff
	- Limited parallel: parallel with limit, such as successfully emailing 1,000,000 recipients from a list of 10 mil users

https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking

- Capacity in Node is single threaded, so concurrency refers to the event loop's capacity to execute JS callback functions after completing other work.
https://nodejs.org/en/learn/asynchronous-work/discover-javascript-timers
- JS is synchronous by default and is single threaded meaning it cannot create new threads to run in parallel
- The timeout function specifies a callback function to execute later, and a value expressing how much later in milliseconds.
- A delay of 0 will cause the function to execute as soon as possible after the current function.
- setInterval starts a function every n milliseconds, without any consideration about when a function finished its execution
- `setTimeout` and `setInterval` are available in node through the timers module

https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick

- The event loop is what allows node to perform non-blocking I/O operations - despite the fact that a single JS thread is used by default - by offloading operations to the system kernel whenever possible.
- When node starts, it initialises the event loop, processes the provided input script which may make API calls, schedule timers or call `process.nextTick`, then begins processing the event loop.
- overview of the event loop operations
```
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```

- timers: this phase executes callbacks scheduled by `setTimeOut` and `setInterval`
- pending callbacks: executes I/O callbacks deferred to the next loop iteration
- idle, prepare: only used internally
- poll: retrieve new I/O; execute I/O related callbacks; node will block here
- check: callbacks are invoked here
- close callbacks: some close callbacks
- The main advantage to using `setImeediate` over `setTimeout` is `setImmediate` will always be executed before any timers if scheduled within an I/O cycle, independently of how many timers are present.

https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick#phases-in-detail
#### Timers

- A timer specifies the threshold after which a provided callback may be executed rather than the exact time a person wants it to execute
- Timer callbacks will run as early as they can be scheduled after the specified amount of time has passed.

#### Pending Callbacks

- This phase executes callbacks for some system operations such as types of TCP errors.

#### Poll

- The poll phase has two main functions
	- Calculate how long it should block and poll for I/O
	- Process events in the poll queue