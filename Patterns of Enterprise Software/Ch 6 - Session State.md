
pg 81

- The difference between business and system transactions underlie much of the debate over stateless versus stateful sessions
- The whole point of objects is that they combine state (data) with behavior
- A true stateless object is one with no fields (?) - this isn't what most people mean when they talk about statelessness is a distributed enterprise application
- When people refer to stateless server they mean an object that doesn't retain state between requests. Such an object may well have fields, but when you invoke a method on a stateless server the values of the fields are undefined

### State Session

pg 83

- The details of a shopping cart are session state, meaning that the data in the cart is relevant only to that particular session
- Session state is distinct from "record data", which is long-term persistent data held in the database and visible to all sessions
- While a customer is editing the session state, the current state may not be legal. The customer uses a request to send this to the system indicating invalid values.
- The biggest issue with session state is dealing with isolation. The most obvious is two people editing the state at the same time.

pg 84

- Storing the session state
	- Client session state: Store the data on the client. Encode data in a URL for Web presentation, using cookies, serializing data into some hidden field on a web form
	- Server Session state: May be as simple as holding data in memory. Usually there's a mechanism for storing the session state somewhere more durable as a serialized object
	- Database session: Also server-side storage involves breaking up data into tables and fields and storing it in the database much as you would store more lasting data.
- Session migration allows a session to move from server to server as one server handles one request and other servers take on the others. Its opposite to server affinity, which forces one server to handle requests for a particular session. Server migrations leads to better balancing of your servers, particularly if your sessions are long.

- Ways to store the session state
	- Client Session State - Store the data on the client. There are several ways to do this: encoding the data in URL for Web presentation, using cookies, serializing data into some hidden field on a Web form
	- Server Session State - May be as simple as holding the data in mem between requests. The object can be stored on the app server's local file system, or it can be placed in a shared data source.
	- Database Session State - Is also server-side storage, but it involves breaking up the data into tables and fields and storing it in the database.

