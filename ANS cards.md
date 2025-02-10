
---
Misc

Private IPv4 address spaces

The following three blocks of the IPv4 address space for private internets, see https://datatracker.ietf.org/doc/html/rfc1918 :
- 10.0.0.0        -   10.255.255.255  (10/8 prefix)
- 172.16.0.0      -   172.31.255.255  (172.16/12 prefix)
- 192.168.0.0     -   192.168.255.255 (192.168/16 prefix)


Unique Local Address (ULA)

IPv6 addresses in the address range fc00::/7. These addresses are non-globally reachable. Each user of ULAs has a unique address range. AWS the ULA block _fd00:ec2::/32_ within VPCs for local services, such as time sync services or DNS resolvers.



---

Site-Site VPNs

- A logical connection between a VPC and an on-premises network encrypted using IPSec, running over the public internet.
- Quick to provision, usually less than an hour


Site-Site VPNs availability
- Full HA - If you design and implement correctly


Site-Site VPN Components
- Virtual Private Gateway (VGW) - Can be the target on route tables. Something you create and associate with a single VPC, and its the target on one or more route tables.
- Customer Gateway (CGW) - Can refer to either the logical piece of configuration within AWS or the physical device the configuration represents.
- VPN Connections between the VGW and CGW - Store the configuration and it's linked to one VGW and one CGW.


Site-Site Static VPNs
- Static routes are added to the route tables and static networks have to be identified on the VPN connection
- Easy to set up and works anywhere with any combination of routers.


Site-Site Dynamic VPNs
- BGP is configured on both the customer and AWS side using ASN
- Networks are exchanged via BGP
- They can communicate the state of links and adjust routing on the fly
- Multiple VPN connections provide HA and traffic distribution
- Routes can still be added to the RT statically or you can make the entire solution fully dynamic by enabling a route feature called route propagation on the route tables in the VPC

Site-Site VPNs Considerations
- Speed Limitation ~ 1.25 Gbps (imposed by AWS)
- Latency - inconsistent via public internet
- Cost - AWS hourly cost, GB out cost,
- Speed of setup very quick - hours
- Can be used with Direct Connect (DX)