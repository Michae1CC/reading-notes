Private Cloud - Business owns the hardware
Public Cloud - Cloud provider owns the hardware
Hybrid Cloud - Infra shared between public and private cloud. For example, a data center on-premise that their admins support and manage, but because of the flexibility of cloud resources, we add a public cloud to our business resources.

Cloud Service Models
- Infra as a service - Becomes a combination or virtual servers, and those virtual servers are what admins choose (CPU, mem, OS).
- Platform as a service - Allows the cloud provider to manage hardware and Operating Systems.
- Software as a Service - Let the Service provide manage everything apart from application.

Cloud Access and security
- Direct Connect - A single mode fibre optic cable from our organization to our cloud service provider. A high speed connection which gives a lot of control over the traffic coming to and from that cloud.
- VPN Tunnel - Connects our organisation to our virtual private cloud
- Network security list - A set of rules that's going to exist before a subnet. It will allow an admin to configure rules for the entire subnet - similar to an AWS NACL.
- Network security group (AWS security group) - Applied to the network interface card. Stateful

Data Centre Basics
- VPN Tunnels don't offer all of the features that we actually need in order to run our data center effectively, for instance, the ability to pass multicast traffic. VPNs do a great job of unicast traffic from one device to another but no multicast. Multicast traffic is used to manage the data centre itself.
- GRE Tunnel - Allows for multicasting traffic. Doesn't do encryption, we can encrypt our GRE tunnel with IPsec. Allows us to encapsulate VXLAN traffic
- VXLAN - Allows us to connect two data centers together as a layer 2 connection. From an application's perspective (up at the application layer) our data center, which is in two locations, appears to be in the one single location.
