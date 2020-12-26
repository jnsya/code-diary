# Database Indexes
Sources:
- use-the-index-luke

## Overview
I spent some time reading about database indexes this off-season (from the online book use-the-index-luke) and thought I'd post some main things I learned here:

- Probably the most useful bit was understanding the anatomy of an index (they consist of two structures: a balanced tree, and a doubly-linked list of leaf nodes) and how that can cause poor performance. That's all explained in the first section here which I highly recommend: https://use-the-index-luke.com/sql/anatomy.

**Concatenated indexes are better**
- "Concatenated" indexes (ie, indexes on multiple columns) are preferable to single-column indexes.
- That's because concatenated indexes support simple queries on single columns AND multiple columns (as long as they meet the condition in the next point).
- A concatenated index needs to be used from left to right. That is, a concatenated index on columns A, B and C will support: queries on column A, queries on column A + B, queries on column A + B + C. It won't support queries on column B alone, or columns B + C alone.
- Column order is the most important choice when creating a concatenated index: you want it to be used by as many queries as possible.

**If you query with a function, index using that same function**
- To use an index for a query that uses case insensitive search (like `LOWER(some_string)`), the index needs to be explicitly defined with the same function that changes the case: `CREATE INDEX emp_down_name ON employees (LOWER(last_name))`).
- Anything inside the `LOWER()` function (or any function) is a black box to Postgres, so it won't recognise that the query can use the index unless they both use the same function.

**Range conditions can cause slow index scans**
- Index-only scans on range conditions (any query that uses greater than, less than, or between) can be slow if the range covers a large number of records.
- That's because accessing a range of consecutive index values  
- The solution is to index for equality first — then for ranges. So if you have an index on a state column and a date column, and we do queries on both, the state column should come first in any indexes if possible.

- There is myth that the most selective column should come first in a concatenated index. That's not true; the considerations above should have priority.

- Avoid `LIKE` queries with leading wildcards (eg `%TERM`). The later that a wildcard appears in the query string, the quicker an index scan which uses that column will be.

- Indexes can be a large proportion of the total table size. (For example, our `shipments` table is 29 GB, and 20 GB of that is the associated indexes.)

## INDEX STRUCTURE
- The index comprises two structures: the leaf nodes, and the b-tree.
  - the leaf nodes comprise a doubly-linked list.
  - 
- The *logical* order of the index is established by a doubly-linked list of leaf nodes.
- The b-tree (balanced tree) structure allows near-instant search (for unique records).
- Leaf Nodes connected as a doubly-linked list. That means, each leaf node has a pointer to the previous and next node.
- The leaf nodes are stored in a db page (aka block) - the db's smallest storage unit. As many index entries as possible are stored in each page.
- Each index entry stores the indexed column value, and the Row ID (ROWID or RID), which points to the row ID in the indexed table.
- The leaf nodes store the logical order of the indexed value, but they are not next to each other in disk space. To get from one leaf node to the next, you follow the pointer, but this operation can be slow since the leaf nodes are not physically next to each other. That's why you have the B-Tree.
- Each branch node of the B-Tree corresponds to the biggest value in its respective leaf node.

[See diagram](https://use-the-index-luke.com/static/fig01_01_index_leaf_nodes.en.MMHwYDFb.png)
[See diagram](https://use-the-index-luke.com/static/fig01_02_tree_structure.en.BdEzalqw.png)

## THREE PARTS OF AN INDEX LOOKUP:
(1) the tree traversal.
(2) following the leaf node chain.
(3) fetching the table data.

- Of these, only 1 has an upper-bound (ie guaranteed to be superfast).
- 2) and 3) can be slow.
- 2) will not be slow if query is constrained to 1 record, or if the field is subject to a primary key constraint (so every value is unique). If not constrained to 1 record, then db must sometimes read the next leaf node to see if there are more examples of that record.
- 3) will not need to happen if you make it an index-only scan.

### REASONS AN INDEX CAN BE SLOW:
1) Follow the leaf node chain
2) Fetching rows from the database

For example, if you query on an indexed field, but that query returns many rows, it might not be worthwhile to use that index - because both following the leaf node chain and returning the db rows might take a long time.

## UNDERSTANDING QUERY PLANS

*Types of Table Scan*:
- Seq Scan. Scans the entire table.
- Index Scan. B-tree traversal + follow leaf node chain + fetch table data. (fetches one tuple-pointer at a time from the index, and immediately visits that tuple in the table)
- Index Only Scan. B-Tree traversal + follow leaf node chain.
- Bitmap Index Scan / Bitmap Heap Scan / Recheck Cond. Fetches all tuple pointers from index in one go, sorts them by physical location on the disk, then visits the table tuples in physical location order. This reduces IO operations on the table.


Explanation of Hash Join, Nested Loop, Merge Join etc here: https://www.depesz.com/2013/05/09/explaining-the-unexplainable-part-3/

*COST / ROWS / WIDTH*:
Seq Scan on tenk1  (cost=0.00..458.00 rows=10000 width=244)

From left to right
`COST (1)`: Estimated start-up cost. This is the time expended before the output phase can begin, e.g., time to do the sorting in a sort node. The actual unit is in units of “time a sequential 8kb block read takes" (ie, a hypothetical cost).

`COST(2)`: Estimated total cost. This is stated on the assumption that the plan node is run to completion, i.e., all available rows are retrieved. In practice a node's parent node might stop short of reading all available rows (eg with LIMIT). The query plan with the lowest estimated startup cost is the one that will be chosen.

The difference between the two costs represents the difference between the startup cost of the operation and the cost of returning all rows that match the query after that startup cost. So a sort scan has a high startup cost, but the total cost will not be much higher (because once you've sorted the rows, it's easy to return them). A sequential scan will have a startup cost of zero (there is no startup cost - you just begin returning pages).

`ROWS`: Estimated number of rows output by this plan node. Again, the node is assumed to be run to completion.

`WIDTH`: Estimated average width of rows output by this plan node (in bytes). If you have a query which selects every column from a table vs a query that selects one column from a table, obviously the second query will have a lower width.

*INFORMATION FROM ANALYZE*
`(actual TIME=0.008..0.15 ROWS=100 loops=2)`

- `Time(1)`: Actual startup time in ms.
- `Time(2)`: Actual total time ms.
- `Rows`: Actual number of rows returned by this operation.
- `Loops`: Number of times this operation was run. So the *actual* total time for the operation above was `0.30ms`. (Poor performance often comes from looping often!). (Depesz actually multiplies the time by the loops for its exclusive and inclusive columns).

## CASE INSENSITIVE SEARCH
- If your query uses LOWER() or UPPER() to do a case insensitive search (eg `... WHERE LOWER(last_name) = LOWER("Senior")`), it won't use an index on last_name.
- Postgres does not recognise that the index on last name can be used for queries that ask for `LOWER(last_name)`.
- Everthing within the function (in the paranetheses) is a blackbox for Postgres.
- Instead, you need to explicitly set the index on LOWER(last_name): `CREATE INDEX emp_up_name ON employees (LOWER(last_name))`
- It's possible that you can only do this in a Rails migration using raw SQL: `execute "CREATE UNIQUE INDEX index_users_on_lowercase_last_name ON users (lower(last_name));`

## GREATER, LESS AND BETWEEN
- Rule of thumb: index for equality first — then for ranges.
- In this query for example, what should the index be on?
```
SELECT first_name, last_name, date_of_birth
  FROM employees
 WHERE date_of_birth >= TO_DATE(?, 'YYYY-MM-DD')
   AND date_of_birth <= TO_DATE(?, 'YYYY-MM-DD')
   AND subsidiary_id  = ?
```
- A: index on subsidiary_id THEN date_of_birth
- Why?: because we want to limit the scanned range as much as possible.
- If date_of_birth was first, more leaf nodes would have to be traversed. The leaf node traversal would start with the beginning of the date range and end with the end of the date range. Only
- To start with


## CONCATENATED INDEXES
- Can only be used if the index is in the correct order.
- To visualise the structure of a concatenated index, run `SELECT <list of columns that comprise the concatenated index in correct order> ORDER BY <same list as before> FETCH FIRST 100 ROWS ONLY;`.
  - For example: `SELECT state, tour_schedule_id FROM tours ORDER BY state, tour_schedule_id FETCH FIRST 100 ROWS ONLY;` (visualises what an index on columns state and tour_schedule_id for the tours table would look like)

## QUESTIONS
- Is there a Postgres action for "traverse B-Tree only" (without following leaf node chain)?

## MISC
- Heap == the database table.
