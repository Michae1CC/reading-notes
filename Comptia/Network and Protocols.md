
Network time protocol - used for clock synchronisation. Useful since encryption generally use certification which are usually time-based.

Network Management
- Telnet, secure shell (SSH)

SQL Databases
- Can refer to the protocol that communicates between the server and database

NAT
- Network Layer function
- APIPA addresses (private) `169.254.0.0/16`

DNS
- Application Layer protocol
- TLD - Last part of the domain
- Second level domain - The 'domain'
- We need a DNS server configured on our servers
- TTL configured in the SOA

Topology
- Bus - All the computers connected to a single wire. One wire used to communicate.
- Ring - Connect all the devices to a ring wire
- Star - Every device to connected to a central location
- P2P - Two devices on the network with direct communication with each other

Blank Area Networks
LAN - Numerous device connected to a switch - Layer 2 device
WLAN - Numerous device connected wirelessly
WAN- Take 2 LANs and connect them together
SAN - Store info on hard drives - connect to a network using a high-speed connection

WAN Technologies
- Leased line - A copper (T1) line
- Internet - DSL, Fiber Optic, Satellite, Cable
- MPLS

SDWAN
- A software that sets up routing and some tunnels that allow all the facilities in an organization to communicate in the effective way possible. These are virtual tunnels that connect one building to another without worrying about the actual path through the network. These tunnels are called Multipoint Generic Routing Encapsulation (MGRE)

Software Defined Networks
- Layers:
	- Application Layer - Where admin and devs to add utilities
	- Control Layer - Responsible for moving traffic around and accepting connections
	- Infra Layer - Consists of routers and switches connected with certain policies

SAN connections
- Connecting to SAN Servers:
	- iSCSI - A method we can use to connect our SAN to our servers, uses TCP/IP to make TCP/IP connections between the SAN and servers