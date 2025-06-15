

Cryptographic Failures common violations
- Web server doesn’t enforce TLSv1.2 or later; attacker can force a downgrade to a vulnerable version
- Application uses database transparent data encryption
- Poor key generation and lifecycle management

Cryptographic Failures how to prevent
- Don't store sensitive data unnecessarily. Discard it as soon as possible or use PCI DSS compliant tokenization or even truncation.
- Make sure to encrypt all sensitive data at rest.
- Initialization vectors must be chosen appropriate for the mode of operation

When is an app vulnerable to injections
- User-supplied data is not validated, filtered, or sanitized
- Hostile data is directly used or concatenated

Typical Injection Vulnerabilities
- SQL injection manipulates the SQL commands sent to backend databases
- OS Command injection allows an attacker to execute shell commands
- LDAP injection allows attackers to compromise a website’s authentication process

Injection Prevention
- Validate input parameters to the minimum character set possible
- Use Prepared SQL Statements
- Use LIMIT and other SQL controls within queries to prevent mass disclosure of records
- Maintain a whitelist of valid commands

Insecure Design
- A broad category representing different weaknesses, expressed as “missing or ineffective control design.”
- An insecure design cannot be fixed by a perfect implementation as by definition, needed security controls were never created to defend against specific attacks

Insecure Design Typical Vulnerabilities
- Generation of error message containing sensitive information
- Uncontrolled resource consumption
- Unprotected storage of credentials

Insecure Design Prevention
- Use threat modeling for critical authentication, access control, business logic, and key flows
- Segregate tier layers on the system and network layers depending on the exposure and protection needs
- Establish and use a library of secure design patterns or paved road ready to use components