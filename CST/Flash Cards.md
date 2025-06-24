
Security Misconfiguration
- Relates to misconfiguration of an application and/or server that may be exploited to launch an attack

Security Misconfiguration Typical Vulnerabilities
- Missing ‘HttpOnly’ flag for sensitive cookies
- Missing appropriate security hardening across any part of the application stack or improperly configured permissions on cloud services

Security Misconfiguration Prevention
- Develop a repeatable, preferably automated, process for applying hardening guides
- Enforce and automate the use of security attributes/flags/directives for HTTP/S connections

Vulnerable and Outdated Components
- Using components that significantly trail current releases, have known vulnerabilities, or are no longer supported
- Applies to the entire technology stack and includes operating systems, libraries, frameworks and middleware, server software, and databases

Vulnerable and Outdated Components Typical Vulnerabilities
- Unpatched components may contain a Remote Code Execution (RCE) vulnerability, which are the most serious vulnerabilities

Vulnerable and Outdated Components Typical Vulnerabilities
- WordPress 5.0.0 allowed attackers to execute arbitrary code by uploading an image file that contained PHP code in its Exif metadata
- Log4J 2.x – a widely used open-source logging library that contained an RCE vulnerability which allowed attackers to execute code remotely via attacker-controlled LDAP servers

Vulnerable and Outdated Components Prevention
- Patch priorities driven by security risk; that is by the severity of the vulnerabilities
- Develop an automated process to scan system components for vulnerabilities and out of date releases

Vulnerable and Outdated Components Prevention - Virtual Patching
- Using a WAF to block attack vectors; applications are not modified/patched
- Allows zero-day vulnerabilities to be mitigated until a formal patch is made available, or critical patch updates performed during normal patch cycles
