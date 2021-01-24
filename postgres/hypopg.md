# HypoPG 

HypoPG is a Postgres extension for safely testing whether a new index will be useful or not on your production database, without having to actually create the index.

It does this by creating "hypothetical" indexes, which don't interfere with real-life queries from your application, and which only apply to `EXPLAIN` queries in the current session.

In my experience using HypoPG has two main benefits:
1. You deploy performance improvements much faster, with high confidence that they'll work.
1. You learn more about indexing and the query planner (because you can experiment quickly, and without friction).

I'll talk about how to use HypoPG with a case study, go into more detail about the benefits we experienced at Liefery, and include my personal cheatsheet at the end to get you started.

## Using hypoPG

You'll need to install the extension on your production database following the installation instructions here. Luckily it doesn't require a database restart, unlike other extensions like pg_stat_statements.

It's important that you install it on production. The Postgres query planner is sensitive not only to the structure of your database, but also to the actual data that it contains. It's common for your Staging database table to be several orders of magnitude smaller than the same table on Production, and such a disparity makes a difference. A sequential scan (going through the table row by row) might be efficient for many queries if your Staging users table has a few hundred rows, but not for your Production users table with a few million rows.

## Case Study

Here's an example of how using HypoPG fits into a normal database analysis.

### Step 1: EXPLAIN the query

At Liefery, we noticed from our logs that we had a very common, very slow query on our shipments table:

`SELECT shipments.id FROM "shipments" WHERE "shipments"."is_quote" = false AND (courier_company_id = 476);`

The query fetches the ID of any non-quote (ie, real) shipment delivered by a particular courier company (`476` is just an example of a courier company ID - in reality there were multiple courier companies affected).

The first step is to get a query plan to see the planner's current strategy for executing the query.

We get the query plan for the query by navigating to the production database console and running the same query as above, but prefaced by `EXPLAIN`: `EXPLAIN ...`. Our example looked like this:

```
Bitmap Heap Scan on shipments  (cost=520.45..29424.47 rows=8261 width=4)
  Recheck Cond: (courier_company_id = 476)
  Filter: (NOT is_quote)
  ->  Bitmap Index Scan on index_shipments_on_courier_company_id  (cost=0.00..518.38 rows=8261 width=0)
        Index Cond: (courier_company_id = 476)
```

If you're new to query plans, the information can be overwhelming, but here's what's important for our purposes:
- The overall "cost" of the plan is 29424.47. 
- We already have a relevant index (`index_shipments_on_courier_company_id`), and the query plan begins by querying this index.
- Next, the plan returns to the database heap to check whether a shipment is a quote or not.
- The cost of using the index to filter by courier company ID (518.38) is relatively small compared to the total cost of the plan.

So to summarise: we have an index on 1 of the 3 of the columns needed for the query, but the query remains expensive.

Our reading of the query plan gave us an hypothesis: the current index would help more if it contained all the information required by the query. If we added is_quote to the index (creating a multi-column index), we could speed up the query.

### Step 2: Test our hypothesis with a hypothetical index

So, we want to see whether an index on `courier_company_id` AND `is_quote` will make this query faster.

To test it via hypoPG, first we need to create a hypothetical index:
```
SELECT * FROM hypopg_create_index('CREATE INDEX ON shipments (courier_company_id, is_quote)');
```

This might look a bit weird - it's structured as a SELECT query but the FROM clause is contains the instructions for creating the hyopothetical index. For us, `shipments` is the name of the table we want to add the index to, and everything in the parantheses (`courier_company`, `is_quote`) are the names of the columns to include in the index - in the order that they should be included.

So we run the command above to create the hypothetical index using hypoPG, then we run the `EXPLAIN` query again to see from the query plan whether the query would use the index. Here's the result from running it again:

```
 Bitmap Heap Scan on shipments  (cost=197.84..24554.47 rows=6808 width=4)
   Recheck Cond: (courier_company_id = 476)
   Filter: (NOT is_quote)
   ->  Bitmap Index Scan on <4999678>btree_shipments_courier_company_id_is_quote  (cost=0.00..196.13 rows=6808 width=0)
         Index Cond: ((courier_company_id = 476) AND (is_quote = false))
(5 rows)

```





Query plan with whole index
```
  Index Only Scan using <4999679>btree_shipments_courier_company_id_is_quote_id on shipments  (cost=0.06..15475.58 rows=6808 width=4)
   Index Cond: ((courier_company_id = 476) AND (is_quote = false))
   Filter: (NOT is_quote)
(3 rows)
```


So to summarise, hypoPG allowed us to quickly test whether a new index would be beneficial, and also what the best ordering for the columns would be.

### Step 3: Try out another index
- We realised that we might improve even more by making this an index-only scan.
- then the query wouldn't need to go to the database heap at all.

## Deploy faster
This can hugely speed up your development cycle.

At Liefery, before using HypoPG, our index deployment workflow looked like this:
1. Analzye a query plan using `EXPLAIN`
1. Guess what kind of index would improve this query.
1. Create a migration that adds this index, and push it in a PR.
1. Code review
1. Wait for deployment (1-8 days)
1. Check if the query was improved as expected. If not, repeat from step 2.

Workflow with HypoPG:
1. Analzye a query plan using `EXPLAIN`
1. Guess what kind of index would improve this query.
1. Check if the query was improved as expected. If not, repeat from step 2.


Obviously we still need to deploy an index once we find one that works, but now we only need to do so *after* we're confident that it's the best possible index for the job. 

One loop of the pre-HypoPG workflow took 1-8 days; afterwards it takes ~1 minute. By this I mean the time it takes to answer the question "Does this index significantly improve my query?", though of course even with HypoPG you still need to go through the PR review and deployment process before the index is speeding up real-life queries.

For the majority of my time at Liefery, we ran weekly deploys (including a 24 hour Staging freeze).

This is an example where we had an hypothesis whose assumptions were confirmed by hypoPG. But the opposite can also happen (and be just as useful): we think a new index would help, but hypoPG tells us it wouldn't. This saves us not only the deployment cycle, but a second cycle of having to remove the useless index.


## Drawbacks

There are some drawbacks to hypoPG: partial indexes not supported.


## Cheatsheet
- List all hypothetical indexes, including their OID: `SELECT * FROM hypopg_list_indexes();`.
- Create a hypothetical index: `SELECT * FROM hypopg_create_index('CREATE INDEX ON table_name (column_name, column_name)');`.
- Remove one hypothetical index (via the index's OID): `SELECT * FROM hypopg_drop_index(oid);`
- Remove all hypothetical indexes:  `SELECT * FROM hypopg_reset();`

- Test whether a query uses the hypothetical index with `EXPLAIN`. Don't use `EXPLAIN ANALYZE`: it won't use hypothetical indexes with ANALYZE.
- Closing the session will delete all hypothetical indexes.
- hypopg_drop_index(oid): remove the given hypothetical index
- hypopg_reset(): remove all hypothetical indexes

