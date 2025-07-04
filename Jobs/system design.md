
- [ByteByteGo System Design](https://bytebytego.com/courses/system-design-interview/foreword)
- [Hired in Tech](https://www.hiredintech.com/system-design/)
- https://youtu.be/ZgdS0EUmn70 
- [Leetcode template](https://leetcode.com/discuss/career/229177/My-System-Design-Template)
- [Back of envelop](https://highscalability.com/google-pro-tip-use-back-of-the-envelope-calculations-to-choo/)
- [Questions and Solutions](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#system-design-interview-questions-with-solutions)
- [OOP Questions](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#object-oriented-design-interview-questions-with-solutions)
- [Donne Martin collections](https://github.com/donnemartin/system-design-primer)


## Approach


  -  Requirement clarification (5 mins) - 
	  - Use cases
		  - Whats in scope
		  - what is not in scope
	  - user/costumers
		  - Who will use
		  - how it will be used
		  - frequency of use
	  - Scale (read/write)
		  - How many reads/write per second
		  - How many users per month or day
		  - what is the peak traffic
		  - How much data
	  - Performance
		  - write to read delay
		  - Expected latency
	  - Cost/resources restrictions
		  - Hardware restrictions
		  - time restrictions
		  - Storage restrictions
		  - Limited budget
- Non functional (5 mins)  
	- Latency and Throughput requirements 
	- Consistency vs Availability  (Weak/strong/eventual => consistency | Failover/replication => availability)
	- Scalable
	- Available
	- Performant
	- Consistency
	- Accuracy
	- Durability
	- Freshness
	- Compliance and Regulations
	- Security
	- Logging and monitoring
- Estimation (5 mins)  
	- Throughput (QPS for read and write queries) 
	- Latency expected from the system (for read and write queries) 
	- Read/Write ratio 
	- Traffic estimates 
		- Write (QPS, Volume of data) 
		- Read  (QPS, Volume of data) 
	- Storage estimates 
	- Memory estimates 
		- If we are using a cache, what is the kind of data we want to store in cache 
		- How much RAM and how many machines do we need for us to achieve this ? 
		- Amount of data you want to store in disk/ssd
- High Level Design (5-10 mins)
	- APIs for read/write scenarios for crucial components/features
	- Database schema 
	- Basic algorithm 
	- High level design for Read/Write scenario
- Design Deep Dive (15-20 mins)
	- Scaling the algorithm 
	- Scaling individual components:  
		- Availability, Consistency and Scale story for each component 
		- Consistency and availability patterns
	- Think about the following components, how they would fit in and how it would help 
		- DNS 
		- CDN [Push vs Pull]  
		- Load Balancers [Active-Passive, Active-Active, Layer 4, Layer 7]  
		- Reverse Proxy  
		- Application layer scaling [Microservices, Service Discovery]  
		- RDBMS - Master-slave, Master-master, Federation, Sharding, Denormalization, SQL Tuning  
			- Structured data
			- Strict schema
			- Relational data
			- Need for complex joins
			- Transactions
			- Clear patterns for scaling
			- More established: developers, community, code, tools, etc
			- Lookups by index are very fast
		- NoSQL
			- Key-Value - Redis, memcached, DynamoDB
			- Wide-Column - HBase, BigTable, Cassandra
			- Graph - Neo4j, FlockDB
			- Document - DynamoDB, mongoDB, CouchDB, elastic search
		- NoSQL characterstics
			- Semi-structured data
			- Dynamic or flexible schema
			- Non-relational data
			- No need for complex joins
			- Store many TB (or PB) of data
			- Very data intensive workload
			- Very high throughput for IOPS
		- Sample data well-suited for NoSQL:
			- Rapid ingest of clickstream and log data
			- Leaderboard or scoring data
			- Temporary data, such as a shopping cart
			- Frequently accessed ('hot') tables
			- Metadata/lookup tables
		- Fast-lookups: 
			- RAM  [Bounded size] => Redis, Memcached 
			- AP [Unbounded size] => Cassandra, RIAK, Voldemort 
			- CP [Unbounded size] => HBase, MongoDB, Couchbase, DynamoDB
		- Caches
			- Eviction policies:  
				- Cache aside  
				- Write through  
				- Write behind  
				- Refresh ahead
			- CDN
			- Client side (Browser, mobile) cache
			- Load balance caching
			- RDBMS - Write ahead logs, Transaction logs, Materialized view, Buffer pool, replication log
			- In-memory - redis
			- Indexed data - elastic search
			- Issues
				- Cache stampede - large number of request for a resource that's not in cache
				- Cache crash - all the request goes to database after the crash
				- Cache avalanche - delay in one response causes more number of refresh rate from user
				- 
		- Database
			- How to scale read/write
			- how to make read/write fast
			- Handle hardware partitions, network failures
			- consistency trade offs
			- data security
			- data recovery
			- cost
			- data model extensibility
			- where to run (cloud, on-prem)

- Walkthrough ( 5 mins)
	- Go through a sample flow
	- Explain how the non-functional requirements are achieved
	- Throughput and latency between each component/layer
	- Trade-offs
	- Corner scenarios

Common solutions
- Reliability, durability - Replicate data
- Consistent hashing for partitioning data/nodes/sharding
- Check for existence - bloom filter
- Counting large data - Count min algo
- Availability - load balancer, redundancy
- Scalability - partitioning
- Fast access/Read heavy - caching, CDN
- Write heavy - async write, LSM tree
- Large Files - Object storage
- Data processing - Stream processing, Batch processing
- Fault tolerants system
	- replication
	- redundancy
	- fail over
	- load balance
	- graceful degradation
	- monitoring and alerting
- Scaling
	- Decentralize components - loose coupling
	- Stateless components
	- Async processing



- SQL vs NoSQL
	- normalized data
	- ACID (atomicity, Consistency, Isolation, Durability), BASE (Basically available, Soft State, Eventual consistency)
	- DB types - graph, key-value, column, document
- Storage
	- Hot storage
	- Cold storage
	- Object storage
	- File storage
	- In-memory storage
- Single vs multiple parititon
	- Consistency - read/write qurom
- Service discovery
	- Leader election
	- Gossip protocol
- Parallelize/single queue
	- synchronization
	- Delivery mechanism - at least once, exact once, at most once
	- Deduplication
	- dead letter queue
	- Push vs Pull messages
	- Maintain FIFO 
- Client requests
	- Blocking vs Non-blocking IO
	- Buffering/batching of requests/data
	- Retries/timeouts - Exponential backoff and jitter
	- Circuit breaker
- Load balancers
	- Software vs hardware
	- Network protocols - TCP vs HTTP
	- Balancing algo
		- Round robin
		- Sticky RR
		- Weighted round robin
		- Geo hash
		- Custom (IP, URL) Hash
		- Least time
		- Least connections
	- Health checks
- Reverse proxy
	- Request validation
	- AuthZ/AuthN
	- TLS termination
	- Server side encryption
	- Caching
	- Rate limiting (Throttling)
		- Leaky bucket
		- Token bucket
		- Sliding window
		- Fixed window
	- Request dispatch
	- Request deduplication
	- Usage data collection
- Message formats
	- Text - Json, XML
	- binary - gRPC
- API types
	- REST
	- SOAP
	- Graphql
	- gRPC
	- WebSocket
	- Webhook
- API best practices
	- Versioning
	- Security
	- Rate limiting
	- Documentation
	- Idempotency where matters
- Increasing API performance
	- Indexing
	- Caching
	- Rate limiting
	- Load balancing
	- pagination
	- Connection pool
	- Avoid N+1 problem - fetch 1 response and make n request for items from the response
	- Data compression
	- Fast Serialization
	- Async logging
- Distributed patters
	- Ambassador - delegate task
	- Circuit pattern - stop request dispatch if server is unavailable
	- CQRS - command Query Responsibility Segregation, separate service for read and write requests
	- Leader election
	- Sharding
	- Publisher subscriber
- Deployment strategy
	- Rolling
	- Canary
	- Blue- green
	- Big bang
	- Feature toggle
- Useful Algos
	- Consistent hashing
	- Geohash
	- Quadtree
	- Leaky bucket
	- Token bucket
	- Trie
	- Bloom filter
	- Paxos/Raft

## Trade-offs
- Strong vs Eventual consistency
- SQL vs NoSQL
- Batch vs Stream processing
- Normalization vs Denormalization
- Consistency vs Availability
- Stateful vs Stateless
- REST vs Graphql
- Read through vs write through cache
- sync vs async processing
- vertical vs horizontal scaling


## Tools

- Kafka
	- Append only write
	- Zero copy write
- password - hash + salt
- Redis
	- In memory database
	- Support for custom objects
	- single threaded, IO multiplexing
- Database Data structures
	- Skip list
	- BTree
	- Hash index - key value pair
	- Inverted index - word to document indexing
	- SSTable
	- Suffix tree - 
	- LSM Tree - Cassandra write using Sorted sets
	- R-Tree - geohashing
- 