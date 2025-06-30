
### Shift Left Security

- Shift Left Sec is best described as incorporating sec requirements, sec design and sec testing eariler in the Software Development
- The goals of Shift Left Sec
	- Identify and include requirements as part of product requirements
- Improve the overall sec posture of a product/feature
- Fix sec defects earlier in the software development facilitating reduced cost and faster deliver
- Improved collab between dev, sec and operations
- Imbue a sec culture
- Improved compliance with industry and regulatory standards

- Agile development methodologies focus on user stories to define functional and non-functional features
- Incorporating sec users stories puts sec considerations up front
- ASVS lists security requirements for each web app security domain

### Security Design Principles

- Grant users/systems least privilege necessary to complete a task
- For highly sensitive operations, split responsibilities
- No single person has complete control; requires collusion to exploit
- Examples of sensitive operations include:
	- Key and/or HSM management
	- Recording auditing and managing audit security logs
	- Root/admin actions
- Database and routers with default passwords are excellent examples of violating this principle
- Applies more broadly than default passwords
- Applies to any state/configuration that could have a detrimental sec impact
- Multiple layers of sec controls that include admin, physical and technical controls
- Other technical defense in-depth strategies include
- Ensure all access paths are protected assets are checked consistently
- Typically hard to retrofit to existing systems

- An attack surface comprises the totality of the entry points an attacker can use try to compromise a system/application
- These include:
	- HTTP endpoints
	- API endpoints
	- Remote access endpoints
	- Unused ports/services
	- Unused features
- The larger the attack surface the harder it is to secure
- Try to limit external interfaces, remote access
- Simple small designs are easy to verify
- Contrast the simplicity of ACLs in Unix vs Windows

### Open Design

- "A cryptosystem should be secure even if everything is known about it, except the key, is public knowledge"
- A good example, don't obfuscate sensitive data in cookies or passwords in a file

### Usability

- Refers to the useability or psychology acceptability of a user interface - if it's too hard, people will find a way around it
- User interfaces must be designed for ease of use so that the protection mechanisms are used correctly and consistently
- Where the user interface supports a user's metal model of the protection mechanism, mistakes will be minimized

### Sec Requirements

- In addition to normal functional testing, sec requirements driven testing concentrates on validating the sec controls are in place and testing their effectiveness
- Security Misuse Cases
	- Security misuse case testing tests the effectivenes of a sec control
	- It does this by trying break/circumvent the sec control through a technique called fuzzing
	- Fuzzing is about generating and using random, malformed data
	- Best implemented using automated tooling


### Cross Cutting Activities

- Maps to the OWASP SAMM Education and Guidance practice
- Concerned with Training and Awareness and Organization and Culture
- Training and Awareness
	- Provide ongoing training to develop security awareness / skills for all development and operational staff
	- Skill based training should be role focused
	- Security awareness training should be an annual activity; some standards require this
	- Maintain a register of staff training for compliance and merit tracking
	- Extend security awareness training to non developmental/operational staff
- Sec culture will develop as part of widespread sec training

### Policy and Compliance

- Forms the foundation of an Informational Security Management Program

### Strategy and Metrics

- Maps to the OWASP SAMM Strategy and Metrics practice
- Key activities defined are Create and Promote and Measure and Improve

### NIST Secure Software Dev Framework

- Describes a set of high-level practices based on established standards, guidance and existing secure software dev practices
- Not specific to technologies and implementing a risk-based approach to implementing software

### Zero Trust

- A security model that eliminates implicit trust, instead of focusing on the continuous evaluation of explicit access and trust levels. This evaluation is based on id and context
- Zero trust is about authentication and authorization
- Does not assume trust based on granted access in the network
- Assume a hostile network that is that an attacker is present
- Continuously evals trust based on client identity
- A strategic approach for mitigating organisational risk by reducing implicit trust
- Tenets:
	- All data source and computing services are considered resources
	- All communication is secured regardless of network location
	- Access to individual enterprise resources is granted on a per-session basis
	- Monitors and measures the integrity and security posture of all owned and associated assests
	- Collects as much info as possible about the current state of assets, network infra and communications
- Network assumptions
	- The entire enterprise private network is not considered an implicit trust zone
	- Devices on the network may not be owned or configurable by the enterprise
	- Not all enterprise resources are on enterprise-owned infra
- Transition
	- Align zero trust to organizational objectives and risks
	- Effective communication
	- Map organisational risk that can be mitigated with a zero trust principles
	- Architectural plan


- Current TLS version recommended
- When to symmetric vs asymmetric encryption
- Block encryption
- What is a WAF, where is it positioned, what it does
- Signals 
- What is PII
- Sift left
- Design principals
- Not too on frameworks
- Zero trust, what is it