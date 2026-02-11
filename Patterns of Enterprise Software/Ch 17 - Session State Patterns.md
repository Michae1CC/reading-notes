
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