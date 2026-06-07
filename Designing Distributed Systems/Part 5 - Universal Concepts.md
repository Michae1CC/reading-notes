
pg 159

- In any system, there are four key concepts which make up our solutions:
	- Logging
	- Metrics
	- Alerting
	- Tracing
- A metric is generally related to aggregate data across multiple requests
- To differentiate between logs and understand how they relate to a specific request, context like thread identifier, user ID, or request ID help differentiate between otherwise identical logs on the same server and filter to the context of a specific request
- When monitoring your application there are three basic types of data that are interesting to record:
	- Histograms
	- Counts
	- Values