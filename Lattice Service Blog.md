
References:
- (customer blog) https://aws.amazon.com/blogs/networking-and-content-delivery/how-iag-accelerated-service-to-service-communication-with-amazon-vpc-lattice/
- (structure) https://aws.amazon.com/blogs/networking-and-content-delivery/streamline-and-secure-access-to-shared-services-and-resources-with-amazon-vpc-lattice/

## Abstract

Deswik is a global mining software company delivering a growing portfolio of cloud-hosted SaaS products under a unified platform. As the number of applications scaled, their existing Application Load Balancer-based routing was approaching listener limits and growing increasingly complex for application teams to manage. To address this, Deswik built Frontdoor — a centralized, data-driven HTTP/S entry layer for all incoming traffic to web applications. Built on Amazon CloudFront and Amazon VPC Lattice, Frontdoor provides a single, standardized ingress point that abstracts away the routing and infrastructure details from individual product teams. It supports a wide range of deployment targets across varying AWS account structures and tenancy models, while centralizing SSL management, request logging, and a Web Application Firewall.

## Prerequisites

Before reading on, you should be familiar with the following concepts:
- Amazon's Virtual Private Cloud offering
- Multi-tenant CloudFront distributions and CloudFront Functions
- EC2, Elastic Load Balancers and Amazon Auto-Scaling Groups
- Amazon VPC Lattice product and concepts, namely: VPC Lattice Services, VPC Lattice Service Associations, VPC Lattice Service Networks

## Previous State Architecture

Previously, Deswik used a single Application Load Balancer as a public entry point for customer web requests. In this implementation, all customers would access a backend deployed behind the Application Load Balancer by using a unique path prefix for said backend. The path prefix is then used by an Application Load Balancer Listener Rule with a query string condition which directed requests to the appropriate backend.

Todo: PREVIOUS SOLUTION IMAGE

This provided a very cost efficient and easily maintainable solution while only a small number of customers required cloud deployments. With an increasing demand for SaaS offerings by Deswik customers, a number of shortcomings became evident:
- There is a limit of 100 Load Balancer Conditions per Application Load Balancer.
- Services must be created in the same VPC as the Application Load Balancer.
- All customers are forced into a multi-tenancy deployment.

## Solution Overview

A new solution for a public web request entry point was given the following requirements to address the shortcomings of the old architecture:
- Must be able to front private backends numbered in the tens of thousands
- Must offer a single tenancy option.
- New Deswik cloud offerings that have followed a lift and shift strategy must easily integrate with the new solution.
- It should be able to route requests to VPCs within a different account.
The following architecture depicts a high-level design showing a CloudFront distribution to serve as a public endpoint to expose service backends via a VPC Lattice Service Network.

Todo: NEW SOLUTION IMAGE

#### Key Components

1. A Multi-Tenant Distribution to act as a public endpoint for all backend traffic. A Distribution Tenant is created for each customer with each Tenant Distribution being provided its own domain.
2. An Nginx cluster connects the Distribution to the VPC Lattice Service Network. The cluster is used to modify the request payload and proxy requests to the correct VPC Lattice Service. The Nginx cluster targets the VPC Lattice Service to proxy requests to by using the default domain name that VPC Lattice Service prescribes.
3. A single VPC Lattice Service Network is shared from a central networking account to all accounts with backends that need to register to Frontdoor. These accounts are able to create VPC Lattice Services that are associated with the shared VPC Lattice Service Network.
4. VPC Lattice Service abstracts deployments across different accounts and VPCs and shuttles requests to the appropriate backend. 

#### Request Flow

The following diagram depicts how traffic is moved from a customer request to its intended backend. The backend may be any form of compute that the VPC Lattice Service product supports as a target, e.g., ECS.

Todo: REQUEST FLOW IMAGE

- The client performs a DNS lookup for a custom domain name provided to their organization.
- The client will access a product deployment for their organization by prefixing HTTP request paths with the product identifier, for example `https://example.deswik.app/product1`.
- Each Distribution has a CloudFront Function which contains a key-value-store mapping from tenant subdomain and product identifier to the VPC Lattice Service domain for the product deployment. Once the request reaches the distribution, the CloudFront Function adds a custom header of the VPC Lattice Service domain the request is intended for.
- Once the Nginx cluster receives the request, it proxies the request to the intended VPC Lattice Service using the aforementioned custom header. Amazon Route 53 is used by Nginx to resolve the destination address, that is, the VPC Lattice Network VPC Association.
- The VPC Lattice Service performs most of the routing heavy lifting by forwarding the request to the intended backend which is deployed in a separate VPC and AWS account.

_TODO_: Talk about how much better we can scale with this approach

There is a soft quota maximum of 10,000 distribution tenants for each account and 2,000 VPC Lattice Services per region allowing us to scale to backends in the tens of thousands across the organization. This also allows backend deployments to register to the central Lattice Service Network from a different VPC within another account.

## Conclusion

By building Frontdoor on Amazon CloudFront and Amazon VPC Lattice, Deswik replaced a single Application Load Balancer that was approaching its listener and rule limits with a centralized, standardized ingress layer that scales to backends in the tens of thousands. The new architecture removes the constraints of the previous design: backends no longer need to live in the same VPC as the entry point, deployments can be routed across separate VPCs and AWS accounts, and customers can be served through either multi-tenant or single-tenant models. Just as importantly, product teams are insulated from the underlying routing and infrastructure details, allowing them to onboard a new deployment by simply registering a VPC Lattice Service with the shared service network.

For organizations running a growing portfolio of services across many accounts and VPCs, Amazon VPC Lattice provides a powerful abstraction layer for delivering secure, scalable, and consistent service-to-service connectivity without the operational overhead of managing IP-based networking. To learn more and get started, see the Amazon VPC Lattice documentation.