
https://www.postgresql.org/docs/current/config-setting.html

- All Parameter names are case insensitive and can be one of 5 value types:
	- Boolean: `on`, `off`, `true`, `false`, `1`, `0`
	- String
	- Numeric
	- Numeric w/ Unit
	- Enumerated
- `ALTER DATABASE` - allows global settings to be overridden on a per database basis
- `ALTER ROLE` - allows both global and per-database settings to be overridden with user-specific values