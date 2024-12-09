
## Continuous Integration

- IMPORTANT: When a number of people are working in the same Bounded Context there is a strong tendency for the model to fragment. The bigger the team, the bigger the problem, but as few as three or four people can encounter serious problems. Yet breaking down the system into ever-smaller contexts eventually loses a valuable level of integration and coherency.
- Continuous Integration means that all work within the context is being merged and made consistent frequently enough that when splinters happen they are caught and corrected quickly.
- Most effective processes for CI:
	- step-by-step merge/build technique
	- automated test suites
	- rules that set some reasonable small upper limit on the lifetime of unintegrated changes


### Context Map

pg 244

- A Context Map is in the overlap between the project management and software design
- Within a bounded context you will have a coherent dialect of the Ubiquitous Language. The names of the Bounded Context will themselves enter the Language so that you can speak unambiguously about the model of any part of the design by making you Context clear
- IMPORTANT: Identify each model in play on the project and define its Bounded Context. This includes the implicit models of non-OO subsystems. Name each Bounded Context and make the names part of the Ubiquitous Language.


### Shared Kernel

pg 251

- When functional integration is limited, the overhead of Continuous Integration may be deemed too high.
- IMPORTANT: Uncoordinated teams working on closely related applications can go racing forward for a while. but what they produce may not fit together. They can end up spending more on translation layers and retro fitting than they would have on Continuous Integration in the first place, meanwhile duplicating effort and losing the benefits of a common Ubiquitous Language.
- IMPORTANT: Designate some subset of the domain model that the two teams agree to share. Of course this includes, along with this subset of the model, the subset of code or of the database design associated with that part of the model. This explicitly shared stuff has special status, and should not be changed without consultation with the other team. Integrate a functional system frequently, but somewhat less than the pace of Continuous Integration within the teams. At these integrations, run the tests of both teams.


## Customer/Supplier Dev Teams

pg 252

- Upstream and downstream subsystems where the downstream component takes the output from the upstream component and all the dependencies go one way, separate naturally into to Bounded Contexts.
- IMPORTANT: The freewheeling development of the upstream team can be cramped if the downstream has veto power over changes or if procedures for requesting changes are too cumbersome. The upstream team may even be inhibited by worries of breaking the downstream system. Meanwhile, the downstream team can be helpless, at the mercy of upstream priorities.
- IMPORTANT: Establish a clear customer/supplier relationship between two teams. In planning sessions, make the downstream team play the customer role to the upstream team. Negotiate and budget tasks for the downstream requirements so that everyone understands the commitment and schedule.
- Jointly develop automated acceptance tests that will validate the interface they expect.