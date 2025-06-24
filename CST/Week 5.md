
## Sec Misconfiguration

- Relates to misconfiguration of an app
- Typical examples:
	- Default accounts still enabled
	- Unused ports
	- Unpatched libs and OS with known sec vulnerabilities
	- Missing the 'secure' attribute for HTTPs connections use secure vulnerabilities
	- Missing 'HttpOnly' flag for sensitive flags
	- Cleartext message of sensitive information in a cookie
	- use of hard coded sec values
	- Runtime error messages contain sensitive system information
	- Permissive cross domain policies
	- Too much information provided in response to auth failures

#### Attack Scenarios

- Directory listing is not disabled on a server allowing an attacker to download sensitive files

#### Prevention

- Adopt industry standard hardening guides
- Develop a repeatable preferable automated process for applying hardening guides
- Minimize services / apps / components on a server or disable unwanted


### Vulnerable and Outdated components

- Applies to the entire tech stack and includes operating systems, libs, frameworks and middleware

#### Typical Vuln

- Not maintaining a software bill of materials means that there may be components that are outdated, unsupported and/or unpatched. You can't fix what you don't know.
- Unpatched components may contain a Remote Code execution vuln which are the most serious. RCE vuln allows attackers to execute arbitrary code on remote servers.
- Log4J 2.x - a widely used open-source logging library that contained RCE vuln which allowed attackers to execute code remotely via attacker-controlled LDAP servers
- Wordpress 5.0.0 allowed attackers to execute arbitrary code uploading an image file that contained PHP code in Exif metadata

#### Prevention

- Develop a robust management process for all system components
- Patch priorities driven by sec risk; that is by the severity of the vuln
- Develop an automated process to scan system components for vuln and out of date releases
- Use vuln scanners such as Nessus, OpenVAS, etc
- Virtual patching

- CVSS - Common Vulnerability Scoring System is a measure of the severity
- Virtual Patching - Essentially using a WAF to block attack vectors; applications are not modified/patched
- Allows zero-day vuln to be mitigated until a formal patch is made available


### Identification and auth failures

- Addresses weak or vuln identification and auth mechanisms that are vulnerable to a range of auth related attacks
- Improper validation of certificate
- Use of weak or well known passwords
- vulnerable password recovery mechanisms
- expose session identifier in URL
- Failure to invalidate session identifier after session termination
- session reuse

#### Attack Scenario

- An application does not enforce limits/constraints on failed user login attempts that allows an attacker to perform credential stuffing attacks.

#### Prevention

- Use mfa auth where possible
- Implemented weak password checks against a list of bad passwords
- Implemented password checks against compromised password list
- Enforce password length and complexity as per NIST 800-63b guidelines
- Increasing delays between failed attempts
- Ensure session ids are invalidated after termination/logout