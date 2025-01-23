
---
ELB

Application Load Balancers
- Layer 7 LB on HTTP and/or HTTPS only (no TCP/UDP/TLS)
- HTTP/S always terminated on the ALB - no unbroken SSL. A new connection is made to the app.
- Health checks evaluated app health

ALB Rules
- Rules direct connections which arrive at a listner
- Processed in prority with a defautl catch all
- Rule conditions: host-header, http-header, http request method, query-string etc
- Actions: Forward, redirect, fixed-response, auth

NLB Rules
- Layer 4 LB - TCP, TLS, UDP
- Health checks just check ICP/TCP Handshake
- NLBs can have static IPs - useful for whitelisting
- Forward TCP to instances ... unbroken encryption
- Used with private link to provide services to other VPCs

Connection Draining
- Controls what happens when instances are unhealthy or deregistered
- By default all connections are closed and no new connections are made
- Connections draining allows in-flight requests to complete

Connection Draining Support
- Only supported on Classic LBs

Connection Draining Timeout
- Between 1 and 3600 (default 300)

Deregistration Delay
- Support on ALB, NLB and GWLBs
- Defined of the Target Group - NOT the LB

Deregistration Delay Process/Time
- Stops sending requests to the deregistering targets, existing connections can continue until the completed or deregistration delay is reached
- Default delay is 300sec (0-3600 seconds)

X-Forwarded
- A set of HTTP headers, eg. `X-Forwarded-For: client,proxy1,proxy` (client is the left-most in the list)
- The header is added/appended by proxies/LBs
- Supported for ALB and CLB

Proxy Protocol
- Works at Layer 4
- An additional layer 4 (tcp) header. Works with a range of protocols (including HTTP and HTTPS)
- Supported by NLB and CLB
- End-2-end encryption - e.g. unbroken HTTPS (tcp listener)

LB Security Policies
- A set of ciphers and protocols which are okay to use on a listener
- Protocols is just a method of ensuring secure communications and a cipher is an algorithm
- You control the client => LB policy, the AWS chosen one is used LB => targets is `ELBSecurityPolicy-2016-08`

GWLB
- Help you run 3rd party appliances for inbound and outbound traffic
- Balances across multiple backend appliances
- Traffic and metadata is tunneled using the GENEVE protocol
- Image from: 7:30  https://learn.cantrill.io/courses/1231680/lectures/34269861

GWLB Components
- Endpoints - Where traffic enters/leaves via these endpoints


---
Route53

NS Records
- Delegates a DNS zone to use the given authoritative name servers
- 1:23, https://learn.cantrill.io/courses/1231680/lectures/31339909

A and AAAA records
- Map host name to Ipv4/Ipv6 addresses

CNAME Records
- Alias of one name to another: the DNS lookup will continue by retrying the lookup with the new name, i.e. host to host records
- 3:46, https://learn.cantrill.io/courses/1231680/lectures/31339909

MX Records
- List of mail exchange servers that accept email for a domain
- Lower values in the priority field are high priority, moves to lower priority if the higher are not working
- 7:02, https://learn.cantrill.io/courses/1231680/lectures/31339909

TXT Records
- Originally for arbitrary human-readable _text_ in a DNS record.
- Commonly used to prove domain ownership

DNS TTL
- A numeric value in seconds set on records that indicates the appropriate time records can be cached on resolver servers