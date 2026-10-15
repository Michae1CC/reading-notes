https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/http

### HTTP Connection Manager

- This filter translates raw bytes into HTTP level messages and events

### HTTP Filters

- Filters can be written that operate on HTTP level messages without knowledge of the underlying physical protocol
- Three types of filters:
	- Decoder: Invoked when the connection manager is decoding parts of the request stream (headers, body, trailers)
	- Encoder: Invoked when the connection manager is about to encode parts of the response stream (headers, body, and trailers)
	- Decoder/Encoder: Invoked both when the connection managers is decoding parts of the request stream and when the connection manager is about to encode parts of the response stream
- Filtering ordering matters
```yaml
http_filters:
  - A
  - B
  # The last configured filter has to be a terminal filter, as determined by the
  # NamedHttpFilterConfigFactory::isTerminalFilterByProto(config, context) function. This is most likely the router
  # filter.
  - C
```
- Decoder filters invokes in the order: A,B,C

### HTTP Routing

- Useful for handling edge traffic as well as for building a service Envoy mesh
- The router takes an incoming HTTP request, matches is to an upstream cluster, acquires a connection pool to a host in the upstream cluster and forwards the request
- path, prefix and header matching
- cookie matching
- path, prefix and host re-writing
- request redirection
- request timeouts, retries
- Scoped routing enables Envoy to put constraints on search space of domains and route rules
- The configuration for the HTTP connection manager owns a route table that is used by all configured HTTP filters
- Envoy allows retries to be configured both in the route configuration as well as for specific requests via request headers
	- Maximum number of retries
	- Retry conditions
	- Retry budgets
	- Host selection retry plugins
- Envoy supports direct responses. These are preconfigured HTTP responses that do not require proxying to an upstream server.

### HTTP dynamic forward proxy

- Envoy can perform the role of an HTTP proxy without prior knowledge of all configured DNS addresses, while still retaining the vast majority of Envoy's benefits including async DNS resolution
- Works as follows:
	- The dynamic forward proxy HTTP filter is used to pause requests if the target DNS host is not already in cache
	- Envoy will begin async resolving the DNS address, unblocking any requests waiting on the response when the resolution completes
	- Any future requests will not be blocked as the DNS address is already cached
	- All known hosts are stored in the dynamic forward proxy cluster such that they can be displayed in admin output
	- A special load balancer will select the right host to use based on the HTTP host/authority header during forwarding
	- Hosts that have not been used for a period of time are subject to a TTL that will purge them
- Memory usage:
	- Each resolved host/pair uses a fixed amount of memory global to the server and shared amongst all workers
	- Address changes are performed inline using read/write locks and require no host reallocations
	- Hosts removed via a TTL are purged once all active connections stop referring to them and all used memory is re-gained
	- max_hosts can be used to limit the number of hosts that the DNS cache will store at any given time
	- The cluster's max_pending_request circuit breaker can be used to limit the number of requests that are pending waiting for the DNS cache to load a host