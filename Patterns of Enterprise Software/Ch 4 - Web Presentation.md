
pg 55

- The web server's job is to interpret the URL of request the request and hand over control to a Web server program
- Two mains forms of structuring a program in a Web server: script or server page
- Script form is a program, usually with functions or methods to handle the HTTP call. The program text can do pretty much anything a program can do, and the script can be broken down into subroutines and can create other services. It gets data from the Web page by examining the HTTP request object which is a string (may be parsed by the framework)
pg 56
- In the MVC model, a request come in to an input controller, which pulls information off the request. It then forwards the business logic to an appropriate model object. The model object talks to the data source and does everything indicated by the request.
- The most important reason for applying MVC is to ensure the models are completely separated from the Web presentation. Putting the processing into separate Transaction Scripts or Domain Model objects will make it easier to test your view.
- The purpose of an application controller is to handle to flow of an application

pg 57 - MVC flow diagram