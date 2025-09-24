
Firewall
- Prevent a barrier between internal and external networks

Network appliances and storage
- Wireless LAN controller - A centralised server that manages all of the access points.
- Load Balancer - Distribute load across multiple servers without the users knowing.
- Quality of Service - Used to make the quality of voice/video calls better. Add info to packets to prioritise certain connections. Lives between L2 and L3 of OSI model.
- NAS/SAN - Allows central data storage which can be accessed over a network. Uses a Fibre Channel Switch.

3 Three Tier Model
- Access Layer - Where we have our devices connecting to our Layer 2 network through switches or wireless access points. We create policies to only allow the devices we want on the network and exclude devices we don't want on the network.
- Distribution Layer - Will have routing policies to route traffic between them.
- Core Layer - Performs the bulk of our routing from one location to another location.
- Often Distribution and Core Layers are lumped together

Map Hardware

- Data Link Layer
	- Frame
	- Switch
	- Ethernet
	- MAC Address
	- ARP
- Network Layer
	- Packet
	- Router
	- Load Balancer
	- IP/IP Address
	- IPSec - A protocol that enables VPNs to work at the network layer
- Transport Layer
	- Segment
	- Firewall
	- TCP/UDP
	- Port Numbers
- Application Layer
	- SSH
	- SQL
	- LDAP
	- NTP
	- DNS
	- DHCP
	- FTP
	- HTTP/s