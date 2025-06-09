
2/06/2025

### OWASP

- Open Worldwide Application Sec Project
- Non-profit organisation focused on improving the security of software
- Vision: No more insecure software
- First release in 2003, current is 2021
- Updated every 3-4 years to allow time to adopt
- Next release due 2025
- Top 10 list:
	- Broken Access control
	- Cryptograhic failures
	- Injection
	- Insecure Design
	- Sec Misconfiguration
	- Vuln and Outdated Components
	- Identification and outdated components
	- Software and Data Integrity failures
	- Sec logging and Monitoring Failures
	- Server-Side Request Forgery


- Broken Access Control
	- Ensuring auth access to resource. Includes authentication and authorisation
	- Authenication is verifying identity
	- Authorisation is about verifying if that identity is allowed to perform the requested operation against a target resource
	- Access Control is often performed on multiple endpoints and at multiple levels increasing the attack surface
	- Typical Vulnerabilities
		- More roles/users are granted to resources than they need to do their job
		- Roles/user granted more priviledges that required to do their job - violation of least privilege principles 
		- Modification of URL params to elevate privileges or allow unauth access to resources
		- CORS misconfigured
	- Prevention
		- Deny by Default
		- Centralized access control mechanisms
		- Enforce record ownership rather than rely on perimeter auth mechanisms
		- Use RBAC or ABAC models rather than specifies identities
		- Log access control failures and ensure log are monitored ideally via an automated alert mechanism
		- Ensure application state is invalidated on logout

- Cryptographic Failures
	- Require sensitive data to protect using strong cryptography
	- Lack of knowledge where all sensitive data is stored
	- Too much data retained/stored
	- Web server does not enforce TLSv1.2 or later, attacker can force a downgrade to a vuln version
	- Doesn't validate tickets
	- Uses database transparent data encryption
	- Passwords are stored using unsalted or weak hashes
	- Poor key generation and lifecycle management
	- Prevention
		- Minimize storage and retention of sensitive data
		- Encrypt are sensitive data
		- cheatsheetseries.owasp.org
		- Use enforce the use of current transport protocols