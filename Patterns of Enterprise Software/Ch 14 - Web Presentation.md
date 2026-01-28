
### MVC Model

pg 330

- Splits user interface into three distinct roles
- MVC considers three roles:
	- The Model is an object that represents some information about the domain. It's a non-visual object containing all the data and behaviour other than that used for the UI.
	- The View represents the display of the model in the UI.
	- The Controller takes the user input, manipulates the model, and causes the view to update appropriately
  - The UI is often a combination of the view and controller
  - MVC is used to principally separate the presentation from the model and to separate the controller from the view
  - Fundamentally, presentation and model are about different concerns

### Page Controller

pg 333

- An object that handles a request for a specific page or action on a Website
- Key notion is that each page on the Web site is a separate doc on the server
- Page Controller has one input controller for each logical page of the web site. In practice, it doesn't work out to exactly one module per page, since you may hit a link sometimes and get a different page depending on dynamic information
- Basic responsibilities of a Page Controller are:
	- Decode the URL and extract any form data to figure out all the data for the action
	- Create and invoke any model object to process the data. All relevant data from the HTML request should be passed to the model so that the model objects don't need any connection to the HTML request
	- Determine which view should display the result page and forward the model info to it


### Front Controller

pg 344

- A controller that handles all requests for a Web site
- In a complex website, there are many similar things you need to do when handling a request. These include security, internationalization, and providing particular views for certain users. If the input controller behaviour is scattered across multiple objects, much of this behaviour can end up being duplicated
- Front Controller consolidates all request handling by channeling requests through a single handler object
- Usually structured into two parts: a Web handler and a command hierarchy
	- The web handler is the object that actually receives post or get requests from the Web server. It pulls just enough information from the URL and the request to decide what kind of action to initiate and then delegates to a command to carry out the action
	- The Web handler itself is usually a fairly simple program that does nothing other decide which command to run either statically or dynamically
- Only one Front Controller has to be configured into the Web server, which can be easily enhanced with decorators (for auth, encoding, internalization)

### Template View

pg 350

- Renders information into HTML by embedding markers in an HTML page.
- The basic idea of Template View is to embed markers into a static HTML page when it's written. When the page is used to service a request, the markers are replaced by the results of some computation , such as a database query. This way the page can be laid out in the usual manner
- Embedding markers:
	- HTML-like tags
	- WYSIWYG

### Transform View

pg 361

- A view that processes domain data element by element and transforms it into HTML.
- Using Transform View means thinking of this as a transformation where you have the model's data as input and its HTML as output
- The key difference between Transform View and Template View is the way in which the view is organized
- A Template View is organized around the output

### Two Step View

pg 365

- Turns domain data into HTML in two steps: first by forming some kind of logical page, then rendering the logical page into HTML
- You may want to make global changes to the appearance of the sire easily, but common approachers using Template View or Transform View make this difficult because presentation decisions are often difficult across multiple pages or transform modules.
- Two Step View deals with this problem by splitting the transformation into two stages. The first transforms the model data into a logical presentation without any specific formatting; the second converts that logical presentation with the actual formatting needed
- The first stage assembles the information in a logical screen structure that is suggestive of the display elements yet contains no HTML
- The second stage takes the presentation oriented structure and renders it into HTML
- This presentation oriented structure is assembled by specific code written for each screen