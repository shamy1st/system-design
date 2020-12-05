# System Design

## Step by Step Guide
1. Requirements
2. Design Considerations
3. Estimation
4. High-level Design
5. Database Model
6. System Interface
7. Low-level Design
8. Bottlenecks

## Design

### Easy
1. TinyURL 
2. Pastebin

### Medium
3. [Instagram](https://github.com/shamy1st/system-design-instagram)
4. Dropbox
5. Facebook Messenger
6. Twitter
7. Youtube or Netflix
8. Typeahead Suggestion
9. API Rate Limiter
10. Twitter Search

### Hard
11. Web Crawler
12. Facebook’s Newsfeed
13. Yelp or Nearby Friends
14. Uber backend
15. Ticketmaster

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

  * **Redundancy** is the duplication of critical components or functions of a system with the intention of increasing the reliability of the system, usually in the form of a backup or fail-safe, or to improve actual system performance.
    * For example, if there is only one copy of a file stored on a single server, then losing that server means losing the file.
    * Since losing data is seldom a good thing, we can create duplicate or redundant copies of the file to solve this problem.
    * Redundancy plays a key role in removing the single points of failure in the system and provides backups if needed in a crisis.
    * For example, if we have two instances of a service running in production and one fails, the system can failover to the other one.
    
    ![](https://github.com/shamy1st/system-design/blob/main/images/replication.png)
    
  * **Replication** means sharing information to ensure consistency between redundant resources, such as software or hardware components, to improve reliability, fault-tolerance, or accessibility.
    * Replication is widely used in many database management systems (DBMS), usually with a master-slave relationship between the original and the copies.
    * The master gets all the updates, which then ripple through to the slaves. Each slave outputs a message stating that it has received the update successfully, thus allowing the sending of subsequent updates.
  

### SQL vs. NoSQL

  * Relational databases are structured and have predefined schemas like phone books that store phone numbers and addresses.
  * Non-relational databases are unstructured, distributed, and have a dynamic schema like file folders that hold everything from a person’s address and phone number to their Facebook ‘likes’ and online shopping preferences.
  
  * **SQL** like MySQL, Oracle, MS SQL Server, SQLite, Postgres, and MariaDB.
  
  * **NoSQL** most common types:
    1. **Key-Value Stores** like Redis, Voldemort, and Dynamo.
    2. **Document Databases** like CouchDB and MongoDB.
    3. **Wide-Column Databases** like Cassandra and HBase.
    4. **Graph Databases** like Neo4J and InfiniteGraph.

  * **Differences between SQL and NoSQL**
    * Storage
    * Schema
    * Querying
    * Scalability
    * Reliability or ACID Compliancy (Atomicity, Consistency, Isolation, Durability)

  * **Reasons to use SQL database**
    * We need to ensure ACID compliance.
    * Your data is structured and unchanging.
    
  * **Reasons to use NoSQL database**
    * Storing large volumes of data that often have little to no structure.
    * Making the most of cloud computing and storage.
    * Rapid development.

### CAP Theorem

  * **CAP Theorem** states that it is impossible for a distributed software system to simultaneously provide more than two out of three of the following guarantees (CAP): Consistency, Availability, and Partition tolerance.
  
  * CAP theorem says while designing a distributed system we can pick only two of the following three options:
    * **Consistency** All nodes see the same data at the same time. Consistency is achieved by updating several nodes before allowing further reads.
    * **Availability** Every request gets a response on success/failure. Availability is achieved by replicating the data across different servers.
    * **Partition Tolerance** The system continues to work despite message loss or partial failure.

![](https://github.com/shamy1st/system-design/blob/main/images/cap-theorem.png)

  * We cannot build a general data store that is continually available, sequentially consistent, and tolerant to any partition failures.
  * We can only build a system that has any two of these three properties.
  * Because, to be consistent, all nodes should see the same set of updates in the same order.
  * But if the network suffers a partition, updates in one partition might not make it to the other partitions before a client reads from the out-of-date partition after having read from the up-to-date one.
  * The only thing that can be done to cope with this possibility is to stop serving requests from the out-of-date partition, but then the service is no longer 100% available.


### Consistent Hashing

  * Distributed Hash Table (DHT) is one of the fundamental components used in distributed scalable systems.
  * Hash Tables need a key, a value, and a hash function where hash function maps the key to a location where the value is stored.
  
        index = hash_function(key)
        
  * Suppose we are designing a distributed caching system. Given ‘n’ cache servers, an intuitive hash function would be ‘key % n’.
  * It is simple and commonly used. But it has two major drawbacks:
    1. It is NOT horizontally scalable. Whenever a new cache host is added to the system, all existing mappings are broken.
    2. It may NOT be load balanced, especially for non-uniformly distributed data.
  * In such situations, consistent hashing is a good way to improve the caching system.
  * **Consistent hashing** is a very useful strategy for distributed caching system and DHTs.
  * It allows us to distribute data across a cluster in such a way that will minimize reorganization when nodes are added or removed.
  * Hence, the caching system will be easier to scale up or scale down.
  * In Consistent Hashing, when the hash table is resized (e.g. a new cache host is added to the system), only ‘k/n’ keys need to be remapped where ‘k’ is the total number of keys and ‘n’ is the total number of servers.
  * Recall that in a caching system using the ‘mod’ as the hash function, all keys need to be remapped.
  * In Consistent Hashing, objects are mapped to the same host if possible.
  * When a host is removed from the system, the objects on that host are shared by other hosts; when a new host is added, it takes its share from a few hosts without touching other’s shares.
  * How does it work?
    * As a typical hash function, consistent hashing maps a key to an integer. Suppose the output of the hash function is in the range of \[0, 256).
    * Imagine that the integers in the range are placed on a ring such that the values are wrapped around.
    * Given a list of cache servers, hash them to integers in the range.
    * To map a key to a server, 
      * Hash it to a single integer.
      * Move clockwise on the ring until finding the first cache it encounters.
      * That cache is the one that contains the key.
    * To add a new server, say D,
      * keys that were originally residing at C will be split.
      * Some of them will be shifted to D
      * while other keys will not be touched.
    * To remove a cache or, if a cache fails, say A,
      * all keys that were originally mapped to A will fall into B
      * and only those keys need to be moved to B
      * other keys will not be affected.


### Long-Polling vs WebSockets vs Server-Sent Events
