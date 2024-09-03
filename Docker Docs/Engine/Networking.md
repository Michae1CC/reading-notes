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