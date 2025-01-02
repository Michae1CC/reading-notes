

---
VPC Public Networking
Egress-Only IG route in RT
- Default IPv6 route is `::/0` added to RT with `eigw-id` as target

VPC Public Networking
AWS owned IP addresses
- Owned by AWS and can't be used on premise
- AWS Advertising via Border Gateway Protocol (BGP) and authorised to do so

VPC Public Networking
Privately owned IP addresses
- Addresses you own and control via an Regional Internet Registry
- Advertised by you via BGP with the Regional Internet Registry (RIR) agreement

VPC Public Networking
BYOIP Steps
- 1: Create a RSA Public/Private Key pair and an X509 Cert signed with the Private Key
- 2: Register the key with the Resource Public Key Infrastructure (RPKI) of your RIR (used to verify requests are made by you)
- 3: Create a sign an ROA authorising AWS to advertise public IPs
- 4: Add the certificate to the RDAP address space within the appropriate RIR, allows other to verify advertising of IP range is okay
- 5: Create an authorisation message for AWS with the private key using the `aws ec2 provision-byoip-cidr` cli command
- 6: AWS uses RDAP to verify that you control the IP range

VPC Public Networking
AWS BGP ASNs
- 16509, 14618

VPC Public Networking
Route Origin Authorisation (ROA) is a cryptographically signed object that states with Autonomous System (AS) is authorised to originate a certain prefix "Who can use your IP address space"

VPC Public Networking
BYOIP CIDR limits
- Smallest IPv4 range is /24
- Smallest IPv6 range is /48 (public) and /56 (not publicly advertised)

VPC Public Networking
BYOIP account limits
- Import ranges to be used in *one* specific region at a time
- 5 IPv4 and IPv6 ranges per region per account
- IP ranges cannot be shared with other accounts with AWS Resources Access Manager (RAM)

VPC Public Networking
Bastion Hosts
- Acts as a gatekeeper between a public and private network
- Ingress control - logging, auth, security
- Is a subset of a jumpbox (hardened)

VPC Public Networking
NAT Instance
- Software running on non specialized self-managed EC2
- At end of life

VPC Public Networking
NAT Instance setup
- Speed based on size and type of EC2 instance, can be fixed to manage costs
- Disable source/dest checks
- Works across DX, VPN and Peering connections (Not recommended)

Elastic IP
- A static IPv4 address
- Allocated to your account in a region

VPC Public Networking
NAT Instance Availability
- Scripts can be used to perform RT changes on failure
- Combine NAT/Bastion/Port Forwards into the on instance

VPC Public Networking
NAT Gateway configuration
- Uses an elastic IP which cannot be changed
- NATGW public IP is associated with its private IP with translations managed by the IG
- Private subnets have their own RTs (different to public route tables) where the default IPv4 route is the NATGW

VPC Public Networking
NATGW Availability
- AZ resilience service (HA in a AZ)
- For region resilience - NATGW in each AZ and RT in each AZ with that NATGW as a target
- NATGWs are only accessible within the VPC that they are provisioned into (ie. cannot be used for VPC Peers, Site-to-Site VPN or Direct Connect)

VPC Public Networking
NATGW Speed
- 5Gpbs by default, scales to 45 Gpbs
- To scale beyond 45 Gpbs, split resources into multiple subnets and use multiple NATGWs

VPC Public Networking
NATGW Connection Limits
- 55,000 simultaneous connections to each unique dest
- ErrorPortAllocation if fails
- Can view the number of connections on CW

VPC Public Networking
NATGW Important
- For S3/DynamoDB - use GW Endpoint (no cost)
- Secured using NACLs on the (source or NATGW) subnets and SG on the private instances
- Cannot use SGs on the NATGW itself