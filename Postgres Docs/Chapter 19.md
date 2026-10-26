
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