
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

