
https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/observability/observability

- Observability is a measure of how well internal states of a system can be inferred from knowledge of external outputs
- For TCP Proxy, it is possible to enable a one-time log entry right after a successful upstream connection by enabling flush access log on connected
- It is possible to enable a one-time access log entry each time a new HTTP request arrives, and before the filter chain is processed by enabling flush access log on new request
- It is possible to enable one-time upstream access log entry each time a new upstream is associated with a downstream stream, after the connection with the upstream is established