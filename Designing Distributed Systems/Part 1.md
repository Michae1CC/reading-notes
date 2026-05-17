
pg 13

- _Referential integrity_ - Refers to the integrity or accuracy of data stored in multiple different rows within a system. If the code in your application relies on there being consistency between various data stores, then your code can handle cases in which data in multiple stores may not fully be in sync. If instead your code does not rely on data being in sync, your code does not rely on relational integrity.
- _Consistency_ - The notion that the data written is the same in all locations
- In a _strongly consistent_ system, the data is guaranteed to be present everywhere before the writing of the data is considered to be complete
- In an _eventually consistent_ system, the data is guaranteed to be the same everywhere "eventually", where the eventually can actually mean a long-ish time
- _Health_ is used to understand when an application is ready to be added to a load balancer, when an application is unhealthly and needs to be restarted