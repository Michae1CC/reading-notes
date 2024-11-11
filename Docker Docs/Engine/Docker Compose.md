
https://docs.docker.com/compose/

- A tool for defining and running multi-container applications.

https://docs.docker.com/compose/how-tos/project-name/

- The default project name is derived from the base name of the project directory. However you have the flexibility to set custom project names.
- Precedence for project name
	- The `-p` command line flag
	- The `COMPOSE_PROJECT_NAME` envar
	- The top level `:name` attribute in the Compose file
	- The base name of the project directory containing you Compose file.
	- The base name of the current directory if no Compose file is specified

https://docs.docker.com/compose/how-tos/lifecycle/
