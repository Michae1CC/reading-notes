
pg 162
- Old and new versions of the code, and old and new data formats, may potentially exist in the system at the same time. For the system to continue running smoothly, you need to maintain compatibility in both directions
	- Backward Compatibility - Ensure that newer code can read data written by older code
	- Forward Compatibility - Ensures that older code can read data written by newer code

pg 187
- Workflows are executed by a _workflow engine_
- Workflow engine are typically composed of an orchestrator and executor
	- The _orchestrator_ is responsible for scheduling tasks to be executed
	- The _executor_ is responsible for executing tasks