
https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy

- Envoy is an L7 proxy and communication bus designed forlarge modern service oriented architectures

### Terminology

- Host: An Entity capable of network communication (mobile app, server etc)
- Downstream: A downstream host connects to Envoy, send requests and receives responses
- Upstream: An upstream host receives connections and requests from Envoy and returns responses
- Listener: A listener is a named network location (port, unix domain socket, etc.) that can be connected to by downstream clients
- Cluster: A cluster is a group of logically similar upstream hosts that Envoy connects to. Envoy discovers the members of a cluster via service discovery. It optionally determines the health of cluster members via active health checking
- Mesh: A group of hosts that coordinate to provide a consistent network topology
- Runtime configuration: Out of band realtime configuration system deployed alongside Envoy

### Threading Model

- Consists of three main components
	- Main Thread: A single thread that handles various coordination tasks
		- Configuration updates (xDS)
		- Stats flushing
		- Admin interface
	- Worker Threads: A configurable number of threads (controlled by the `--concurrency` flag) that handle actual listening, filtering and forwarding of network traffic
	- File Flusher Thread: A dedicated thread for flushing access logs to disk to avoid blocking the main processing path
- When a connection is accepted by a listener, it is bound to a single worker thread for its entire lifetime

### Listeners

- The Envoy configuration supports any number of listeners within a single process

### Listener Filters

- Envoy's listener filters may be used to manipulate connection metadata
- The main purpose of listener filters are to make adding further system integration functions easier by not requiring changes to Envoy core functionality and also to make interaction between multiple such features more explicit
- Network filters are chained in an ordered list of FilterChain
- Each listener can have multiple FilterChain and an optional default_filter_chain
- Upon, receiving a request, the FilterChain with most specific match criteria is used - otherwise the default is used


