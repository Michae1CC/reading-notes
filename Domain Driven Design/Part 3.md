
- A "deep model" provides a lucid expression of the primary concerns of the domain experts and their most relevant knowledge while it sloughs off the superficial aspects of the domain. A deep model usually has abstract elements - pg 134

### Making Implicit Models Explicit

- You can find books that explain the fundamental concepts and conversation wisdom.
- Explicit Constraints: Factoring the constraint into its own method allows us to give it an intention revealing name that makes the constraint explicit in our design. It also gives the constraint room. pg 156
- Warning signs that a constraint is distorting the design of its host object:
	- Evaluating a constraint requires data that does not otherwise fit the object's definition
	- Related rules appear in multiple objects, forcing duplication or inheritance between objects that are not otherwise a family
- IMPORTANT: Business rules often do not fit the responsibility of any of the obvious Entities or Value Objects and their variety and combinations can overwhelm the basic meaning of the domain object. But moving the rules out of the domain layer is even worse, since code no longer expresses the model. _Logic programming_ provides the concept of separate, combinable, rule objects called predicates, but full implementation of this concept with objects is cumbersome. It is also so general that it doesn't communicate intent as much as more specialized designs.
- Truth tests that can be factored out into a separate Value Object. This new object can evaluate another object to see if the predicate is true for the object. The new object is a specification.
- A Specification states a constraint on the state of another object, which may or may not be present.
- IMPORTANT: Create explicit predicate-like Value Objects for specialized purposes. A Specification is a predicate that determines if an object does or does not satisfy some criteria. pg 159