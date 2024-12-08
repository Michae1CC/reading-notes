
### Docker Build Overview

https://docs.docker.com/build/concepts/overview/

- Docker Build implements a client-server architecture, where:
	- Client: Buildx is the client and the user interface for running and managing builds
	- Server: Buildkit is the server, or builder, that handles the build execution
- When you invoke a build, the Buildx client sends a build request to the BuildKit backend. Buildkit resolves the build instructions and executes the build steps
- Buildx is a CLI tool that you use to run builds. The `docker build` command is a wrapper around Buildx.
- BuildKit is the daemon process that executes the build workloads.
- A build execution starts with the invocation of a `docker build` command. Buildx interprets your build command and send a build request to the BuildKit backend.
- The request to the BuildKit includes:
	- The Dockerfile
	- Build arguments
	- Export options Caching options


### Docker file
https://docs.docker.com/build/concepts/dockerfile/

- A Dockerfile is a text file containing instructions for building your source code.
- Instructions:
	- FROM - Defines a base for your image
	- RUN \<command\> - Executes any commands in a new layer on top of the current image and commits the result.
	- COPY \<src\> \<dest\> - Copies new files or directories from src and adds them to the filesystem of the container at the path dest.
	- Default name is `Dockerfile` with an extension, `<something>.Dockerfile` is also a common convention

#### Syntax

- The first line is a syntax parser directive which instructs Docker builder what syntax to use when parsing the Dockerfile


### Build context

https://docs.docker.com/build/concepts/context/

- The build context is the set of files that your build can access. The positional argument that you pass to the build command specifies the context that you want to use for the build
- When your build context is a local directory, a remote Git repository, or tar file, then that becomes the set of files that the builder can access during the build. Build instructions such as `COPY` and `ADD` can refer to any of the files and directories in the context
- Use the following syntax to build an image using files on your local file system, while using the Dockerfile from stdin `docker build -f- PATH`.

```bash
# create a directory to work in
mkdir example
cd example

# create an example file
touch somefile.txt

# build an image using the current directory as context
# and a Dockerfile passed through stdin
docker build -t myimage:latest -f- . <<EOF
FROM busybox
COPY somefile.txt ./
RUN cat /somefile.txt
EOF
```

- You can create a tarball of the directory and pipe it to the build for use as a context

```
$ tar czf foo.tar.gz *
$ docker build --file test.Dockerfile - < foo.tar.gz
```

https://docs.docker.com/build/concepts/context/#remote-context

- When you pass a URL pointing to the location of a Git repository as an argument to `docker build`, the builder uses the repository as the build context.
- Only performs a shallow clone
- Recursively clones the repo and any submodules it contains
- `docker build https://github.com/user/myrepo.git`