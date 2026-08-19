
References:
- (customer blog) https://aws.amazon.com/blogs/networking-and-content-delivery/how-iag-accelerated-service-to-service-communication-with-amazon-vpc-lattice/
- (structure) https://aws.amazon.com/blogs/networking-and-content-delivery/streamline-and-secure-access-to-shared-services-and-resources-with-amazon-vpc-lattice/

## Abstract

Deswik is a global mining software company delivering a growing portfolio of cloud-hosted SaaS products under a unified platform. As the number of applications scaled, their existing Application Load Balancer-based routing was approaching listener limits and growing increasingly complex for application teams to manage. To address this, Deswik built Frontdoor — a centralised, data-driven HTTP/S entry layer for all incoming traffic to web application. Built on Amazon CloudFront and AWS VPC Lattice, Frontdoor provides a single, standardised ingress point that abstracts away the routing and infrastructure details from individual product teams. It supports a wide range of deployment targets across varying AWS account structures and tenancy models, while centralizing SSL management, request logging, and a Web Application Firewall.

## Prerequisites

Before reading on, you should be familiar with the following concepts:
- Amazon's Virtual Private Cloud offering
- Multi-tenant Cloudfront Distributions and Cloudfront Functions
- EC2, Elastic Load Balancers and Amazon Auto-Scaling Groups
- Amazon VPC Lattice product and concepts, namely: VPC Lattice Services, VPC Lattice Service Associations, VPC Lattice Service Networks

## Previous State Architecture

**DPs**
- For each stage, a single public ALB fronted all customer deployments
- All customer deployments backends
- The ALB had a single domain, each customer deployment sat behind a different path prefix
- To direct customer traffic is directed to respective backends using an ALB Listener Rule with a query string condition
- This was a feasible and easy to maintain solution when the company was still in its start up stages. 

Short comings
- Limited to 100 backends due to ALB listener rules
- Services had to be created in the same VPC as the ALB
- Can simply create a new ALBs for each customer deployments, since most customer deployments exist in the same region and we will run into ALB quota limits - this would also become prohibitively expensive to operate

**Paragraphed**
Previously, Deswik used a single Application Load Balancer as a public entry point for customer web requests. In this implementation, all customers would access a service deployed behind the Application Load Balancer by using a unique path prefix for said service. The path prefix is then used by an Application Load Balancer Listener Rule with a query string condition which directed requests to the appropriate backend.

This provided a very cost efficient and easily maintainable solution while only a small number of customers had cloud deployments. With an increasing demand for SaaS offerings by Deswik customers, a number of shortcoming become evident:
- There is a limit of 100 Load Balancer Conditions per Application Load Balancer
- Services must be created in the same VPC as the Application Load Balancer
- All customers are forced into a multi-tenancy deployment

## Solution Overview

**DPs**
- A new solution as a web request entry point which was given the following requirements to address to shortcomings of the old architecture:
	- Must scale to publicly front backends numbered in the tens of thousands.
	- Must offer single tenancy.
	- Provide a centralised systems for security and monitoring for traffic entering from a public domain.
	- New company cloud offerings that have followed a lift and shift strategy must easily integrate with the new solution.
	- Backends will be spread across multiple accounts.
- The following architecture depicts a high-level design showing a Cloudfront distribution to serve as a public endpoint to service backends.
- All Cloudfront traffic is handed to a Nginx cluster that sends traffic to a VPC Lattice Service network which forwards requests to their respective backends

**Paragraphed**
A new solution for a public web request entry point was given the following requirements to address the shortcoming of the old architecture:
- Must be able to front private backends numbered in the ten's of thousands
- Must offer a single tenancy option.
- New Deswik cloud offerings that have followed a lift and shift strategy must easily integrate with the new solution.
- Should be able to route requests to VPCs within a different account.
The following architecture depicts a high-level design showing a Cloudfront distribution to serve as a public endpoint to expose service backends via a VPC Lattice Service Network.

Todo: SOULTION IMAGE

#### Key Components

**DPs**
- Multi-Tenant distribution to serve all public facing traffic.
- Nginx Cluster used to modify request structure and to proxy the request to the correct VPC Lattice Service
- VPC Lattice Service Network layer to route requests to their respective backend

**Paragraphed**
1. A Multi-Tenant Distribution to act as a public endpoint for all backend traffic. A Distribution Tenant is created for each customer with each Tenant Distribution being provided its own domain.
2. An Nginx cluster connects the Distribution to the VPC Lattice Service Network. The cluster is used to modify the request payload and proxy requests to the correct VPC Lattice Service. The Nginx cluster targets the VPC Lattice Service to proxy requests to by using the default domain name that VPC Lattice Service prescribes.
3. A single VPC Lattice Service Network is shared from a central networking account to all accounts with backend that need to register to Frontdoor. These accounts are able to create VPC Lattice Service that are associated with the shared VPC Lattice Service Network.
4. VPC Lattice Service abstracts deployments across different accounts and VPCs and shuttles requests to appropriate backend. 

#### How it works/Request Flow

**DPs**
- Each customer tenant is provided with their own SaaS domain. The tenant may then access each of their SaaS services through their domain
- Talk about cfn to provision app group resources?
- Talk about RAM to share LSN and register LS
- Talk about kvs mapping and Nginx proxy

**Paragraphed**
The following diagram depicts how traffic is moved from a customer request to its intended backend. The backend maybe any for of compute that the VPC Lattice Service product supports as a target, eg: ECS.

Todo: REQUEST FLOW IMAGE

- The client performs a DNS lookup for a custom domain name provided to their organization.
- The client will access a product deployment for their organization by prefixing HTTP request paths with the product identifier, for example `https://example.deswik.app/product1`.
- Each Distribution has a Cloudfront Function which contains a key-value-store mapping from tenant subdomain and product identifier to the VPC Lattice Service domain for the product deployment. Once the request reaches the distribution, the Cloudfront Function adds a custom header of the VPC Lattice Service domain the request is intended for.
- Once the Nginx cluster receives the request, it proxies the request to the intended VPC Lattice Service using the aforementioned custom header. Route53 is used by Nginx to resolve the destination address, that is, the VPC Lattice Network VPC Association.
- The VPC Lattice Service performs most of the routing heavy lifting by forwarding the request to the intended backend which is deployed in a separate VPC and AWS account.

_TODO_: Talk about how much better we can scale with this approach

There is a soft quota maximum of 10,000 distribution tenants for each account and 2,000 VPC Lattice Services per region allowing us to scale to backends in the tens of thousands across the organization. This also allows backend deployments to register to the central Lattice Service Network from a different VPC within another account. 

#### Accessing a HTTPS service/Traffic Flows

**DPs**
- Customers will use their own domain and be directed to thier Cloudfront Distribution Tenant
- A Cloudfront will use the domain and path prefix to look up the Lattice Service domain name the request is intended for and set the domain in a custom header
- The Nginx cluster will take the domain from the custom header and proxy the request to the intended Lattice Service
- Finally, the Lattice Service will forward the request to its corresponding backend, for example an ECS service