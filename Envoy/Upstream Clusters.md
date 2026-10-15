
https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/upstream

### Cluster Manager

- Envoy's cluster manager manages all configured upstream clusters
- An aggregate cluster allows you to set up failover between multiple upstream clusters that have different configurations


### Service Discovery

- Static: The config explicitly specifies the resolved network name of each upstream network of each upstream host
- Strict DNS: When using DNS service discovery, Envoy will continuously and async resolve the specified DNS targets. Each returned IP address in the DNS result will be considered an explicit host in the upstream cluster. This means that is the query returns three IP addresses, Envoy will assume the cluster has three hosts and all three should be load balanced to
- Logical DNS: Instead of strictly taking the results of the DNS query and assuming that they comprise the entire upstream cluster, a logical DNS cluster only uses the first IP address returned when a new connection needs to be initiated. Optimal for large scale web services that must be accessed via DNS


### Health Checking

- Active health checking can be configured on a per upstream cluster basis
- Three types of health checking
	- HTTP: By default expects a 200 response if the host is healthy.
	- gRPC
	- L3/L4
	- Redis
	- Thrift
- Health checks occur over the transport socket specified for the cluster. This implies that is a cluster is using a TLS enabled transport socket, the health check will also occur using a TLS
- A per-healthchecker log ejection and addition events can optionally be produced by Envoy by specifying a log file path

### Connection Pooling

- HTTP/1.1: Connection pool acquires connections as needed to an upstream host. Requests are bound to connections as they become available, either because a connection is done processing a previous request or because a new connection is ready to receive its first request.
- HTTP/2: Multiplexes multiple requests over a single connection
- Each host in each cluster will have one or more connection pools. If the cluster will have one or more connections pools. If the cluster has a single explicit protocol configured, then the host may have only a single connection pool. However, if the cluster supports multiple upstream protocols, then unless it is using ALPN, one connection

### Load Balancing

- Global load balancing refers to having a single, global auth that decides how load should be distributed between hosts. For Envoy, this would be done by the control plane, which is able to adjust the load applied to individual endpoints by specifying various parameters


### Circuit breaking

- Cluster maximum connections: The maximum number of connections that Envoy will establish to all hosts in an upstream cluster
- Cluster maximum pending requests: The maximum number of requests that will be queued while waiting for a ready connection pool connection. Requests are added to the list of pending requests whenever there aren't enough upstream connections available to immediately dispatch the request


### Load Reporting Service (LRS)

- Provides a mechanism by which Envoy can emit Load Reports to a management server at a regular cadence