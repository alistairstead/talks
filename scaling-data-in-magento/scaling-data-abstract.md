# Scalable and highly available data in Magento

## Focus ##

This presentation discusses scaling through replication and how this impacts your infrastructure, deployment and implementation. Replication brings benefits and some pitfalls. Using simple solutions; MySQL replication or Tungsten to achieve high availability and high performance data storage. Methods to avoiding database failure or in the event of issue recovery scenarios and solutions.

### Key Points ###

- Introduction to database scaling issues.
- Technical solution details for MySQL and Tungsten.
- Common pitfalls and solutions.
- Connection, deployment and data management.
- Failure and recovery scenarios.
- Case study of the impact and real statistic.

### Speaking Experience ###

- Magento Imagine 2011
- Magento Developer Paradise 2011
- Magento Imagine 2012
- Magento Live 2012
- Dutch PHP Conference
- PHP North West

### Magento Experience ###

- Magento Certification Board Member
- Lead Magento projects for Warner Music, Kookai, Dreams, MissGuided and others
- +5 Years commercial experience with Magento Community and Magento Enterprise
- Zend Certified Engineer
- +14 Years commercial experience developing in PHP and other technologies

## Outline

* Intro
* Focused seasonal trends such as Cyber Monday have the capacity to melt tin
* Server capacity can of course be expanded with inexpensive hardware but when the DB is the bottleneck this is much harder
* I'm not a DBA
  * But when you're projects are popular you have to learn
  * You have to learn how to ask questions
  * You have to find people that can help
  * Engineering is not a solitary affair
* All web servers are scaled and running nicely
* You have Magento configured for optimal running
* Scaling, high availability and data redundancy
* What to look at
* You can't do anything with out good instrumentation
  * Development instrumentation to help identify
* We have metrics that say the DB needs to scale
  * The PHP process is blocking waiting on the DB
* Indexes
  * Identify missing indexes that would speed up the query and release the PHP thread quicker
* Transaction levels
* Transaction sizes
* Reducing write
  * Off-load functionality to third parties
  * Cache FPC and Varnish or even CDN caching of pages
  * Only modify state on HTTP POST
  * Keep data on the client and push logic there
* Introduce queues for eventual write and data consistency
* Clustering & replication
* Off load read from write
* Write read consistency with module level connections
* Cluster and replication options
* Tungsten
* Live hot upgrades
* Management of master
* Service discovery
* Future scaling options and replication architecture
