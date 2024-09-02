- JS is designed to be non-blocking on the "main" thread, where views are rendered
- State dependency arises when the input or other variable of a function relies on an outside function
https://nodejs.org/en/learn/asynchronous-work/asynchronous-flow-control#state-management

- You will be able to perform almost all of your operations with the following 3 patterns:
	- In series - functions will be executed in a strict sequential order, this one is the most similar to `for` loops
	- Full parallel - when ordering does not matter, such as emailing 1,000,000 staff
	- Limited parallel: parallel with limit, such as successfully emailing 1,000,000 recipients from a list of 10 mil users

https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking

- Capacity in Node is single threaded, so concurrency refers to the event loop's capacity to execute JS callback functions after completing other work.

- JS is synchronous by default and is single threaded meaning it cannot create new threads to run in parallel
- The timeout function specifies a callback function to execute later, and a value expressing how much later in milliseconds.
- A delay of 0 will cause the function to execute as soon as possible after the current function.