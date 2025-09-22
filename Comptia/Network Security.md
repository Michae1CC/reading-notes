Common sec terminology
- Threat - Any potential danger to your network. It can be an attack or natural disaster
- Vuln - A weakness in a network that can be exploited
- Exploit - The method used by attackers to take advantage of a system
- Risk - The potential for loss or damage

CIA Triad
- Confidentiality - Encryption, Access controls
- Integrity - Hashing, digital signatures
- Availability - Ensures data and systems are accessible when we need them

Logical Sec
- Data in Transit - TLS/SSL w/ HTTPs, VPNs, E2E Encryption
- Data at rest - Full disk encryption, File level encryption

IAM
- A framework of policies and technologies to ensure that the right entities have access to the right resources at the right times for the right reasons

Deception Technologies
- Honeypot - Deliberately vuln system designed to attract and detect attackers
- Honeynet - Network of honeypots simulating a real network environment

Network Attack Types
- ARP Spoofing - Attacker sends fake ARP messages to associate their MAC address with the IP of a legit device
- ARP Poisoning - Corrupting the ARP table of devices on a network, directing them to an incorrect MAC address
- Mitigation: DAI on switches

MAC Flooding
- Overwhelm switch's MAC table to force it into broadcast mode

DNS Spoofing
- Impersonates DNS server to send users to mailious site
- Redirects individual users to fake servers and services

DNS Poisoning
- Corrupts DNS cache with false cache

Device Harden
- Securing network device by reducing their attack surface by eliminating vuln
- Includes changing default credentials, disable unnecessary services/daemons, segmentation of networks, ACLs

Network Access Control
- Verify device and user id before granting network access
- Ensure devices meet security requirements before connecting

