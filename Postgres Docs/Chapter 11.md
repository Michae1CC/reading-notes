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

### Index Types

- PostgreSQL provides several index types. Each index type uses a different algorithm that is best suited to different types of indexable clauses. By default a B-tree index is created which fits most common situations
- Other index types are selected by writing the keyword `USING` followed by the index type name
- B-tree index: can handle equality and range queries on data that can be sorted into some ordering
- Hash index: can only handle simple equality comparisons
- Gist index: not a single kind of index, but rather an infrastructure within which many different strategies can be implemented. Optimized for "nearest-neighbor" searches
- SP-Gist: support nearest neighbour searches
- GIN: "inverted indexes" which are appropriate for data values that contain multiple component values, such as arrays. Can efficiently handle queries that test for the presence of specific component values
- BRIN: Most effective for columns whose values are well-correlated with the physical order of the table rows

### Multicolumn Indexes

- An index can be defined on more than one column
- If queries of the following form are frequently issued
```sql
SELECT name FROM test2 WHERE major = constant AND minor = constant;
```
- then it might be appropriate to define an index on the columns `major` and `minor` together
```sql
CREATE INDEX test2_mm_idx ON test2 (major, minor);
```
- Only B-tree, GiST, GIN and BRIN index types support multiple-key-column indexes


### Indexes and `ORDER BY`

- Only B-tree can produce sorted output - the other index types return matching rows in an unspecified, implementation-dependent order
- By default, B-tree indexes store their entries in ascending order with nulls last. This means that a forward scan of an index on a column `x` produces output satisfying `ORDER BY x ASC NULLS LAST`

### Combining Multiple Indexes

- A single index scan can only use query clauses that use the index's columns with operators class and are joined with `AND`
- Postgres has the ability to combine multiple indexes to handle cases that cannot be implemented by a single index scans. The system can form `AND` and `OR` conditions across several index scans. For example, a query like `WHERE x = 42 OR x = 47 OR x = 53 OR x = 99` could be broken down into four separate scans of an index on `x`, each scan using one of the query clauses. The results of these scans are then ORed together to produce the result

### Unique Indexes

- Indexes can also be used to enforce uniqueness of a column's value, or the uniqueness of the combined values of more than one column
- Only B-tree indexes can be declared unique
- When an index is declared unique, multiple rows with equal indexed values are not allowed
- Null values values in a unique column are not considered equal, allowing multiple nulls in the column
- The `NULLS NOT DISTINCT` option modifies this and causes the index to treat nulls as equal
- PostgreSQL automatically creates a unique index when a unique constraint or primary key is defined for a table. The index covers the columns that make up the primary key or unique constraint and is the mechanism that enforces the constraint