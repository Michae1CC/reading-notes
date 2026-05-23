
pg 69

- with sharded services, each replica, or _shard_, is only capable of serving a subset of all requests. A load balancing node, or _root_, is responsible for examining each request and distributing each request to the appropriate shard for processing
- The primary reason fir sharding a service is because the size of the state is too large to be served by a single machine
- Sometimes sharding can also be useful for stateless services for the purpose of isolation 