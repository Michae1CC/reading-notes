
### Gateway

pg 466

- An object that encapsulates access to an external system or resource
- When accessing external resources, you'll usually get APIs for them. However, these APIs are naturally going to be complicated because they take the nature of the resource into account
- Create a simple API for your usage and use the Gateway to translate to the external source
- Keep a gateway as simple as you can. Focus on the essential roles to adapting the external service and providing a good point for stubbing
- Sometimes a good strategy is to build the Gateway in terms of more than one object, obvious form is to use two objects:
	- back end - acts as a minimal overlay to the external resource and doesn't simplify the resources API at all
	- front end - transforms the awkward API into a more convenient one for your application to use
- you should consider Gateway whenever you have an awkward interface to something that feels external
- Gateway usually makes a system easier to test by giving you a clear point at which to deploy Service Stubs
- Any change to the resource means you only have to alter the Gateway class - the change doesn't have to ripple through the rest of the system
- Facade differences - while a Facade simplifies a more complex API, it's usually done by the writer of the service for general use. A Gateway is written by the client for its particular use.
- Adapter alters an implementation's interface to match another interface you need to work with
- Mediator usually separates multiple objects so that they don't know about each other but do know about the mediator