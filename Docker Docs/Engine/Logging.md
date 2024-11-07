https://docs.docker.com/engine/logging/

- By default `docker logs` or `docker service logs` shows the command's output just as it would appear if you ran the command interactively in a terminal.
- By default `docker logs shows the commands` `STDOUT` and `STDERR`.


https://docs.docker.com/engine/logging/configure/

- Logging drivers are mechanisms to help you get information running from containers and services.
- Each Docker daemon has a default logging driver, which each container uses unless you configure it to use a different logging driver, or log driver for short
- As a default, Docker uses the json-file logging driver which caches logs as JSON internally
- Docker provides two modes for delivering messages from the container to the log driver:
	- (default) direct, blocking delivery from the container to driver
	- non-blocking, delivery that stores log messages in an intermediate per-container buffer for consumption by driver.
- The non-blocking message delivery mode presents applications from blocking due to logging back pressure. Applications are likely to fail in unexpected ways when STDERR or STDOUT streams block.

https://docs.docker.com/engine/logging/drivers/json-file/
https://docs.docker.com/engine/logging/plugins/
https://docs.docker.com/engine/logging/drivers/fluentd/
https://docs.docker.com/engine/logging/drivers/awslogs/