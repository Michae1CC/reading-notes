
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

ID and auth failures
- Weak or vulnerable identification and authentication mechanisms that are vulnerable to a range of authentication related attacks

ID and auth failures Typical Vuln
- Use of weak or well-known passwords
- Vulnerable password recovery mechanisms

ID and auth failures Attack Scenarios
- An application does not enforce limits/constraints on failed user login attempts that allows an attacker to perform credential stuffing attacks
- Session fixation is where an attacker forces a victim to use a specific session ID when connecting to a web server

ID and auth failures Prevention
- Use MFA
- Implement weak/compromised password attacks

Software and Data Integrity Failures
- Refer to the lack of provenance and whether a software or data artefact has been tampered since its original creation

Software and Data Integrity Failures Typical Vuln
- Insufficient verification of data origin and authenticity
- Missing support for integrity check

Software and Data Integrity Failures Prevention
- Use digital signatures to verify the source and integrity of software and data artefacts
- Use integrity checks/signatures when sending serialised data to untrusted clients

