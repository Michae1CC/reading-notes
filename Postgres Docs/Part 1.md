
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


https://www.postgresql.org/docs/current/tutorial-join.html

`JOIN` - Combine rows from one table with rows from a second table, with an expression specifying which rows are to be paired.
```sql
SELECT * FROM weather JOIN cities ON city = name;
```


`LEFT OUTER JOIN`
```sql
SELECT *
    FROM weather AS w LEFT OUTER JOIN cities AS c ON w.city = c.name;
```


https://www.postgresql.org/docs/current/tutorial-agg.html

aggregate functions - computes a single result from multiple input rows. Cannot be used in the `WHERE` clause. This restriction exists because the `WHERE` clause determines which rows will be included in the aggregate calculation; so obviously it has to be evaluated before aggregate functions are computed.


`SUM()`, `COUNT()`, `AVG()`, `MAX()`, `MIN()` are the most common aggregate functions


```sql
SELECT max(temp_lo) FROM weather
```


`GROUP BY`
```sql
SELECT city, count(*), max(temp_lo)
    FROM weather
    GROUP BY city;
```


`HAVING`
```sql
SELECT city, count(*), max(temp_lo)
    FROM weather
    GROUP BY city
    HAVING max(temp_lo) < 40;
```

`FILTER` - a per-aggregate option
```sql
SELECT city, count(*) FILTER (WHERE temp_lo < 45), max(temp_lo)
    FROM weather
    GROUP BY city;
```
`FILTER` is much like `WHERE`, except that it removes rows only from the input of the particular aggregate function that it is attached to. Here, the `count` aggregate counts only rows with `temp_lo` below 45; but the `max` aggregate is still applied to all rows, so it still finds the reading of 46.


SQL Order of Operations

- **`FROM`**: The database locates the table and gathers the raw rows.
- **`WHERE`**: **The database filters the raw rows.** Any row that doesn't meet the condition is thrown out immediately.
- **`GROUP BY`**: The remaining rows are gathered and grouped together.
- _Aggregation Happens_: The database finally calculates the `SUM()`, `COUNT()`, or `AVG()` for those groups.
- **`HAVING`**: **The database filters the aggregated groups**.
- **`SELECT`**: The database determines which columns or calculated values to display.
- **`ORDER BY`**


`WHERE` vs `HAVING`
The fundamental difference between `WHERE` and `HAVING` is this: `WHERE` selects input rows before groups and aggregates are computed (thus, it controls which rows go into the aggregate computation), whereas `HAVING` selects group rows after groups and aggregates are computed. Thus, the `WHERE` clause must not contain aggregate functions; it makes no sense to try to use an aggregate to determine which rows will be inputs to the aggregates. On the other hand, the `HAVING` clause always contains aggregate functions. (Strictly speaking, you are allowed to write a `HAVING` clause that doesn't use aggregates, but it's seldom useful. The same condition could be used more efficiently at the `WHERE` stage.)