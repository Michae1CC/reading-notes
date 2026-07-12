
pg 197
- _Replication_ means keeping a copy of the same data on multiple machines that are connected via a network
- Reasons to replicate:
	- Keep geographical data close
	- To allow a system to continue working even if some of its parts have failed (increase availability and durability)
	- To scale out the number of machines that can serve read queries
- Each node that stores a copy of the db is called a replica. Every write to the db needs to be processed by every replica; otherwise the replicas would no longer contain the same data
- _leader-based_ replication:
	- One of the replicas is designated as the _leader_/_primary_/_source_.
	- The other replicas are known as _followers_/_read replicas_/_secondaries_. Whenever the leader writes new data to its local storage, it also sends the data change to all its followers as part of a replication log or stream. Each follower takes the log from the leader and updates its local copy of the db accordingly
	- When a client wants to read from the database, it can query either the leader or any of the followers. However, writes are accepted only by the leader