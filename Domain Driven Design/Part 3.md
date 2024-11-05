
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

- IMPORTANT: Interactions of multiple rules or compositions of calculations become extremely difficult to predict. The developer calling an operation must understand its implementation and the implementation of all its delegations in order to anticipate the result. The value of any abstraction of interfaces is limited if the developers are forced to pierce the veil. Without safely predictable abstractions, the developers must limit the combinatory explosion, placing a low ceiling on the richness of behaviour that is feasible to build.
- The problem of side-effects can be mitigated it two ways:
	- Keep commands and queries strictly segregated in different operations. The methods that cause changes do not return domain data, and kept as simple as possible.
	- There are often alternative models and designs that do not call for existing object to be modified at all. Instead a new Value Object, representing the result of a computation is created and returned.
- IMPORTANT: Place as much logic of the program as possible into functions, operations that return results with no observable side effects. Strictly segregate commands (resulting in modifications to observable state) into very simple operations that do not return domain information. Further control side effects by moving complex logic into Value Objects with conceptual definitions fitting the responsibility.
- Side-Effect-Free-Functions, especially in immutable Value Objects, allow safe combination of operations. When a Function is presented through an Intention Revealing Interface, a developer can use it without understanding the detail of its implementation.