
pg 162
- Old and new versions of the code, and old and new data formats, may potentially exist in the system at the same time. For the system to continue running smoothly, you need to maintain compatibility in both directions
	- Backward Compatibility - Ensure that newer code can read data written by older code
	- Forward Compatibility - Ensures that older code can read data written by newer code

pg 187
- Workflows are executed by a _workflow engine_
- Workflow engine are typically composed of an orchestrator and executor
	- The _orchestrator_ is responsible for scheduling tasks to be executed
	- The _executor_ is responsible for executing tasks

pg 209
- An app reading from an _async_ follower may see outdated information if the follower has fallen behind. This leads to apparent inconsistencies in the database; if you run the same query on the leader and a follower at the same time, you may get different results, because not all writes have been reflected in the follower. This inconsistency is a temporary state - if you stop writing to the database and wait a while, the followers will eventually catch up and become consistent with the leader. For that reason it is known as _eventual consistency_.

pg 222
- The biggest problem with multi-leader replication is dealing with concurrent writes on different leaders can lead to conflicts that need to be resolved
- Conflict avoidance - Ensure that a request from a particular user are always routed to the same region and use the leader in that region for reading and writing
- Last write wins - If conflicts can't be avoided, the simplest way of resolving them is to attach a timestamp to each write and to always use the value with the most recent timestamp