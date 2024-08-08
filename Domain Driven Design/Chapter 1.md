
#### Knowledge Rich Design
- Devs refactor the implementation to express the model and give the application use of that knowledge
- It's not good to hide business rules in a hidden guard clause in an application since:
	- It's unlikely and business expert could read this code to verify the rule
	- It would be difficult for a technical, non-business person to connect the requirement text with the code
- When this is the case, we can make use of the strategy pattern 
- Commit a team to using a language relentlessly in all communication within the team and in the code

- Behavioral responsibilities of an object can be hinted at through operation names, and implicitly demonstrated with object interaction (or sequence) diagrams, but they cannot be _stated_. This falls to supplemental text or conversation. In other words, UML diagrams cannot convey two of the most important aspects of a model: the meaning of concepts it represents and what they are meant to do - pg 32

#### Written Design Docs
- Extreme Programming advocates using no extra design docs at all, let the code speak for itself - pg 32
- Documents should give insight to large-scale structures and to focus attention on core elements. It can clarify design intent in a case where the programming language does not support a straight forward implementation of a concept. Written documents should compliment code and the verbal communication

pg 34