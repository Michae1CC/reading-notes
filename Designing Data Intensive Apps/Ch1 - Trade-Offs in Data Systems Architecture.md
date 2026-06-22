
pg 1

- Many applications need to do the following:
	- Store data so that they, or another application, can find it later (databases)
	- Remember the results of an expensive operation, to speed up reads (caches)
	- Allows users to search data by keyword or filter it in various ways (search indexes)
	- Handles events and data changes as soon as they occur (stream processing)
	- Periodically crunch a large amount of accumulated data (batch processing)
- _backend engineers_ - build services that handle requests for reading and updating data
- _Operational Systems_ - backend services and data infra where data is created - for example, by serving external users. Here, the application code both reads and modifies the data in the databases, based on the actions performed by the users
- _Analytical Systems_ - server the needs of the business analysts and data scientist. They contain read-only copy of the data from the operational system and they are optimized for they types of data processing that are needed for the analytics