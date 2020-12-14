# HypoPG 

HypoPG is a Postgres extension for safely testing whether a new database index will be useful or not on your production database, without having to actually create the index.

It does this by creating "hypothetical" indexes, which don't interfere with real-life queries from your application, and only apply to `EXPLAIN` queries in the current session.

In my experience using HypoPG has two main benefits:
1. You deploy performance improvements much faster, with high confidence that they'll work.
1. You quickly learn more about indexing and the query planner (because you can experiment quickly, and without friction).

I'll talk about how to use HypoPG, go into more detail about the benefits, and include my personal cheatsheet at the end to get you started.

## Using hypoPG




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

## Cheatsheet
- List all hypothetical indexes, including their OID: `SELECT * FROM hypopg_list_indexes();`.
- Create a hypothetical index: `SELECT * FROM hypopg_create_index('CREATE INDEX ON table_name (column_name, column_name)');`.
- Remove one hypothetical index (via the index's OID): `SELECT * FROM hypopg_drop_index(oid);`
- Remove all hypothetical indexes:  `SELECT * FROM hypopg_reset();`

- Test whether a query uses the hypothetical index with `EXPLAIN`. Don't use `EXPLAIN ANALYZE`: it won't use hypothetical indexes with ANALYZE.
- Closing the session will delete all hypothetical indexes.
- hypopg_drop_index(oid): remove the given hypothetical index
- hypopg_reset(): remove all hypothetical indexes

