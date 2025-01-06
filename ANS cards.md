
---
VPC-Endpoints
Privatelink
- AWS PrivateLink establishes private connectivity between virtual private clouds (VPC) and supported AWS services, services hosted by other AWS accounts, and supported AWS Marketplace services.
- You do not need to use an internet gateway, NAT device, AWS Direct Connect connection, or AWS Site-to-Site VPN connection to communicate with the service.
- Include this image: https://docs.aws.amazon.com/images/vpc/latest/userguide/images/use-cases.png

Privatelink Availability
-  A highly available solution can be achieved by deploying multiple endpoints, one in each availability zone inside the consumer VPC.

Privatelink protocol support
- Supports IPv4 and TCP only (IPv6 is not supported)

Privatelink networking support
- Private DNS is supported (e.g. overriding the default public DNS with one which points at the an interface endpoint for the PrivateLink service)
- You can access PrivateLink services over direct connect, site-to-site VPN and a VPC pier

Gateway Endpoints
- Provide private access to S3 and DynamoDB
- Works by adding a route to the RT which points to the GW Endpoint
- Associated with one or more subnets

Gateway Endpoints access
- Endpoint policy is used to control what it can access
- Can't access cross-region services

Gateway Endpoints use cases
- Prevent Leaky Buckets - S3 Buckets can be set to be private only by allowing access only from a GW endpoint

Gateway Endpoints Availability
- Regionally resilient by default, don't need to worry about AZ placement