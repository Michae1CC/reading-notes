
9/06/2025

Exam:
- Understand OWASP top 10, examples
- Crypto
- CIA

### OWASP A03 Injection

- Fundamentally this involves using untrusted input, containing malicious payload in a server-side interpreter / application query without validating and/or sanitizing 
- Can facilitate auth bypass, extraction of sensitive data and/or compromising target database
- Classic example is SQL injection


### Typical vulnerabilities

- Injection such as SQL, NoSQL, OS Command, LDAP, Expression Language
- SQL injection Manipulates the SQL commands sent to the backend databases
- OS Command injection allows attacker to execute commands
- LDAP injection allows the attacker to compromise a websites auth process if LDAP is used for auth

### Prevention

- General:
	- Restrict the character set
	- Escape special characters
- SQL Injection:
	- Use pre-pared statements
- OS Command injection
	- Maintain a command whitelist
	- Maintain a command parameter whitelist, check for special chars
	- Restrict inputs to be alpha numeric
- LDAP:
	- Escape untrusted data
	- Use Encoding for LDAP search


### OWASP A04 Insecure Design

- Focuses on risks associated with insecure design and architectural flaws
- Insecure design can never be fixed by perfect implementation
- No the root cause of all sec vuln
- Lack of business risk analysis is a significant factor that undermines the level of security design and controls needed/not needed
	- Tenant separate for multi-tenant systems
	- Network architecture and trusted zones
	- Encryption requirements
	- Sensitive data storage medium
- Cyber sec is driven by a business risk

#### Design Attack Scenarios

- Use of questions and answers in credential recovery workflow. Answers to many of these questions and be obtained via internet searches
- Use of a single hardware instance to implement / separate trusted and untrusted network zones. Compromise of the hardware would allow access to trusted zones.
- Uncontrolled API call velocity by a single customer can starve access for customers
- Deletion of customer data may not be possible when stored on SAN drives and/or backups
- Compromise of databases credentials may expose all customer data and/or sensitive data
- Prevention:
	- Incorporate business risk analysis to drive the necessary security controls
	- Deploy a documented secure hardware secure software development lifecycle
	- Embed the use of secure design patterns or "paved road" components into the development infra and culture
	- Threat modelling for security critical functions
	- Incorporate security control into user stories
	- Develop test cases with user stories