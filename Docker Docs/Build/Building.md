
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