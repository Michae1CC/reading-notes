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

#### Json File Driver

https://docs.docker.com/engine/logging/drivers/json-file/

- Be default, Docker captures the standard output (and standard error) of all your containers and writes them in files using the JSON format
- Example:

```
{
  "log": "Log line is here\n",
  "stream": "stdout",
  "time": "2019-01-01T11:11:11.111111111Z"
}
```

#### Awslogs Driver

https://docs.docker.com/engine/logging/drivers/awslogs/

- Send container logs to Amazon Cloudwatch Logs. Log entries can be retrieved through the AWS management Console.
- You can set the logging driver for a specific container by using the `--log-driver` option to `docker run`: `docker run --log-driver=awslogs ...`
- Options:
	- `awslogs-endpoint`: Uses either the `awslogs-region` log option or the detected region to construct the remote CloudWatch Logs API endpoint
	- `awslogs-group`: You must specify a log-group for the awslogs driver.
	- `awslogs-create-group`: Log driver returns an error by default if the log group does not exist. You set this option to `true` to automatically create the log group as needed.

https://docs.docker.com/engine/logging/plugins/
https://docs.docker.com/engine/logging/drivers/fluentd/
