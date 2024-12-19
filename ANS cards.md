
- Static NAT
	- Static (map) network address translation (NAT) provides a one-to-one mapping of private IP addresses to public IP addresses.
	- Used by AWS IGW

- Dynamic NAT
	- Converts private IP addresses to public IPs through a pool. 
	- The pool is registered public IP addresses on a first-come, first-served basis.

- PAT or NAPT
	- Multiple devices on a local area network (LAN) to share a single public IP address by assigning different ports to private sockets.
	- Used by AWS NATGW

- Subnet notation
	- xxxx.xxxx.xxxx.xxxx/16 indicates the subnet prefix is the first 16 bits with a subnet mask of 1111.1111.0000.0000

-  VLAN 802.1Q
	- Allows multiple VLANS to operate over the same L2 physical network. Each has a separate broadcast domain and isolated from all others

- VLAN 802.1AD ethernet frame (QinQ)
	- QinQ allows ISPs or carriers to use VLANS across their network, while carrying customer traffic which might also be using multiple VLANS.

- Switch Access ports
	- Access ports communicate with stations with stations using normal ethernet, i.e. no VLAN tagging

- Switch TRUNK ports
	- A TRUNK PORT is a connection between two 802.1Q capable devices, it carriers all VIS with tagging

- Custom VPC attributes
	- Ipv4 Private CIDR Blocks and public IPs
	- 1 Primary Private Ipv4 CIDR Block (min /28, max /16)
	- Optional secondary Ipv4 Blocks
	- Optional single assigned Ipv6 /56 CIDR Block

- VPC DNS configuration
	- DNS IP is Base IP + 2
	- enableDnsHostnames  - give instances DNS Names
	- enableDnsSupport - enabled DNS resolution in the VPC

- Public vs Private services
	- Public services is accessed using public networking endpoints, eg S3
	- A Private AWS service is something which runs in a VPC, eg RDS

- Subnet attributes
	- Must belong to a single VPC in a single AZ
	- CIDR range cannot overlap with other subnets
	- Optional Ipv6 CIDRs
	- Can communicate with other subnets by default

- Reserved Subnet IPs
	- 5 reserved in total
	- First address in the CIDR, use to represent the subnet
	- Network + 1, used by VPC router
	- Network + 2, used for DNS
	- Network + 3, reserved
	- Last address, used for broadcasting

- DHCP
	- Auto configuration for network resources
	- Starts with a L2 broadcast to get info from DHCP server
	- Device obtains IP address, subnet mask and default gateway
	- Configures with DNS servers to use

- DHCP option sets
	- Immutable
	- Associating a new option set is immediate but requires a DHCP renew
	- Can be used to configure NTP servers

- DHCP option set associations
	- Each option set me be associated with 0 or more VPCs
	- Each VPC can have a maximum of one option set

- VPC Router General Info
	- High available across all AZs in that region
	- Routes between subnets and external networks
	- Interface in every subnet "subnet + 1 address"

- VPC Router configuration
	- Every VPC has a default main route table
	- Custom route tables can replace main table
	- Subnets are associated with ONE RT (main or custom)

- NACL
	- Stateless firewalls both request and response need individual rules
	- Rules are processed in order, lowest rule number first with * as an implicit DENY
	- Can be associated with many subnets

- NACL with subnets
	- Connections within a subnet aren't impacted by NACLs
	- NACLs filter traffic crossing the subnet boundary inbound or outbound
	- Each subnet can only have one NACL (default or custom)

- NACL limitations
	- Can only explicitly allow or deny
	- IPs/CIDR, ports and protocols - no logical resources
	- Can only be assigned to subnets

- Security Groups
	- Stateful firewalls
	- Attached to ENI's, not instances
	- Cannot explicitly block traffic

- Router
	- A computer that forwards data packets between IP networks

- VPC Flow logs
	- Captures metadata (eg. src/dest IPs), not contents
	- Attached to all VPCs ENIs, all subnet ENIs or single ENIs

- VPC flow log logging dest
	- S3 or cloudwatch logs

- VPC flow log protocol numbers
	- ICMP -> 1
	- TCP -> 6
	- UDP -> 17

- Public Ipv4 addressing on services
	- Services never have Ipv4 addressing configured on them within a VPC
	- Always handled by IGW

- Public Ipv6 addressing
	- BYO IPv6 or be assigned a /56 range by AWS
	- All publicly routable, separate routes to Ipv4
	- Addressing can be migrated to Ipv6

- IGW associations with VPC
	- You can have both a IGW and egress only IGW on a VPC

- VPC Traffic Mirroring
	- Traffic to ENIs are mirrored to different targets
	- Filters - rules to define what is mirrored
	- A session is the (src, target, filters)

- VPC Mirroring technology
	- Uses VXLAN to encapsulate L2 and send it over L3 + GENEVE if GWLB
	- Encapsulated data is running via UDP port 4789

- VPC Mirroring limitations
	- same/different VPC, same/different account but must be in the same region
	- Targets must be: ENI, NLB, GWLB (UDP 4789)
	- The same ENI cannot be both the src and the target

- IGW NAT
	- Highly available and scalable by default
	- (Ipv4) Uses 1:1 static NAT
	- (Ipv6) Routes Ipv6 packets in and out of a VPC



