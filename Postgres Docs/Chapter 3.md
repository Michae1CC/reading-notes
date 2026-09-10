
https://www.postgresql.org/docs/current/tutorial-advanced.html

Views - gives a name to a query that you can refer to like an ordinary table
```sql
CREATE VIEW myview AS
    SELECT name, temp_lo, temp_hi, prcp, date, location
        FROM weather, cities
        WHERE city = name;

SELECT * FROM myview;
```

Foreign keys
```sql
CREATE TABLE cities (
        name     varchar(80) primary key,
        location point
);

CREATE TABLE weather (
        city      varchar(80) references cities(name),
        temp_lo   int,
        temp_hi   int,
        prcp      real,
        date      date
);
```

#### Transactions

https://www.postgresql.org/docs/current/tutorial-transactions.html

- The essential point of a transaction is that it bundles multiple steps into a single all-or-nothing operation. The intermediate states between the steps are not visible to the other concurrent transactions, and if some failure occurs that prevents the transaction from completing, then none of the steps affect the database
- A transaction is set up by surrounding the SQL commands of the transaction with `BEGIN` and `COMMIT` commands, so our banking would actually look like:
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
-- etc etc
COMMIT;
```