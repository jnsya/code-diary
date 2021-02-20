# Key Concepts

## Questions
- What's the difference between RAM and CPU? What metrics would we use to tell us we needed more of one or the other?
- What are the key metrics that determine which level of scaling is appropriate? And what are those levels?
  - eg: how much data/requests per second would we need to have before a single database server wouldn't be enough? What about when replication wouldn't be enough, and we'd need to consider sharding?
  - 

## Terminology
- _Vertical Scaling_
- _Horizontal Scaling_
- _Caching_
- _CDN_
- _Database replication_
- _Load balancer_
- _Web tier_
- _Data tier_
- _Cache tier_
- _Stateless architecture_
- _Data centers_
  - _Geo-routing_
- _Sharding_


## Horizontal Scaling
- Most basic form: add another server, and a _load balancer_ to direct traffic evenly between the servers.
- A "traffic cop" that sits in front of your servers, and routes client traffic between them.
- Advantages of a load balancer:
  - distributes load evenly across servers
  - ensures high availability by sending requests only to online servers

## Caching
- Accessing cached data is faster than reading from the database, because it's usually stored in RAM - and it's faster to read from RAM than from the disk. 
- Caches can be applied at every technology layer: OS, networking (eg CDNs), application, and database.
- The _caching strategy_ decides what data gets cached.
  - The correct caching strategy depends on the use-case: eg write-heavy, read-heavy, only unique records, etc.

## CDN
- Content Delivery Network
- TLDR: store your static assets (images, videos, JS, CSS, etc) on an external server that's geographically closer to your user. Makes it faster to serve them static content when they ask for it, and reduce server load.
- 
