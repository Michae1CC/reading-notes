
https://www.postgresql.org/docs/current/tutorial.html

## Getting started

- *Server Process* - Manages the database files, accepts connections to the database from the client applications, and performs database actions on behalf of the clients.
- The *user's client application* that wants to perform database operations. Client applications can be very diverse.
- The PostgreSQL server can handle multiple concurrent connections from clients. To achieve this it starts (forks) a new process for each connection. From that point on, the client and the new server process communicate without intervention by the original postgres process.

## SQL

https://www.postgresql.org/docs/current/tutorial-table.html

`CREATE TABLE` - Create a new table by specifying the table name, along with all column names and their types
```sql
CREATE TABLE weather (
    city            varchar(80),
    temp_lo         int,           -- low temperature
    temp_hi         int,           -- high temperature
    prcp            real,          -- precipitation
    date            date
);
```


`DROP TABLE tablename` - Remove a table


`INSERT` - Used to populate a table with rows
```sql
INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');

INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
    VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');
```


`COPY`  - Loads large amounts of data from flat-text file
```sql
COPY weather FROM '/home/user/weather.txt';
```