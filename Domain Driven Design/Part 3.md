
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