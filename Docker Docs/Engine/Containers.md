
https://docs.docker.com/engine/containers/run/

Docker runs processes isolated containers. A container is a process which runs on a host. The host may be local or remote. When you execute `docker run`, the container process that runs is isolated in that it has its own file system, its own networking, and its own isolated process tree separate from the host.

- When you start a container, the container runs in the foreground by default. If you want to run the container in the background instead, you can use the `--detach` (or `-d`) flag. This starts the container without occupying you terminal window.

If you want to run the container in the background instead, you can use the `--detach` (or `-d`) flag. This starts the container without occupying you terminal window.

https://docs.docker.com/engine/containers/run/#container-networking