
pg 12
- Function programs are true functions, if you decompose a functional program into many smaller functions, each of those will also be a true a true function. This is called referential transparency.
- Functional programs compute a new state from an old state, _without changing the old state_.

Domain Driven Design - Side Effect-Free Functions, pg 175

- Operations can be broadly divided into two categories, commands and queries. Queries obtain information from the system, possibly by simply accessing data in a variable, possible performing a calculation based on that data. Commands (also known as modifiers) are operations that affect some changes to the systems.
- As soon as arbitrary nesting it becomes very hard to anticipate all the consequences of invoking an operation.
- Interactions of multiple rules of compositions become extremely difficult to predict. The developer calling an operation must understand its implementation of all its delegations in order to anticipate the result. The value of any abstraction of interfaces is limited if the developers are forced to pierce the veil.
- Keep commands and queries segregated in different operations.
- Often alternative models and designs that do not call for an existing object to be modified at all. Just create a new Value Object
- Control side effects by moving complex logic into Value Objects with conceptual definitions fitting the responsibility.


