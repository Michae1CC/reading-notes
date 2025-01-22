
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