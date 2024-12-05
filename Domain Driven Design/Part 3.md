
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

### Applying and Implementing Specification

- Reasons to specify the state of an object (pg 160):
	- Validation of an object to see if it fulfills some need for some purpose
	- selection of an object from a collection
	- specifying the creation of a new object to fit some need
- To avoid leaving infrastructure in the specification, methods should be added to the repository to translate specifications into a infrastructure specific query. Example:

Specification Class:
```java
public class DelinquentInvoiceSpecification {
	// basic DelinquentInvoiceSpecification code here
	
	public Set satisfyingElementsFrom(InvoiceRepository repository) {
		//Delinquency rule is defined as due date and grace period past.
		return repository.selectWhereGracePeriodPast();
	}
}
```

Repository Class:
```java
public class InvoiceRepository {
	public Set selectWhereGracePeriodPast(){
		//This is not a rule, just a specialized query.
		String sql = whereGracePeriodPast_SQL();
		ResultSet queryResultSet =
			SQLDatabaseInterface.instance().executeQuery(sql);
		return buildInvoicesFromResultSet(queryResultSet);
	}
	
	public String whereGracePeriodPast_SQL() {
		return "SELECT *" +
			" FROM INVOICE, CUSTOMER" +
			" WHERE INVOICE.CUST_ID = CUSTOMER.ID" +
			" AND TO_DAYS(INVOICE.DUE_DATE) + CUSTOMER.GRACE_PERIOD" +
			" < TO_DAYS(" + SQLUtility.dateAsSQL(currentDate) + ")";
	}
	
	public Set selectSatisfying(InvoiceSpecification spec) {
		return spec.satisfyingElementsFrom(this);
	}
}
```

- This leaves infrastructure specific code (SQL queries in this case) in the Repository, while the Specification controls what query should be used. It doesn't collect the rules as neatly into the Specification, but the essential declaration is there of what constitutes delinquency. pg 163
- Can keep the rule in the specification pg 164
```java
public class InvoiceRepository {
	public Set selectWhereDueDateIsBefore(Date aDate) {
		String sql = whereDueDateIsBefore_SQL();
		ResultSet queryResultSet =
			SQLDatabaseInterface.instance().executeQuery(sql);
		return buildInvoicesFromResultSet(queryResultSet);
	}
	
	public String whereDueDateIsBefore_SQL() {
		return "SELECT *" +
			" FROM INVOICE" +
			" WHERE TO_DAYS(INVOICE.DUE_DATE)" +
			" < TO_DAYS(" + SQLUtility.dateAsSQL(currentDate) +")";
	}
	
	public Set selectSatisfying(InvoiceSpecification spec) {
		return spec.satisfyingElementsFrom(this);
	}
	
}

public class DelinquentInvoiceSpecification {
	//basic DelinquentInvoiceSpecification code here
	public Set satisfyingElementsFrom(InvoiceRepository repository) {
		Collection pastDueInvoices =
			repository.selectWhereDueDateBefore(currentDate);
		
		Set delinquentInvoices = new HashSet();
		Iterator it = pastDueInvoices.iterator();
		while (it.hasNext()) {
			Invoice anInvoice = (Invoice)it.next();
			if (this.isSatisfiedBy(anInvoice))
				delinquentInvoices.add(anInvoice);
		}
		return delinquentInvoices;
	}
}
```

### Building to order
- Whole new objects or set of objects can be made or reconfigured to satisfy the Specification
- An interface of the generator that is defined in terms of a descriptive Specification explicitly constrains the generated objects. This will have several advantages:
	- The generators implementation is decoupled from its interface. The specification declares the requirements for the output, but that does not define how the result is reached.
	- The interface communicates its rules explicitly so developers can know what to expect from the generator without understanding all the details of its operation. The only way to predict the behaviour of a procedurally defined generator is to run cases or to understand every line of code.
	- The interface is more flexible, or can be enhanced with more flexibility, since the statement of the requests is in the hands of the client, while the server (generator) is only obligated to fulfill the letter of the Specification.
	- It is easier to test since the model contains an explicit way to define input into the generator that is also a validation of the output. That is, the same Specification that is passed into the generators interface to constrain the creation process can also be used, in its validation role to confirm the created object is correct.


## Supple Design

pg 170

- IMPORTANT: If a developer must consider the implementation of a component in order to use it, the value of encapsulation is lost. If someone other than the original developer must infer the purpose of an object or operation based on implementation, that new developer may infer the purpose that the operation or class fulfills only by chance. If that was not the intent, the code may work for the moment, but the conceptual basis of the design will have been corrupted, and the two developers will be working at cross-purposes.
- IMPORTANT: Name classes and operations to describe their effect and purpose, without reference to the mean by which they do what they promise. This relieves the client developer of the need to understand the internals. These names should conform to the Ubiquitous Language so that team members can quickly infer their meaning. Write a test for a behaviour before creating it, to force your thinking into the client developer mode. pg 172

#### Side Effect-Free Functions

pg 175

- IMPORTANT: Interactions of multiple rules or compositions of calculations become extremely difficult to predict. The developer calling an operation must understand its implementation and the implementation of all its delegations in order to anticipate the result. The value of any abstraction of interfaces is limited if the developers are forced to pierce the veil. Without safely predictable abstractions, the developers must limit the combinatory explosion, placing a low ceiling on the richness of behaviour that is feasible to build.
- The problem of side-effects can be mitigated it two ways:
	- Keep commands and queries strictly segregated in different operations. The methods that cause changes do not return domain data, and kept as simple as possible.
	- There are often alternative models and designs that do not call for existing object to be modified at all. Instead a new Value Object, representing the result of a computation is created and returned.
- IMPORTANT: Place as much logic of the program as possible into functions, operations that return results with no observable side effects. Strictly segregate commands (resulting in modifications to observable state) into very simple operations that do not return domain information. Further control side effects by moving complex logic into Value Objects with conceptual definitions fitting the responsibility.
- Side-Effect-Free-Functions, especially in immutable Value Objects, allow safe combination of operations. When a Function is presented through an Intention Revealing Interface, a developer can use it without understanding the detail of its implementation.

### Assertions

pg 179

- State post conditions of operations and invariants of classes and Aggregates. If Assertions cannot be coded directly in your programming language, write automated unit tests for them. Write them into documentation or diagrams where it fits the style of the project's development process. Seek models with coherent sets of concepts, which lead a developer to infer the intended Assertions, accelerating the learning curve and reducing the risk of contradictory code. pg 179
- IMPORTANT: When side effects of operations are only defined implicitly by their implementation, designs with a lot of delegation become a tangle of cause and effect. The only way to understand a program is to trace execution through branching. The value of encapsulation is lost. The necessity of tracing concrete execution defeats abstraction. pg 179
- **post conditions** -  describe the side effects of an operation, the guaranteed outcome of calling a method
- **pre-conditions** - conditions that must be satisfied in order for the post-conditions guarantee to hold
- IMPORTANT: State post conditions of operations and invariants of classes and Aggregates. If assertions cannot be coded directly in your programming language, write automated unit tests for them. Write them in documentation or diagrams where it fits the style of the project's development process. Seek models with coherent sets of concepts, which lead a developer to infer the intended assertions, accelerating the learning curve and reducing the risk of contradictory code.

#### Conceptual Contours

pg 183

- IMPORTANT: When elements of a model or design are embedded in a monolithic construct, their functionality get duplicated. The external interface doesn't say everything a client might care about. Their meaning is hard to understand, because different concepts are mixed together. On the other hand, breaking classes and methods down can pointlessly complicate the client, forcing client objects to understanding how tiny pieces fit together. Worse, a concept can be lost completely.
- IMPORTANT: Decompose design elements (operations, interfaces) into cohesive units, taking into consideration you intuition of the important divisions in the domain. Observe the axes of change and stability through successive refactorings and look for underlying Conceptual Contours that explain these shearing patterns.
- Intention Revealing Interfaces allow clients to present objects as units of meaning rather than  just mechanisms.
- Side Effect Free Functions and Assertions make it safe to use those units and make complex combinations.


#### Standalone Classes

pg 188

- Interdependencies make models hard to understand. They also make them hard to test and maintain.
- IMPORTANT: Even within a Module, the difficulty of interpreting a design increases wildly as dependencies are added. This adds to mental overload, limiting the design complexity a developer can handle. Implicit concepts contribute to this load even more than explicit references.
- IMPORTANT: Low coupling is a fundamental to the object design. When you can, go all the way. Eliminate all other concepts from the picture. Then the class will be completely self-contained and can be studied and understood alone. Every such self-contained class significantly eases the burden of understanding a Module.


#### Closure of Operations

pg 190

- Most interesting things end up doing things that can't be characterized by primatives alone.
- IMPORTANT: Where is fits, define an operation whose return type is the same as the type of its arguments. If the implementer's has state that is used in the computation, then the implementer is effectively an argument of the operation, so the arguments and return value should be of the same type as the implementer.
- An operation can be closed under an abstract type, in which case specific arguments can be different concrete classes. Afterall, addition is closed under real numbers.


### Relating Design Patterns to the Model

#### Strategy

- IMPORTANT: Domain models contain processes that are not technically motivated, but actually meaningful in the problem domain. When alternative processes must be provided, the complexity of choosing the appropriate process is combined with the complexity of the multiple processes themselves, and things get out of hand. pg 219


### Composite

- Common behaviour has to be duplicated at each layer of the hierarchy, and nesting is rigid. Clients must deal with different levels of the hierarchy through different interfaces even though there may be no conceptual difference they care about. Recursion through the hierarchy to produce aggregate information.
- Define an abstract type that encompasses all members of COMPOSITE. Methods that return information are implemented on containers to return aggregate information about their contents, while "leaf" nodes implement them based on their values. Clients deal with the abstract type and have no need to distinguish leafs from containers.