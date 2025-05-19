19/05/25

### Key Management

- Keys are typically stored in a HSM
- HSM are network based, PCI based hardware cards or Trusted Platform Modules (TPM)
- Software based HSMs are normally PKCS#12 files
- If implemented correctly, cryptographic keys are never exposed in the clear

- Key management involves:
	- Creating a security officer and user roles within the organisation
	- strictly controlling access to keys
	- managing key lifetimes
- Sec officers are responsible for initializing HSM tokens adding users and managing user's PIN
- Users are normally entities that can access keys and perform cryptographic operations
- Designing and planning for key rollover. That is replacing the current in-use with a new key.
- Designing and planning for rekey capability
- Creds should be centrally managed ideally with a hosted, enterprise-level infra system
- Cloud based sys will normally provide cred management tools
- All cryptographic keys have a lifetime called Crytoperiod
	- Limit the amount of info and time for crypto analysis
	- Limit the exposure if a single key is exposed
	- Limit use of a particular algorithm
- Factors affecting crypto period
- Hardware of software protected keys
- Operating environment
- Volume of data flow or number of transactions
- Cryptograhpic method
- Rekeying method
- Need to think about how you need to search for data
- Threat to new technologies

- Originated-Usage Period: Period in which the crypto protection may be applied
- Recipient-Usage Period: Period during which protected info is processed


### Digital Certs

- Digital certs bind an entity to a public key and digitally singed by a trusted third-party
- Trusted third-parties are normally CA's
- Digital certs are widely used in TLS which underpins HTTPS and provides secure browsing
- Cert is determined by the IETFx.509 standard; the current version is 3
- CAs issue digital certs after they have verified the identity of the entity to which the public key is bound
- A Public Key Infrastructure (PKI) is a system for creating. managing, distribution, storing and verifying digital certs
- Verification of digital certs involves validation of a cert chain:
	- Check cert domain matches the website/entity expected
	- Check cert integrity by validating the signature
	- Check cert start/end dates
	- Check the permitted key usage
	- Check cert revocation status
	- For each cert in the chain repeat steps 2 through 5 plus:
		- Check basic constraints
		- The number of certs in the chain does not exceed the Basic Constraints pathLenConstraint (refers to CA chain length)
	- Final cert in the chain must be a self-signed root cert and must be held in a trust store (root certs passed over network are ignored and are checked against the trust store)


### TLS

- Communications protocol that provides condfidentiality, integrity and auth between two endpoints
- Used by TLS include asymmetric and symmetric encryption, hashing, digital signatures and digital signatures
- Client says what algorithms it supports, server gets to choose the alg or reject the connection
- Diffie Helman key exchanges uses a different key for every single connection - this is different to using RSA encryption which uses the same public/private key for every single connection


### GDPR

- Europe' newest and harshest data privacy law
- It came to enforce it in 2016
- Breaches to GDPR can result in massive fines that may be up 4% of an organization's global revenue
- The GDPR has 7 key data protection principles

- Personal data - Any information that directly and indirectly identifies a person.
- Processing - Any operation that is performed on personal data or sets of personal data
- Pseudonymisation - Personal data can no longer be attributed to a specific data subject without the use of additional information.
- Consent - Any freely given, specific informed and unambiguous indication of the subject's wishes

- Data controller
	- Decides the purpose and means by which personal information is collected and processed
	- Ultimately responsible for the security of the personal data including data processing
- Data Processor
	- Processes personal data on behalf of the data controller
	- Typically third-parties that have contractual relationship with the data controller
	- Must comply must still comply with GDPR

- Protection Principals
	- Collection of data is lawful, with full consent
- Purpose limitation
	- Collected for a specific legit way and not used in any other way
- Data minimization
	- Limited to what is necessary
- Accuracy
	- Must be accurate and kept up to date
- Storage limitation
	- Kept for a minimum amount of time
- Integrity and confidentiality
	- Processed in a manner that ensures the right amount of sec is used

### Australian Privacy Principles

- Principle based law
- The regulator is the Office of the Australian Privacy Commissioner
- Apply to organisations with an annual turnover that exceeds $3M (some exceptions such as medical/heath services)
- Transparent management of data
- Anonymity and pseudonymity
- Collection of unsolicited data
- Notification of collection of personal data
- Disclosure of PII
- Direct marketing
- Cross-border disclosure or PII
- Adoption or disclosure of government ID
- Quality of PI
- Security of PI
- Access to PI
- Correction of PI

### ASD Essentials

- Patch applications
	- Automated scanner to detect missing patches or updates
	- Critical or severe vulnerabilities are patched within 48 hrs
- Patch OS
- Application Control
	- Control restrict the execution of exes, libs, scripts