
pg 253

- If there's so much data or such high write throughput that a single node cannot handle it, it splits the data into smaller _shards_ or _partitions_, and stores different shards on different nodes
- Used to achieve horizontal scaling

pg 254

- Sometimes sharding is used to implement multitenant systems. Either each tenant is given a separate thread, or multiple small tenants may be grouped together into a larger shard. These shards might be physically separate databases or separately manageable portions of a larger logical database.
- Advantages of sharding for multitenancy
	- Resource Isolation - In one tenant performs a computationally expensive operation, it is less likely that other tenant's performance will be affected if they are running on different shards
	- Permission Isolation - If there is a bug in access control logic, it's less likely that you will accidentally give one tenant access to another tenant's data if those tenants database's are physically separate
	- Cell-based Architecture - You can apply sharding not only at the data storage level, but also for the services running application code. In a _cell-based_ architecture, the services and storage for a particular set of tenants are grouped into a self-contained _cell_, and different cells are set up such that they can run largely independently from each other
	- Per-tenant backup and restore - Makes it possible to restore a tenant's state from a backup without affecting other tenants
	- Regulatory compliance - If each persons' data is stored in a separate shard, this translates into simple data export and deletion operations on their shard
	- Data residence - If a particular tenant's data needs to be stored in a particular jurisdiction to comply with data residency laws, a region-aware database can allow you to assign that tenant's shard to a particular region
	- Gradual schema rollout - Schema migrations can be rolled out gradually, one tenant at a time. This reduces risk, as you can detect problems before they affect all tenants, but difficult to do transactionally
- Difficulties of sharding for multitenancy
	- Assumes that each individual tenant is small enough to fit on a single node
	- You may have many small tenants, creating a separate shard for each one may incur too much overhead
	- If you ever need to support features that connect data across multiple tenants these become harder to implement if you need to join across multiple tenants

pg 261

- _Hash-range sharding_ - If the required number of shards can't be predicted in advanced, it's better to use a scheme in which the number of shards can adapt easily to the workload. One solution is to combine key-range sharding with a hash function so that each shard contains a range of hash values rather than a range of keys

pg 264

- A system that defines shards based on ranges of keys makes it possible to put an individual hot key in a shard by itself
- It's possible to compensate for skew at the application level. For example, if one key is known to be very hot, a simple technique is to add a random number to the beginning or end of the key. The volume of reads to each shard of the hot key is not reduced; only the write load is split. This also requires additional bookkeeping: it makes sense to append the random number for only a small number of hot keys; for the vast majority also need some way of keeping track of which keys with low write throughput, this would be unnecessary overhead.

pg 268

- The partition key is the first part of the primary key, so we can use the partition key to determine the shard and thus route reads and writes to the node that is responsible for that key
- A secondary index is usually doesn't identify a record uniquely but rather is a way of searching for occurrences of a particular value
- _Local Secondary Index_ or _Document-Partitioned Index_ - Each shard independently maintains its own secondary indexes, covering only the records in that shard. It doesn't care what data is stored in other shards. Whenever you write to the database you only need to deal with only the shard containing the record that you are writing.
- When reading from a local secondary index, if you already know the partition key of the record you're looking for, you can just perform the search on the appropriate shard. Moreover, if you want all the results and don't need all of them, you can send a request to any shard. However, if you want all the results and don't know their partition key in advanced, you will need to send the query to all shards and combine their results you get back, because the matching records might be scattered across all the shards.
- _Global Secondary Index_ or _Term-Partitioned_ - Rather than having each shard having its own local secondary index, we can construct an index that covers all data in the shards. A global index must also be sharded, but it can be sharded differently from the primary key.
- Global Indexes have the advantage that a query with a single condition needs to read from only a single shard.