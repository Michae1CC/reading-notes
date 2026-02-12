
### Client Session State

pg 456

- Stores session state on the client
- Even the most server-oriented designs need at least a little Client Session State, if only to hold a session identifier
- There a three commons ways to do client session state
	- URL parameters - Easiest for a small  amount of work. All URLs on any response page take the session state as a parameter. Essentially all URLs on any response page take the session state as a parameter.
	- Hidden field - A field sent to the browser that isn't displayed on the Web page.
	- Cookies - Sent back and forth automatically. Won't work between domains.
- Reacts well in supporting server objects with maximal clustering and failover resiliency
- Always use Client Session State for session identification

### Server Session State

pg 458

- Keeps the session state on a server system in a serialized form.
- Appeal of Server state session is simplicity

### Database Session State

pg 462

- Stores session data as committed data in the database
- When a call goes out from the client to the server, the server object first pulls the data required for the request from the database. Then it does the work it needs to do and saves back to the database all the data required.
- Data involved is typically a mix of session data that's only local to the current interaction and committed data that's relevant to all interactions
- You'll need a mechanism to clean out the session data if a session is canceled or abandoned
- You'll gain by using stateless objects on the server, thus enabling pooling and easy clustering - however, you'll pay with the time needed to pull the data in and out of the server object so you won't have to read the data out of the database whenever the cache is hit, but you'll still pay the write costs
- In a choice between db session state, server session state the biggest issue may be how easu it is to support clustering and failover with server session state in your particular app