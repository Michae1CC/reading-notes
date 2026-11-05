
https://www.postgresql.org/docs/current/config-setting.html

- All Parameter names are case insensitive and can be one of 5 value types:
	- Boolean: `on`, `off`, `true`, `false`, `1`, `0`
	- String
	- Numeric
	- Numeric w/ Unit
	- Enumerated
- `ALTER DATABASE` - allows global settings to be overridden on a per database basis
- `ALTER ROLE` - allows both global and per-database settings to be overridden with user-specific values
- These parameters are set in the `postgresql.conf` file
- The configuration file is re-read whenever the main server process receives a SIGHUP signal - this is most easily sent by running `pg_ctl reload`

https://www.postgresql.org/docs/19/runtime-config-file-locations.html

- `data_directory` - Specifies the directory to use for data storage. This parameter can only be set at server start
- `config_file` - Specifies the main server configuration file. This parameter can only be set on the postgres command line
- `hba_file` - Specifies the configuration file for host-based authentication. Can only be set at server start
- `ident_file` - Specifies the config file for user name mapping. Can only be set at server start
- `hosts_file` - Specifies the configuration file for host-based SSL configuration
- `external_pid_file` - Specifies the name of an additional process-ID file that the server should create for use by server admin programs

#### Connection Settings

https://www.postgresql.org/docs/19/runtime-config-connection.html

- `listen_addresses` - Specifies the TCP/IP address(es) on which the server is to listen for connections from client applications. Comma sep list
- `port` - The TCP port the server listens on; 5432 by default
- `max_connections` - Determines the number of concurrent connections to the database server
- `tcp_keepalives_idle` - Specifies the amount of time with no network activity after which the operating system should send TCP keepalive message to the client
- `tcp_keepalives_count` - Specifies the number of TCP keepalive messages that can be lost before the server's connection to the client is considered dead
- `tcp_keepalives_interval` - Specifies the amount of time after which a TCP keepalive message that has not been acknowledged by the client should be retransmitted
- `tcp_user_timeout` - Specifies the amount of time that transmitted data may remain unacknowledged before the TCP connection is forcibly closed
- `client_connection_check_interval` - Sets the time interval between optional checks that the client is still connected, while running queries. The check is performed by polling the socket and allows long running queries to be aborted sooner if the kernel reports that the connection is closed

https://www.postgresql.org/docs/19/runtime-config-resource.html

- `shared_buffers` - Sets the amount of memory the database server uses for shared memory buffers. The default is typically 128 MB
- There is a separate server process called the _background writer_, whose function is to issue writes of "dirty" shared buffers. When the number of clean shared buffers appears to be insufficient, the background writer writes some dirty buffers to the file system and marks them as clean. This reduces the likelihood that server processes handling user queries will be unable to find clean buffers and have to write to dirty buffers themselves