
References:
- (customer blog) https://aws.amazon.com/blogs/networking-and-content-delivery/how-iag-accelerated-service-to-service-communication-with-amazon-vpc-lattice/
- (structure) https://aws.amazon.com/blogs/networking-and-content-delivery/streamline-and-secure-access-to-shared-services-and-resources-with-amazon-vpc-lattice/

## Abstract

(done)

## Prerequisites

Before reading on, you should be familiar with the following concepts:
- Amazon's Virtual Private Cloud offering
- Amazon VPC Lattice product and concepts, chiefly: VPC Lattice Services, VPC Lattice Service Associations, VPC Lattice Service Networks


## Previous State Architecture

- For each stage, a single public ALB fronted all customer deployments
- All customer deployments backends
- The ALB had a single domain, each customer deployment sat behind a different path prefix
- To direct customer traffic is directed to respective backends using an ALB Listener Rule with a query string condition
- This was a feasible and easy to maintain solution when the company was still in its start up stages. 

Short comings
- Limited to 100 backends due to ALB listener rules
- Can simply create a new ALBs for each customer deployments, since most customer deployments exist in the same region and we will run into ALB quota limits - this would also become prohibitively expensive to operate

## Solution Overview

- A solution was required to address the following shortcomings of the previous architecture:
	- The solution must scale to publicly front backends numbered in the tens of thousands
	- The solution must offer single tenancy
	- Provide a centralised systems for security and monitoring for traffic entering from a public domain
	- New company cloud offerings that have followed a lift and shift strategy must easily ingrate with the new solution
	- Backends will be spread across multiple accounts
- The following architecture depicts a high-level design showing a Cloudfront distribution to serve as a public endpoint to service backends.
- All Cloudfront traffic is handed to a Nginx cluster that sends traffic to a VPC Lattice Service network which forwards requests to their respective backends

#### Key Components

- Multi-Tenant distribution to serve all public facing traffic. Each customer tenant is provided with their own SaaS domain. The tenant may then access each of their SaaS services through their domain
- Nginx Cluster used to modify request structure and to proxy the request to the correct VPC Lattice Service
- VPC Lattice Service Network layer to route requests to their respective backend

#### How it works

- Talk about cfn to provision app group resources

#### Accessing a HTTPS service

- All customer deployments and ECS services which Lattice Service can directly integrate with