
https://docs.docker.com/build/building/multi-stage/

## Multi-stage builds

- You use mutiple FROM statements in your Dockerfile
- Each `FROM` instruction can use a different base and each of them begins a new stage of the build

```Dockerfile
# syntax=docker/dockerfile:1
FROM golang:1.23
WORKDIR /src
COPY <<EOF ./main.go
package main

import "fmt"

func main() {
  fmt.Println("hello, world")
}
EOF
RUN go build -o /bin/hello ./main.go

FROM scratch
COPY --from=0 /bin/hello /bin/hello
CMD ["/bin/hello"]
```

- The end result is a tiny production image with nothing but the binary inside
- The `COPY --from=0` copies just the built artifact from the previous stage into this new stage
- By default, the stages aren't named, and you refer to them by their integer number, starting with 0 for the first `FROM`
- You can name you stages by adding an `AS <NAME>` to the `FROM` instruction

https://docs.docker.com/build/building/multi-stage/#stop-at-a-specific-build-stage

- When you build your image you don't need to build the entire image for every stage
- You can specify a build target with the `--target` flag
- You can use `COPU --from` instruction to copy from a separate image, eg `COPY --from=nginx:latest ...`

https://docs.docker.com/build/building/multi-stage/#stop-at-a-specific-build-stage

- You can pick up where a previous stage left off by referring to it when using the `FROM` directive

```Dockerfile
# syntax=docker/dockerfile:1

FROM alpine:latest AS builder
RUN apk --no-cache add build-base

FROM builder AS build1
COPY source1.cpp source.cpp
RUN g++ -o /binary source.cpp

FROM builder AS build2
COPY source2.cpp source.cpp
RUN g++ -o /binary source.cpp
```


## Build variables

https://docs.docker.com/build/building/variables/

- Build arguments `ARG` and envars `ENV` both serve as a means to pass information into the build process
- Build arguments have no effect on the build unless it's used in an instruction. They're not accessible or present in containers instantiated from the image unless explicitly passed through from the Dockerfile into the filesystem or configuration
- They may persist in the image metadata as provenance attestations and in the image history, which is why they are not suitable for holding secrets

https://docs.docker.com/build/building/variables/#scoping

- Build arguments declared in the global scope aren't automatically inherited into the build stages. They're only accessible in the global scope.

https://docs.docker.com/build/building/variables/#pre-defined-build-arguments

- Multi-platform build arguments describe the build and target platforms for the build.
- The build platform is the OS, architecture and platform variant of the host system where the builder is running:
	- `BUILDPLATFORM`
	- `BUILDOS`
	- `BUILDARCH`
	- `BUILDVARIANT`
- The target platform arguments hold the same values for the target platforms for the build, specified using the `--platform` flag for the `docker build` command
	- `TARGETPLATFORM`
	- `TARGETOS`
	- `TARGETARCH`
	- `TARGETVARIANT`
- Proxy build arguments let you specify proxies to use for a build:
	- `HTTP_PROXY`
	- `HTTPS_PROXY`
	- `FTP_PROXY`
	- `NO_PROXY`
	- `ALL_PROXY`

https://docs.docker.com/build/building/variables/#pre-defined-build-arguments

https://docs.docker.com/build/building/secrets/

- A build secret is any piece of sensitive information, such as password or API token, consumed as part of your application's build process.
- For secret mounts and SSH mounts, using build secrets is a two-step process. First you need to pass the secret into the `docker build` command, and then you need to consume the secret in your `Dockerfile`.
- `docker build --secret id=aws,src=$HOME/.aws/credentials .`
- To consume a secret in a build use the `--mount=type=secret` flag
- The source of a secret can either be a file or an envar
- When consuming a secret in a Dockerfile, the secret is mounted to a file by default

https://docs.docker.com/build/building/multi-platform/

- A multi-platform build refers to a single build invocation that targets multiple different OS or CPU architecture combinations
- When building images, this let's you create a single image that can run on multiple platforms
- Container's share the host kernel, which means that code that's running inside the container must be compatible with the host's architecture
- Mutil-platform images contain a manifest list, pointing to multiple manifests, each of which points to different configuration and set of layers

https://docs.docker.com/build/building/base-images/

- To ensure that you're getting the latest version of dependencies in your build, you can use `--no-cache` option to avoid cache hits

```
# syntax=docker/dockerfile:1
FROM ubuntu:24.04
RUN apt-get -y update && apt-get install -y --no-install-recommends python3
```

- Sort multi-line arguments alphanumerically to make maintenance easier.

### Best practices

https://docs.docker.com/build/building/best-practices/#leverage-build-cache

- To fully secure you supply chain integrity, you can pin the image version to a specific digest.
```
# syntax=docker/dockerfile:1
FROM alpine:3.19@sha256:13b7e62e8df80264dbb747995705a986aa530415763a6c58f84a3ca8af9a5bcd
```

- When you check in a change to source control or create a pull request, use github actions or another CI/CD pipeline to automatically build and tag a Docker image and test it.
- Split long or complex `RUN` statements on multiple lines separated with backslashes to make Dockerfile more readable, understandable and maintainable

```
RUN apt-get update && apt-get install -y --no-install-recommends \
    package-bar \
    package-baz \
    package-foo
```

- Always combine `RUN apt-get update` with `apt-get install` in the same `RUN` statement to prevent caching problems
- Docker executes commands using the `/bin/sh -c` interpreter
- If you want the command to fail due to an error at ay stage in the pipe, prepend `set -o pipefail &&` to ensure that an unexpected error prevents the build from inadvertently succeeding. For example:

```
RUN set -o pipefail && wget -O - https://some.site | wc -l > /number
```

https://docs.docker.com/build/building/best-practices/#expose

https://docs.docker.com/build/building/best-practices/#expose

- Use the `ENV` to update the `PATH` envar for the software for the software your container installs
- `ENV` is useful for providing the required environment variables specific to services you want to containerize
- `ENV` can be used to set commonly used version numbers to services you want to containerize, for example:

```Dockerfile
ENV PG_MAJOR=9.3
ENV PG_VERSION=9.3.4
RUN curl -SL https://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgres
ENV PATH=/usr/local/postgres-$PG_MAJOR/bin:$PATH
```

https://docs.docker.com/build/building/best-practices/#add-or-copy

- `COPY` supports basic copying of files into the container, from the build context or from a stage in a multi-stage build
- `ADD` supports features for fetching files from remote HTTPS and Git URLs and extracting tar files automatically when adding files from the build context
- bind-mounted files are only added temporarily for a single `RUN` instruction, and don't persist in the final image. If you need to include files from the build context in the final image, use `COPY`

https://docs.docker.com/build/building/best-practices/#entrypoint


- The best use for `ENTRYPOINT` is to set the image's main command, allowing that image to be run as though as it was that command, and then use `CMD` as the default flags.
