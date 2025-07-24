
pg 89
- A procedure call within a process is very is fast, between processes is orders of magnitude slower. Even slower between machines.
- If you base you distribution strategy on classes, you'll end up with a system that does a lot of remote calls and thus needs awkward coarse-grained interfaces. Even with course grained interfaces on every remotable class you'll still end up with too many remote calls and a system that's awkward to modify as a bonus
- Don't distribute your objects!
- To make effective use of multiple clusters - put all the classes into a single process and then run multiple copies of that process on various nodes. That way each process uses local calls to get the job done and thus does things faster
