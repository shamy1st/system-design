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

### Caching

### Data Partitioning

### Indexes

### Proxies

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
