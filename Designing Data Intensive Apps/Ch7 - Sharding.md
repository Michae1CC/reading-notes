
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