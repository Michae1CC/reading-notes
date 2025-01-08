
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


## Conformist

pg 255

- IMPORTANT: When two dev teams have an upstream/downstream relationship in which the upstream has no motivation to provide for the downstream team's needs, the downstream team is helpless. Their project will be delayed until they ultimately learn to live with what they are given.
- If the design work is difficult, then the downstream team will still need to develop its own model. They will have to take full responsibility for a translation layer that is likely to be complex.
- IMPORTANT: Eliminate the complexity of translation between Bounded Contexts by slavishly adhering to the model of the upstream team. This cramps the style of the downstream designers. It probably is not going to the ideal model for the application, but it is a huge reduction in complexity. Plus, you will share Ubiquitous Language with you supplier team. Plus, you will make communication easy for them. Altruism may be sufficient to get them to share information with you.


## Separate Ways

pg 261

- Integration is always expensive, and sometime the benefit is small
- Declare a bounded context to have no connection to the others at all, allowing developers to find simple specialized solutions within this small scope


## Open Host Service

pg 263

- When a subsystem has to be integrated with many others, customizing a translator for each can bog down the team. There is no more to maintain, and more and more to worry about when changes are made
- Define a protocol that gives access to your subsystem as a set of services. Open the protocol so that all who need to integrate with you can use it. Enhance and expand the protocol to handle new integration requirements, except when a single team has idiosyncratic needs. Then use a one-off translator to augment the open protocol so that the protocol can stay simple and coherent.


## Published Language

pg 264

- Direct translation to and from the existing domain models may not be a good solution. Those models may be overly complex and poorly factored. They are probably undocumented. If one is used as a data interchange language, it essentially becomes frozen an cannot respond to new development needs.
- Use a well-documented shared language that can express the necessary domain information as a common medium of communication, translating as necessary into and out of that language.


### Highlighted Core

pg 292

- Even though we may know broadly what constitutes an element of the Core Domain, we won't come up with consistent choices from developer to developer or even from one day to the next. As we unravel the model for ourselves, we have no effective way of sharing our discoveries, or even of recording them for ourselves to aide memory. The labor of constantly sifting the model in our heads to identify the key parts is simply too hard. Significant structural changes to the code are the ideal way of identifying the core domain, but not always practical in the short-term. In fact, such major coed changes are difficult to undertake without the very view we are lacking.
- Create a separate document to explain the core domain, it describes the Core Domain and the primary interactions among the Core elements
- Flag the elements of the Core Domain within the primary repository of the model, without particularly trying to elucidate its role. Make it effortless for a developer to know what is in or out of the CORE.
- Use a distillation document as a guide. When programming changes code that does not require any update to the distillation document, they have fully autonomy
- When the  distillation document requires changes to stay in touch, either because they are fundamentally changing the Core Domain elements or relationships or because they are changing the boundaries of the Core, then consultation is called for and dissemination of the new model through a new version of the distillation document.

- When a model or code change affects the Distillation Document, it requires consultation with other team members. When the change is made, it requires immediate notification of all members and the dissemination of the new version of the Distillation Document. Other changes can be integrated without consultation or notification and will be encountered by other members in the course of their work.


### Cohesive Mechanisms

pg 295

- The computations sometimes reach a level of complexity that begins to bloat the design. The conceptual "what" is swamped by the mechanistic "how". A large number of methods that provide algorithms for resolving the problem obscure the methods that express the problem.
- Partition a conceptually cohesive mechanism into a separate lightweight framework. Particularly watch for formalisms or well-documented categories of algorithms. Expose the capabilities of the framework with an Intention-revealing interface. Now the other elements of the domain focused on expressing the problem, delegating the intricacies of the solution to the framework.


### Abstracted Core

pg 305

- When there is a lot of interaction between subdomains in separate packages, either many references will have to be created between packages, which defeats much of the value of the partitioning, or the interaction has to be made more indirect, which makes the model less communicative (harder to understand).
- Identify the most fundamental conceptual elements in the model and factor them into distinct classes, abstract classes, or interfaces. Design this abstract model so that it expresses most of the interaction between significant components. Place this abstract overall model in its own package, while the specalized, detailed implementation classes are placed in their own package defined by subdomain.
- The Abstract core should end up looking like the Distillation Document


### Knowledge Level

pg 326

- A group of objects that describe how another group of objects should behave
- The Knowledge Level addresses requirements for software with configurable behaviour, in which the roles and relationships among Entities must be changed at installation or even runtime
- In an application in which the roles and relationships between Entities varies in different situations, complexity can explode. Neither fully general models nor highly customized ones serves the user's needs. Objects end up with references to other types to cover a variety of cases, or with attributes that are used in different ways in different situations. Or you may end up with many classes that have the same data and behavior, but just have different assembly rules
- A Knowledge Level separates that self-defining aspect of the model and makes its constraints explicit
- Create a distinct set of objects that can be used to describe and constrain the structure and behaviour of the basic model. Keep these concerns distinct as two level, one very concrete, the other reflecting rules and knowledge that a user or super-user is able to customize