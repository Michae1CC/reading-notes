
pg 623

- Ordered Indices - Based on a sorted ordering of the values
- Hash indices - Based on a uniform distribution of values across a range of buckets. The bucket to which a value is assigned is determined by a function, called a hash
- Clustering Index - An index whose search key also defines the sequential order
- Non-clustering Indices - Indices whose search key specifies an order different from the sequential order of the file
- Search key - an attribute or set of attributes used to look up records in a file
- Index Entry, Index Record - Consists of a search-key value and pointers to one or more records with that value as their search-key value
- Dense index - An index entry appears for every search-key value in that file
- Sparse index - An index entry appears for only some of the search-key values