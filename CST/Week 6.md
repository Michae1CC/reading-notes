## Software and Data integrity

- Failures refer to the lack of provenance and whether a software or data artefact has been tempered since its original creation
- Applies to libraries, plugins, apps, CDNs
- Root causes typically comprise the use of untrusted sources and/or lack of in-transit protection for the software or data artefact

### Typical vuln

- Missing support for integrity check
- Untrusted search path
- Deserialisation of untrusted data
- Reliance of cookies without validation and integrity checking
- Inclusion of web functionality from untrusted sources

### Examples Attacks

- Software updates that are not signed
- Spring Framework MVC auto-binding of HTML form fields to pre-defined Java POJO classes enabled changes to non-editable fields

### Prevention

- Verify trustworthiness of source repos possibly locally hosted
- Hardened CI/CD pipeline for proper use


### Security Logging and Monitor Logging Failures

- Security logging and monitoring failures relate to the lack of logging, especially application logging, securing the logs against tampering and crucially the lack of log monitoring
- storing logs locally
- missing sec logs, insufficient log data and lack of active monitoring prevent the timely detection and response to cyber attacks
- Missing sec incident/breach response playbook
- Log injection - Logs that contain externally provide data allow an attacker to inject/modify existing log records or possible execute code

### Prevention

- Ensure all security related events are logged
- Logs should contain the 5 w's, who what when where and why
- Use a trusted time source to ensure the correct chronological order of events; adopt a standardized log format
- Use appropriate design patterns for all log messages
- Ensure 24/7 log monitoring is in place. Use SIEM and SAR and automated alert mechanisms where possible


### Server Side Request Forgery

- SSRF is an attack vector that exploits an app to interact with the internal/external network or the machine

### Typical vuln

- Failed web server request validation
- Poor internal network segmentation; flawed assumptions that internal resources that internal resources are always trusted
- Lack of secure and authenticated communication between internal systems

### Attack Scenarios

- Redirect back-end systems to malicious endpoints

#### Prevention

- Validate user input correctly
- Do not allow requests to private IP addresses
- Validate response data before returning