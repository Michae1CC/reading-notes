
pg 69

- with sharded services, each replica, or _shard_, is only capable of serving a subset of all requests. A load balancing node, or _root_, is responsible for examining each request and distributing each request to the appropriate shard for processing
- The primary reason for sharding a service is because the size of the state is too large to be served by a single machine
- Sometimes sharding can also be useful for stateless services for the purpose of isolation

pg 77

- Given both the request and _Request_ and _Shard_, then the role of the sharding function is to relate them together, specifically:
	- _Shard = ShardingFunction(Request)_
- Commonly the sharding function is defined using a hashing function and the modulo operator
- The hash function has two important characteristics for our sharding:
	- _Determinimism_ - The output should always be the same for a unique input
	- _Uniformity_ - The distribution of outputs across the output space should be equal
- Many sharding functions use _consistent hashing functions_. Consistent hashing functions are special hash functions that are guaranteed to only remap `#keys / #shards`, when resized to `#shards`. This is dramatically better than losing the entire sharded service.

Example proxy pass server:

```nginx
events {
	worker_connections 1024;
}

http {
	# define a named 'backend' that we can use in the proxy directive
	# below.
	upstream backend {
		# Has the full URI of the request and use a consistent hash
		hash $request_uri consistent
		server web-shard-1.web;
		server web-shard-2.web;
		server web-shard-3.web;
	}
	
	server {
			listen localhost:80;
			location / {
			proxy_pass http://backend;
		}
	}
}
```


### Hot Shards

pg 81

- Ideally the load on a shard cache will be perfectly even, but in many cases this isn't true, and _hot shards_ appear because organic load patterns drive more traffic to one particular shard (for example a photo or video going viral)
- If you set up auto scaling for each shard, you can dynamically grow and shrink each replicated shard as the organic traffic to your service shifts around
- _scatter/gather_ pattern is a tree pattern with a root that distributes requests and leaves that process those requests. Requests  are simultaneously farmed out to all of the replicas in the system. Each replica does a small amount of processing and then returns a fraction of the result to root. The root server then combines the various partial results together to form a single complete response to the request and then sends this request back to the client
- _scatter/gather_ is quite useful when you have a large amount of mostly independent processing that is needed to handle a particular request.

### Scatter/Gather with Root Distribution

pg 84

- Each leaf is entirely homogenous but the work is distributed to a number of different leaves in order to improve performance of a request - solves the "embarrassingly parallel" problem
- Truly achieving a completely parallel speed-up on a single process is going to be tricky, as things like memory, network etc. become a bottle neck. Instead of parallelizing an application across cores on a single machine, we can use the scatter/gather pattern to parallize requests on a single machine.

### Faas

pg 92

- It dramatically simplifies the distance from code to running service. Because there is no artifact to create or push beyond the source code itself, Faas makes it simple to go from code on a laptop of web browser to running code in the cloud.
- Functions are stateless, and thus any system you build on top of functions is inherently more modular and decoupled than a similar system built into a single binary.

#### Decorator pattern

pg 95

- Faas is ideal for deploying simple functions that can take an input, transform it into an output, and then pass it on to a different service
- A good example to demonstrate the value of the decorator pattern is adding defaults to the input to an HTTP RESTful API
- Consider the task of adding default values to a RESTful function call if the values are missing. We'll write the default function using the python programming language

```python
def handler(context):
	# Get the input value
	obj = context.json
	# If the 'name' field is not present, set it randomly
	if obj.get("name", None) is None:
		obj["name"] = random_name()
	# If the 'color' field is not present, set it to 'blue'
	if obj.get("color", None) is None:
		obj["color"] = "blue"
	# Call the actual API, potentially with the new default
	# values, and return the result
	return call_my_api(obj)
```