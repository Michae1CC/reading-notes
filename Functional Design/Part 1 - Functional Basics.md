
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

#### Software Transactional Memory (STM)

pg 48

- STM is a set of mechanisms that treat internal mem as though it were a transactional commit/rollback database. Transactions are functions that are protected from concurrent update by a compare-and-swap protocol.
- Avoids the problem of dead locks that may arise if functions _f_ and _g_ attempt to concurrently lock and update objects _o_ and _p_.
- STM avoids this problem by _not_ and instead using a commit/rollback technique. If _swap (o,f)_ updates the object _o_ with _o_f = f(o)_, _swap_ will hold the current state of _o_ in _o_h_
- _swap_ will compute _o_f_ and then in an atomic operation, compare _o_ with _o_h_ and if they are the same, update _o_ with _o_f_
- They mention here that _swap_ here may attempt to update _o_ again, but this probably isn't always the best thing to do


#### Open-Closed Principal

pg 131

- Modules should be open for extension, but closed for modification, i.e. extending or changing behavior does not require you to modify code
- It requires that a high-level policy access the low-level detail through an abstraction layer
- In OO programs, we typically create the abstraction layer through polymorphic interfaces, high-level policies are given access through those interfaces to the low-level details that implement, or inherit from, those interfaces.
- In dynamically typed OO languages like Python, these interfaces are duck types. _Duck types_ have no particular syntax within the language. They are simply sets of function signatures called by the high-level policies and implemented by low-level details. The dynamic type system determines the polymorphic dispatch at runtime by matching those signatures.

- _Multi-methods_ are another form of duck-typing, because they create a loose grouping of methods that are dynamically dispatched based on their function signature


#### Liskov Substitution Principal

- A subtype must be substitutable for its base type in any program that uses the base type.