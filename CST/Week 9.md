
14/07/25

## Cloud Security

- On-Demand Service
- Broad Network Access
- Resource Pooling - Providers computing resources are pooled to serve multiple consumers
- Rapid elasticity
- Measured service

- Private Cloud - Cloud infra provisioned for a exclusive use by a single organization
- Storage level Encryption


### Cloud Sec AWS

- Responsibility matrix
- Key Management is performed by the AWS KMS
- KMS does not perform the encryption but is tightly integrated to numerous services, RDS, S3, EBS
- Regional service
- Integrates with AWS Cloudtrail which means all API calls to keys are tracked and recorded


### Supply Chain Sec

- Cybersec is never just a tech problem, it's people processes and knowledge
- There should be no gap between physical and cybersec
- Lapses in physical sec can facilitate a cyber sec device
- Third party service providers with a physical or virtual access to systems or software
- Poor info security practices by lower-tier/upstream suppliers
- Compromised software or hardware acquired from suppliers
- Software sec vulnerabilities in supply chain management or supplier systems
- Security requirements are included in every RFP or contract
- Security teams work with vendors to address any security vulnerabilities or gaps
- Training for all stakeholders in the lifecycle
- Source code is obtained for all purchased software
- Track and trace programs established provenance for all components
- Tight controls on access by service controls

### XSS and CSFR Examples

- Reflected XSS
	- Web application receives data from a client and includes it in response in an unsafe manner
	- Preconditions and exploitation
		- Web app reflects user input in an unsafe manner
		- The victim needs to log into the vuln web app
		- Victim must (mostly) click on a malicious link sent by an attacker
- Stored XSS
	- Web app receives data from an untrusted source, stores it in an HTTP response later in an unsafe manner
	- Preconditions for exploitation
		- Web app stores malicious attacker script later for retrieval
		- Must visit the vulnerable web app and access the page


### Preparing for Quantum Break

- Inventory all data across the organization and categorize for sensitivity and the need for crypto protection
- Review the use of crypto across the organisation. look for usages of certs, symmetric crypto, signatures, key exchange algorithms, MACs, hash algs and random number generators
- Update symmetric crypto to at least 256 bit keys
- For TLS enforce perfect forward secrecy algorithms
- Establish a relationship with one or more PKI vendors to migrate to PQC safe certs when available
- Establish an application/system architecture and design that supports crypto agility


### AI and Sec

- AI-powered phishing attacks
- AI deepfake technology
- AI in cracking passwords - better algs to crack passwords

- What is the primary goal of cyber sec in an org
- Explain OWASP practice to prevent software and data integrity failures
- Which activity best exemplifies the Shift Left Sec approach in a modern software dev