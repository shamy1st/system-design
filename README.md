# System Design

## Fundamentals

### Key Characteristics of Distributed Systems

1. **Scalability** is the capability of a system, process, or a network to grow and manage increased demand.

   * **Horizontal vs. Vertical Scaling** Horizontal scaling means that you scale by adding more servers into your pool of resources whereas Vertical scaling means that you scale by adding more power (CPU, RAM, Storage, etc.) to an existing server.

   * Good example of horizontal scaling are **Cassandra** and **MongoDB**.
   * Good example of vertical scaling is **MySQL**.

![](https://github.com/shamy1st/system-design/blob/main/images/vertical-vs-horizontal-scaling.png)


2. **Reliability** a distributed system is considered reliable if it keeps delivering its services even when one or several of its software or hardware components fail.

   * For example, if a user has added an item to their shopping cart, the system is expected not to lose it. 


3. **Availability** is the percentage of time that a system, service, or a machine remains operational under normal conditions.

   * Availability takes into account maintainability, repair time, spares availability, and other logistics considerations.
   * If a system is reliable, it is available. However, if it is available, it is not necessarily reliable.


4. **Efficiency**

   * Let’s assume we have an operation that runs in a distributed manner and delivers a set of items as result.
   * Two standard measures of its efficiency:
      1. the **response time** (or latency) that denotes the delay to obtain the first item.
      2. the **throughput** (or bandwidth) which denotes the number of items delivered in a given time unit (e.g., a second).
   * The two measures correspond to the following unit costs:
      1. Number of messages globally sent by the nodes of the system regardless of the message size.
      2. Size of messages representing the volume of data exchanges.


5. **Manageability (Serviceability)** how easy it is to operate and maintain.

   * If the time to fix a failed system increases, then availability will decrease.
   * Things to consider for manageability:
      1. the ease of diagnosing and understanding problems when they occur.
      2. ease of making updates or modifications.
      3. how simple the system is to operate(i.e., does it routinely operate without failure or exceptions?).
   * Early detection of faults can decrease or avoid system downtime.


### Load Balancing

  * **Load Balancer**: helps to spread the traffic across a cluster of servers to improve responsiveness and availability of applications, websites or databases.
    * LB also keeps track of the status of all the resources while distributing requests.
    * If a server is not available to take new requests or is not responding or has elevated error rate, LB will stop sending traffic to such a server.
    * Typically a load balancer sits between the client and the servers.

![](https://github.com/shamy1st/system-design/blob/main/images/load-balancer-places.png)

  * Benefits of Load Balancing
    * Users experience faster.
    * Service providers experience less downtime and higher throughput.
    
  * Load Balancing Algorithms
    * Least Connection Method
    * Least Response Time Method
    * Least Bandwidth Method
    * Round Robin Method
    * Weighted Round Robin Method
    * IP Hash
    
  * Redundant Load Balancers
    * The load balancer can be a single point of failure; to overcome this, a second load balancer can be connected to the first to form a cluster.
![](https://github.com/shamy1st/system-design/blob/main/images/load-balancer-redundant.png)

### Caching

  * A cache is like short-term memory: it has a limited amount of space, but is typically faster than the original data source and contains the most recently accessed items.
  * Caches take advantage of the locality of **reference principle**: recently requested data is likely to be requested again.
  * They are used in almost every layer of computing: hardware, operating systems, web browsers, web applications, and more.
  * **Content Distribution Network (CDN)**
    * CDNs are a kind of cache that comes into play for sites serving large amounts of static media.
    * If the system we are building isn’t yet large enough to have its own CDN, we can use a separate subdomain (e.g. static.yourservice.com) using a lightweight HTTP server like Nginx.
  * **Cache Invalidation** if the data is modified in the database, it should be invalidated in the cache; if not, this can cause inconsistent application behavior.
    * three main schemes used to solve this problem:
      1. **Write-through cache** data is written into the cache and the corresponding database at the same time.
      2. **Write-around cache** data is written directly to permanent storage, bypassing the cache.
      3. **Write-back cache** data is written to cache alone and completion is immediately confirmed to the client.
  * **Cache Eviction Policies**
    1. **First In First Out (FIFO)**: The cache evicts the first block accessed first without any regard to how often or how many times it was accessed before.
    2. **Last In First Out (LIFO)**: The cache evicts the block accessed most recently first without any regard to how often or how many times it was accessed before.
    3. **Least Recently Used (LRU)**: Discards the least recently used items first.
    4. **Most Recently Used (MRU)**: Discards, in contrast to LRU, the most recently used items first.
    5. **Least Frequently Used (LFU)**: Counts how often an item is needed. Those that are used least often are discarded first.
    6. **Random Replacement (RR)**: Randomly selects a candidate item and discards it to make space when necessary.

### Data Partitioning

  * **Data partitioning** is a technique to break up a big database (DB) into many smaller parts.
  * **Partitioning Methods** 
    1. **Horizontal partitioning** put different rows into different tables.
        * For example, if we are storing different places in a table, we can decide that locations with ZIP codes less than 10000 are stored in one table and places with ZIP codes greater than 10000 are stored in a separate table.
        * also called **Data Sharding**
        * The key problem with this approach is that if the value whose range is used for partitioning isn’t chosen carefully, then the partitioning scheme will lead to unbalanced servers.
    2. **Vertical Partitioning** divide our data to store tables related to a specific feature in their own server.
        * For example, if we are building Instagram like application - where we need to store data related to users, photos they upload, and people they follow - we can decide to place user profile information on one DB server, friend lists on another, and photos on a third server.
        * The main problem with this approach is that if our application experiences additional growth, then it may be necessary to further partition a feature specific DB across various servers (e.g. it would not be possible for a single server to handle all the metadata queries for 10 billion photos by 140 million users).
    3. **Directory Based Partitioning**
        * A loosely coupled approach to work around issues mentioned in the above schemes is to create a lookup service which knows your current partitioning scheme and abstracts it away from the DB access code.
        * So, to find out where a particular data entity resides, we query the directory server that holds the mapping between each tuple key to its DB server.
  * **Partitioning Criteria**
    * Key or Hash-based partitioning
    * List partitioning
    * Round-robin partitioning
    * Composite partitioning
  * **Common Problems of Data Partitioning**
    * Joins and Denormalization
    * Referential integrity
    * Rebalancing

### Indexes

  * The goal of creating an index on a particular table in a database is to make it faster to search through the table and find the row or rows that we want.
  * An index can dramatically speed up data retrieval but may itself be large due to the additional keys, which slow down data insertion & update.
  * When adding rows or making updates to existing rows for a table with an active index, we not only have to write the data but also have to update the index.
  * This will decrease the write performance. This performance degradation applies to all insert, update, and delete operations for the table.
  * For this reason, adding unnecessary indexes on tables should be avoided and indexes that are no longer used should be removed.

### Proxies

  * A proxy server is an intermediate server between the client and the back-end server.
  * Typically, proxies are used to filter requests, log requests, or sometimes transform requests (by adding/removing headers, encrypting/decrypting, or compressing a resource).
  * Another advantage of a proxy server is that its cache can serve a lot of requests.
  * If multiple clients access a particular resource, the proxy server can cache it and serve it to all the clients without going to the remote server.
  
  * **Proxy Server Types**
    * **Open Proxy** is a proxy server that is accessible by any Internet user.
      1. **Anonymous Proxy** reveаls іts іdentіty аs а server but does not dіsclose the іnіtіаl IP аddress.
      2. **Trаnspаrent Proxy** іdentіfіes іtself, аnd wіth the support of HTTP heаders, the fіrst IP аddress cаn be vіewed. The mаіn benefіt of usіng thіs sort of server іs іts аbіlіty to cаche the websіtes.
    * **Reverse Proxy** retrieves resources on behalf of a client from one or more servers. These resources are then returned to the client, appearing as if they originated from the proxy server itself.

### Redundancy and Replication

### SQL vs. NoSQL

### CAP Theorem

### Long-Polling vs WebSockets vs Server-Sent Events

## Problems
* System Design Interviews: A step by step guide
* Designing a URL Shortening service like TinyURL
* Designing Pastebin
* Designing Instagram
* Designing Dropbox
* Designing Facebook Messenger
* Designing Twitter
* Designing Youtube or Netflix
* Designing Typeahead Suggestion
* Designing an API Rate Limiter
* Designing Twitter Search
* Designing a Web Crawler
* Designing Facebook’s Newsfeed
* Designing Yelp or Nearby Friends
* Designing Uber backend
* Design Ticketmaster
