https://www.postgresql.org/docs/18/indexes.html

- Suppose we have a table similar to
```sql
CREATE TABLE test1 (
    id integer,
    content varchar
);
```
- if the app wanted to issues queries of the form `SELECT content FROM test1 WHERE id = constant`, the system would have to scan the entire `test1` table
- The following command can be used to create an index on the `id` column
- Indexes can also benefit `UPDATE` and `DELETE` commands with search conditions
- Creating an index on a large table can take a long time
- PostgreSQL allows reads to occur on the table in parallel with index creation, but writes are blocked until the index build is finished
- After an index is created, the system has to keep it sync with the table, this adds overhead to data manipulation operations