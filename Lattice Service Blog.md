
References:
- (customer blog) https://aws.amazon.com/blogs/networking-and-content-delivery/how-iag-accelerated-service-to-service-communication-with-amazon-vpc-lattice/
- (structure) https://aws.amazon.com/blogs/networking-and-content-delivery/streamline-and-secure-access-to-shared-services-and-resources-with-amazon-vpc-lattice/

## Abstract

(done)

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

This provided a very cost efficient and easily maintainable solution while only a select number of customers had cloud deployments. With an increasing demand for SaaS offerings by customer, a number of shortcoming have become evident:
- There is a limit of 100 Load Balancer Conditions per Application Load Balancer
- Services must be created in the same VPC as the Application Load Balancer
- All customers are forced into a multi-tenancy deployment

## Solution Overview

**DPs**
- A new solution as a web request entry point which was given the following requirements to address to shortcomings of the old architecture:
	- The solution must scale to publicly front backends numbered in the tens of thousands
	- The solution must offer single tenancy
	- Provide a centralised systems for security and monitoring for traffic entering from a public domain
	- New company cloud offerings that have followed a lift and shift strategy must easily ingrate with the new solution
	- Backends will be spread across multiple accounts
- The following architecture depicts a high-level design showing a Cloudfront distribution to serve as a public endpoint to service backends.
- All Cloudfront traffic is handed to a Nginx cluster that sends traffic to a VPC Lattice Service network which forwards requests to their respective backends

**Paragraphed**
A new solution for a public web request entry point was given the following requirements to address the shortcoming of the old architecture:
- Must be able to front an order of ten's of thousands of private backends
- Must offer a single tenancy option
- New Deswik cloud offerings that have followed a lift and shift strategy must easily ingrate with the new solution
- Should be able to route requests to VPCs within a different account

#### Key Components

- Multi-Tenant distribution to serve all public facing traffic.
- Nginx Cluster used to modify request structure and to proxy the request to the correct VPC Lattice Service
- VPC Lattice Service Network layer to route requests to their respective backend

#### How it works

- Each customer tenant is provided with their own SaaS domain. The tenant may then access each of their SaaS services through their domain
- Talk about cfn to provision app group resources
- Talk about RAM to share LSN and register LS
- Talk about kvs mapping and Nginx proxy

#### Accessing a HTTPS service/Traffic Flows

- Customers will use their own domain and be directed to thier Cloudfront Distribution Tenant
- A Cloudfront will use the domain and path prefix to look up the Lattice Service domain name the request is intended for and set the domain in a custom header
- The Nginx cluster will take the domain from the custom header and proxy the request to the intended Lattice Service
- Finally, the Lattice Service will forward the request to its corresponding backend, for example an ECS service