
### The Three Principal Layers

pg 20

- **Presentation** - Provision of service, display of information (e.g. Windows or HTML) - Handling of user requests (mouse clicks, keyboard hits), HTTP requests, command-line invocations, batch API. It's primary responsibility of the presentation layer are to display information to the user and to interpret commands from the user into actions upon the domain and data source.
- **Domain/Business Logic** - Logic that is the real point of the system - The work that this application. It involves calculations based on inputs and stored data, validation of any data that comes in from the presentation, and figuring out exactly what data source logic to dispatch depending on commands received from the presentation.
- **Data Source** - Communication with databases, messages systems, transaction managers, other packages - Communicating with other systems that carry out tasks on behalf of the application. These can be transactions monitors, other applications, messaging systems and so forth.

---

(the different layers from DDD pg 53 for comparison)

-  User Interface / Presentation Layer - Used for showing and interpreting user's commands.
- App Layer - Defines the jobs software is suppose to do and directs the expressive domain objects to work out problems. The tasks this layer is responsible for are meaningful to the business or necessary for interaction with the app layers of other systems. It does not contain business rules or knowledge but only coordinates tasks and delegates work to collaborations of domain objects in the next layer down. It does not have state reflecting the business situation, but it can have state that reflects the progress of a task for the user or the program.
- Domain Layer - Responsible for representing concepts of the business, information about the business situation and business rules. State that reflects the business situation is controlled and used here, even though the technical details of storing it are delegated to the infra
- Infra Layer - Provide generic technical capabilities that support the higher layers: message sending for the application, persistence of the domain, drawing widgets for the UI, etc. The infra layer may also support the pattern of interactions between the four layers through an architectural framework.

---
pg 20

- Sometimes layers are arranged so that the domain layer is completely hides the data source from the presentation. More often, the presentation layer accesses the data store directly which tends to work better in practice. The presentation layer may interpret a command, query data then hand it to the domain layer to manipulate that data before presenting

---
pg 55 of DDD for comparison

- LAYERS are meant to be loosely coupled, with design dependencies in only one direction. Upper LAYERS can use or manipulate elements of lower ones straight-forwardly by calling their public interfaces, holding references to them (at least temporarily) and generally using conventional means of interactions. But when an object of a lower level needs to communicate upward (beyond answering a direct query) we need another mechanism, drawing on architectural patterns for relating layers such as callbacks or OBSERVERS

---

- Splitting domain logic across both the desktop and server - isolate the domain logic in the client in a self contained module that isn't dependent on any other part the system.
pg 21
- How you separate (the layers) depends on how complex the application is. A simple script to pull data from the database and display it in a Web page may all be one procedure. I would still endeavor to separate the three layers in separate sub routines. As the system gets more complex, I would break the three layers into separate classes. As complexity increased I would divide the classes into separate packages
- The domain and data source should never be dependent on presentation, i.e there should be no subroutine call from the domain or data source code into the presentation code


