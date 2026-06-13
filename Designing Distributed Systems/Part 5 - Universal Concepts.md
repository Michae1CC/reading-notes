
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
- Though total requests count, the rate of requests, and latency are the most common and most generally useful metrics for monitoring requests to your server or service, there are more specialised metrics
- The most basic form of alerting is based on metric queries and static thresholds

pg 180

- The absence of ay errors is more likely to indicate a serious problem rather than that everything is awesome
- If you treat all unauthorised as "user errors" and you don't look for anomalies liek all user's failing authorization, you probably won't alert when you break your authorization code