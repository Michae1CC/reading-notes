https://docs.docker.com/engine/network/

- Containers have networking enabled by default and they can make outgoing connections.
- A docker hostname is a label assigned to a container to uniquely identify it within a network to facilitate the communication between containers. Defaults to the container's ID if not specified.
- A container has no information about what kind of network it's attached to, or whether their peers are also Docker workloads or not.
- A container on sees a network interface with an IP address, a gateway, a routing table, DNS services, and other network details
- By default, when you create or run a container user `docker create` or `docker run`, containers on bridge networks don't expose any ports to the outside world. Use the `--publish` or `-p` flag to make a port available to services outside the bridge network.

### IP address and hostname
- By default, the container gets an IP address for every Docker network it attaches to
- A container receives an IP address out the IP subnet of the network
- The Docker daemon performs dynamic subnetting and IP address allocation for containers
- Each network also has a default subnet mask and gateway

#### DNS Services
- Containers use the same DNS servers as the host by default, but you can override with `--dns`
- By default, containers inherit the DNS settings as defined in the `/etc/resolv.conf` config file
- Containers that attach to a custom network use Docker's embedded DNS server. The embedded DNS server forwards external DNS lookups to the DNS servers configured on the host.

### Network drivers

https://docs.docker.com/engine/network/drivers/bridge/
#### Bridge
- Uses a software bridge which lets containers connected to the same network communicate, while providing isolation from containers that aren't connected to the bridge network.
- The docker bridge driver automatically installs rules in the host machine so that containers on different bridge networks can't communicate directly with each other.
- When no host address is given in port publishing options like `-p 80`, the default is to make the containers port 80 available on all host addresses, IPv4 and IPv6.
- If you do not specify a network using the `--network` flag, and you do specify a network drive, your container is connected to the default `bridge` network by default. Containers connected to the default `bridge` network can communicate, but only by IP address unless the `--link` flag is used.

#### Overlay
https://docs.docker.com/engine/network/drivers/overlay/
- The overlay network driver creates a distributed network among multiple Docker daemon hosts
- You can create `overlay` networks using `docker network create`, in the same way you would create a user-defined bridged network.
- To join an overlay network run `docker run --network multi-host-network busybox sh`
- Publishing ports of a container on an overlay network opens the ports to other containers on the same network. Containers are discoverable by doing a DNS lookup using the container name.

#### Host
https://docs.docker.com/engine/network/drivers/host/
- The container's network stack isn't isolated from the Docker host and the container doesn't get its own IP-address allocated
- `-p` and `--publish` options are ignored
- Useful for:
	- optimising performance
	- container needs to handle a large range of ports