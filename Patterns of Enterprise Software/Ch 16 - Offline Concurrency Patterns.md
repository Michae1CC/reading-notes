
pg 415

- Optimistic Offline Lock solves this problem by validating that the changes about to be committed by one session don't conflict with the changes of another session
- A successful pre-commit validation is, in sense, obtaining a lock indicating it's okay to go ahead with the changes to the record data
- Optimistic Offline Lock assumes that the chance of conflict is low
- An Optimistic Offline Lock is obtained by validating that, in the time since a session loaded a record, another session hasn't altered it. It can be acquired at any time, but is valid only during the system transaction in which it is obtained
- The most common implementation is to associate a version number with each record in your system with each record in your system
- Once verification succeeds, all changes, including an increment of the version, can be committed
- The version increment is what prevents inconsistent record data
- It's a bad idea to use the modification timestamp rather than a version count for your optimistic check because system clocks are simply too unreliable, especially if your optimistic checks because system clocks are simply too unreliable, especially if you're coordinating across multiple servers
- Often implementing Optimistic Offline Lock is left at including the version in UPDATE and DELETE statements, but this fails to address the problem of an inconsistent read
- Concurrency management is as much a domain issue as it is a technical one
- Appropriate when the chance of conflict between any two business transactions is low