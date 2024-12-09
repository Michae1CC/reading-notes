
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