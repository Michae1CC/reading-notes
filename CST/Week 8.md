
### Threat Modelling

- A structured process used to identify, assess and mitigate potential sec threats to a system or app
- Analyses a system from an adversarial perspective
- The threat model doc is a living doc and should be updated whenever the landscape changes
- Should be performed during the design phase

#### Identify Risks early

- Helps to assess and reduce the attack surface
- Enhance the security posture
- Reducing the cost by fixing vuln earlier in the SDLC

- Improved Visibility of the application
- Increased Security awareness
- Supports Industry Compliance
- Assets - Data that needs to be protected
- Attack Surface - Sum of all attack vectors
- Attack Vector - A pathway or method that an adversary uses to exploit a vuln
- Entry Point - An interface to the outside world
- Threat - What an adversary might try to do to a system
- Trust Boundary - Regions in a system that have different trust levels
- Counter measure - Safeguards or controls used to mitigate a threat

### Risk Management

- Describes the likelihood and potential loss/damage that may occur when a treat is realized
- Risk = Likelihood * Impact
- Apply security controls and or redesigning an application are typically use to reduce risk
- Outsource deployment to a third-party or take out cyber security insurance
- Avoid - Remove a feature/app
- Accept - If the risk falls below a threshold acceptable to the business then the risk can be accepted

### Design Patterns

- Proven solutions to common design problems
- Code reuse
- Separation of concerns

- Auth pattern
	- Create a centralised auth component that authenticates users and encapsulates the auth mechanism
- Secure Logger Pattern
	- Logs messages in a secure manner so they do not contain sensitive data and cannot be altered or deleted

### Language Security

- Security Design Patterns play an important role in developing secure software/systems
- Conduct all validation on a trusted system
- There should be a centralized input validation routine for the application
- Validate all client provided data before processing, including parameters URLs and HTTP header content
- Use a standard, tested routine for each type of output encoding
- Contextually sanitise all output from un-trusted data queries
- Sanistise all output of untrusted data
- Require auth for all pages and resources, except those specifically intended to be public
- Use approved rand number generators