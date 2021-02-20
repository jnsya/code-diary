# Scaling Databases

## Questions
- Why can't we simply horizontally scale databases the same simple way we do on the web tier: add multiple servers with a load balancer?

## Overview
Strategies for scaling databases:
- _Vertical scaling_: bigger servers
- _Horizontal scaling_ : more servers
  - _Replication_: same data across multiple nodes in primary/replica relationship
  - _Sharding_: split data across multiple nodes
- _Denormalisation_: add redundant columns to reduce expensive JOIN queries

## Vertical scaling
- TLDR: "You’re going to need a bigger boat"
- Add more CPU, memory or storage.
- Advantages: easy + simple
- Disadvantages: low ceiling (there's only so big a single server can get)

## Horizontal Scaling

- Add more database nodes

### Database replication
- TLDR: A single database server is split into multiple servers. There are two kinds of server in this setup: primary (master) which handle writes, and replicas (slaves) which handle reads. Since most applications perform more reads than writes, this setup allows multiple queries to be processed in parallel.
- Advantages:
  - Performance: more queries can be processed in parallel.
  - Availability: more redundancy, because if a replica server goes down there are others to take its place; if a primary server goes down, a replica can be promoted to primary.

### Sharding / Horizontal Partitioning
- Split a large database into smaller parts, called _shards_
- Each shard shares the same schema, but stores a different subset of the data.
- the _sharding strategy_ determines how the data is split
  - The _sharding key_ is the column(s) on which you split the data
  - For example, for a user table with four shards, you could do `user_id % 4` to choose a shard. This is the strategy.
    - Here, `user_id` is the sharding key
- 

## Denormalisation
- A performance technique: add redundant columns to one table from another, to avoid having to JOIN query to get it.
- Example: a `subjects` table contains a `teacher_id` column. But we often JOIN the tables simply to get the teacher's name as well. We can add the teacher's name to the `subjects` table (which is redundant), to avoid having to run an expensive JOIN to get the name.
- Useful when the database is sharded, to avoid having to access columns from a different shard (which might be expensive).
 
## Sources
- [System Interview book](https://systeminterview.com/scale-from-zero-to-millions-of-users.php)
- [FreeCodeCamp: Understanding database scaling patterns](https://www.freecodecamp.org/news/understanding-database-scaling-patterns/)
