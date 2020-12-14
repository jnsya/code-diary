# HypoPG 

HypoPG is a Postgres extension for safely testing whether a new database index will be useful or not on your production database, without having to actually create the index.

It does this by creating "hypothetical" indexes, which don't interfere with real-life queries from your application, and only apply to `EXPLAIN` queries in the current session.

In my experience using HypoPG has two main benefits:
1. You deploy performance improvements much faster, with high confidence that they'll work.
1. You learn more about indexing and the query planner (because you can experiment quickly, and without friction).

I'll talk about how to use HypoPG, go into more detail about the benefits we experienced at Liefery, and include my personal cheatsheet at the end to get you started.

## Using hypoPG

You'll need to install the extension on your production database following the installation instructions here. Luckily it doesn't require a database restart, unlike other extensions like pg_stat_statements.

It's important that you install it on production. The Postgres query planner is sensitive not only to the structure of your database, but also to the actual data that it contains. It's common for your Staging database table to be several orders of magnitude smaller than the same table on Production, and such a disparity makes a difference. A sequential scan (going through the table row by row) might be efficient for many queries if your Staging users table has a few hundred rows, but not for your Production users table with a few million rows.

## Case Study

At Liefery, we noticed from our logs that we had a very common, very slow query on our shipments table:

`SELECT shipments.id FROM "shipments" WHERE "shipments"."is_quote" = false AND (courier_company_id = 476);`

The query is getting the ID of any non-quote (ie, real) shipment delivered by a particular courier company (`476` is just an example of a courier company ID - in reality there were multiple courier companies affected).

The first step is to get a query plan to see the planner's current strategy for executing the query.

We get the query plan for the query by navigating to the production database console and running the same query as above, but prefaced by `EXPLAIN`: `EXPLAIN ...`. Our example looked like this:

```
Bitmap Heap Scan on shipments  (cost=520.45..29424.47 rows=8261 width=4)
  Recheck Cond: (courier_company_id = 476)
  Filter: (NOT is_quote)
  ->  Bitmap Index Scan on index_shipments_on_courier_company_id  (cost=0.00..518.38 rows=8261 width=0)
        Index Cond: (courier_company_id = 476)
```

If you're new to query plans, the information can be overwhelming, but here are the key pieces of information for our purposes:
- The overall "cost" of the plan is 29424.47. 
- We already have a relevant index (`index_shipments_on_courier_company_id`), and using the index to filter by courier company ID is the first thing the query plan does 
- After filering using the index, the plan returns to the database heap to check whether a shipment is a quote or not
- The cost of using the index to filter by courier company ID (518.38) is much lower than the total cost of the plan

So to summarise: The index does not contain all the information required by the query, so it returns 

Our reading of the query plan gives us a hypothesis: the current index is helping, but it would help more if it contained all the information required by the query. If we added the remaining columns to the index (creating a multi-column index), thenthe query wouldn't need to go to the database heap at all.

But how do we test this hypothesis? Prior to hypoPG, we would have had to create the index, deploy it, then see what happens.

To test it via hypoPG, first we need to create a hypothetical index:
```
SELECT * FROM hypopg_create_index('CREATE INDEX ON shipments (courier_company_id, is_quote, id)');
```

This might look a bit weird - it's structured as a SELECT query but the FROM clause is contains the instructions for creating the hyopothetical index. For us, `shipments` is the name of the table we want to add the index to, and everything in the parantheses (`courier_company`, `is_quote` and `id`) are the names of the columns to include in the index - in the order that they should be included.

So to summarise, hypoPG allowed us to quickly test whether a new index would be beneficial, and also what the best ordering for the columns would be.

## Deploy faster
This can hugely speed up your development cycle.

At Liefery, before using HypoPG, our index deployment workflow looked like this:
1. Analzye a query plan using `EXPLAIN`
1. Guess what kind of index would improve this query.
1. Create a migration that adds this index, and push it in a PR.
1. Code review
1. Wait for deployment
1. Check if the query was improved as expected. If not, repeat from step 2.

Workflow with HypoPG:
1. Analzye a query plan using `EXPLAIN`
1. Guess what kind of index would improve this query.
1. Check if the query was improved as expected. If not, repeat from step 2.

One loop of the pre-HypoPG workflow took 1-8 days; afterwards it takes ~1 minute. By this I mean the time it takes to answer the question "Does this index significantly improve my query?", though of course even with HypoPG you still need to go through the PR review and deployment process before the index is speeding up real-life queries.

For the majority of my time at Liefery, we ran weekly deploys (including a 24 hour Staging freeze).

This is an example where we had an hypothesis whose assumptions were confirmed by hypoPG. But the opposite can also happen (and be just as useful): we think a new index would help, but hypoPG tells us it wouldn't. This saves us not only the deployment cycle, but a second cycle of having to remove the useless index.


## Drawbacks

There are some drawbacks to hypoPG


## Cheatsheet
- List all hypothetical indexes, including their OID: `SELECT * FROM hypopg_list_indexes();`.
- Create a hypothetical index: `SELECT * FROM hypopg_create_index('CREATE INDEX ON table_name (column_name, column_name)');`.
- Remove one hypothetical index (via the index's OID): `SELECT * FROM hypopg_drop_index(oid);`
- Remove all hypothetical indexes:  `SELECT * FROM hypopg_reset();`

- Test whether a query uses the hypothetical index with `EXPLAIN`. Don't use `EXPLAIN ANALYZE`: it won't use hypothetical indexes with ANALYZE.
- Closing the session will delete all hypothetical indexes.
- hypopg_drop_index(oid): remove the given hypothetical index
- hypopg_reset(): remove all hypothetical indexes

