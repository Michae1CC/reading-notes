
https://www.postgresql.org/docs/current/tutorial.html

## Getting started

- *Server Process* - Manages the database files, accepts connections to the database from the client applications, and performs database actions on behalf of the clients.
- The *user's client application* that wants to perform database operations. Client applications can be very diverse.
- The PostgreSQL server can handle multiple concurrent connections from clients. To achieve this it starts (forks) a new process for each connection. From that point on, the client and the new server process communicate without intervention by the original postgres process.

## Creating a Database

https://www.postgresql.org/docs/current/tutorial-createdb.html