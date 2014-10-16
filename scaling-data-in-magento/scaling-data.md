footer:
slidenumbers: false

# [fit] Scalable
## and
## [fit] Highly Available Data
## in
## [fit] Magento

![](images/session-scew.png)

---

# Alistair Stead
### CTO ![](images/session-logo.png)
### @alistairstead

![](images/Alistair.jpeg)

---

### Focused seasonal trends such as Cyber Monday have the capacity to melt tin

![](images/3265813515_5584814db8_o.jpg)

^The nature of marketing or customer trends is that they will drive traffic in a short time window. This results in a very focused peak in resource load in the hardware.

---

# Server capacity can of course be expanded
## *hardware is inexpensive...*

![](images/10921733615_56f0835096_o.jpg)

^It is often very common to simply add more and bigger hardware to solve the problem. But even this has limitations.

---

## Back pressure
### from lower systems will block PHP

![](images/8210286870_04537767e4_k.jpg)

^PHP may not be the bottleneck but as back pressure from slow service calls can manifest as slow PHP and apache or nginx processes and increased resource consumption.

---

# When the DB is the bottleneck
## *adding more servers will only make it worse*

^The symptoms will simply be exacerbated by adding more hardware, facilitating greater utilisation of the already saturated services.

---

# ~~I'm not a DBA~~

^I have plenty of experience but I would never consider myself a database expert. But I have learned how to devolve a problem down to it's smallest manifestation. Then I have a list of great people to whom I can go for help. That said I'm not afraid to role up my sleeves and work through the problem.

---

# When your store is popular you have to ~~cope~~ *learn*

---

# Don't just copy what is on **stack**overflow
## identify your exact problem.

---

### You have to learn to find the right questions first!

![](https://farm1.staticflickr.com/62/202872717_a8a4799419_o.jpg)

---

# You have to find people that can help!

---

# After all engineering is not a solitary affair

![](images/4730932250_be9ffb039b_o.jpg)

---

# So, you have your store up and running...

---

# All web servers are configured and running nicely...

---

# You have Magento configured for optimal running...

---

# Now you have more traffic and things are slowing down...

---

# What do you do?

![](images/3623768629_955cfaedca_o.jpg)

---

# Well... ..?

![](images/3623768629_955cfaedca_o.jpg)

---

# You can't do anything without *instrumentation*

![](images/6930598830_475c14676b_k.jpg)

---

# Development instrumentation
## *identify problems early*

---

# Production instrumentation
## *will show the real problems*

---

# We have metrics that say the DB is a slowing us down

![](images/88003222_c875cfda73_o.jpg)

^When you have metric that suggest the database is the biggest proportion of the request & response to the client then you have somewhere to investigate

---

# We need to take some actions...

---

# But first...
## a brief interlude

---

## Performance is a moving target
### But if you aim for it, you stand a greater chance of hitting it

---

## Performance, scaling, high availability and redundancy
### All related but separate things

---

## Performance:
### The ability to function quickly

---

## Scaling:
### The ability to function within acceptable limits as the number of users increases

---

## High availability:
### The ability to facilitate continuous function following and during failure

---

## Redundancy:
### Duplication of critical systems so as to have no single point of failure

---

# So where should we start?

---

## The Apache / Nginx process is blocking
### waiting on PHP...
### PHP is waiting on the Database...

---

# 8 of the many steps to performance

---

# Step 1
## Make your queries *FAST*er

![original](images/session-bg.png)

^The quicker your query returns to PHP the quicker all blocks can be released and the next client request can be answered.

---

# MySQL Indexes
## Identify missing indexes for a query and speed up the result

---

# Re-design queries
## Not recommended for core queries but sometimes you have to...
## *However send a patch back to Magento for inclusion in the next release*

---

# Step 2
## *Cache* as much as you can

![original](images/session-bg.png)

---

## Expand query cache as much as you can
### Can you fit your entire DB into memory?

---

## Use Full Page Cache
### Stating the obvious but it protects the database at peak loads

---

## Use proxy or edge caches
### If you don't need to execute PHP don't

---

## At some point your cache *MUST* expire
### On highly merchandised sites then cache is simply not as effective

---

## But this is all for *read* operations...
### What about *writing* data?

---

## Lock wait timeout...
### Have you seen this in your exception log?

---

# Step 3
## Ensure all tables are *INNODB*
## Some legacy code will have created MYISAM

![original](images/session-bg.png)

^This avoids table level locking as row level locks are more atomic preventing locking from being such a problem.

---

## ~~Increase `lock_wait_timeout`~~
### Don't this is an anti-pattern

^For the admin functions you may need higher values but for customers you want to exit on a lock earlier rather than wait for a lock that may not be released in time anyway.

---

# Step 4
## Transaction level

![original](images/session-bg.png)

---

## Use READ COMMITTED
### instead of the MySQL default of REPEATABLE READ

^Read committed is an isolation level that guarantees that any data read was committed at the moment is read. It simply restricts the reader from seeing any intermediate, uncommitted, 'dirty' read. IT makes no promise whatsoever that if the transaction re-issues the read, will find the Same data, data is free to change after it was read.

^Repeatable read is a higher isolation level, that in addition to the guarantees of the read committed level, it also guarantees that any data read cannot change, if the transaction reads the same data again, it will find the previously read data in place, unchanged, and available to read.

---

# Step 5
## *Reduce* transaction size

![original](images/session-bg.png)

---

## Your transaction is not committed?
### You're waiting for external service calls or none critical writes...

^You can think about splitting the critical transaction from these other operations. This means that the transaction locks are shorter and fewer collisions will happen.

---

# Step 6
## Reducing *non-critical* write operations

![original](images/session-bg.png)

---

## Logging can be done somewhere elese

```xml
<?xml version="1.0" encoding="UTF-8"?>
<frontend>
    <events>
        <controller_action_predispatch>
            <observers><log><type>disabled</type></log></observers>
        </controller_action_predispatch>
        <controller_action_postdispatch>
            <observers><log><type>disabled</type></log></observers>
        </controller_action_postdispatch>
        <customer_login>
            <observers><log><type>disabled</type></log></observers>
        </customer_login>
        <customer_logout>
            <observers><log><type>disabled</type></log></observers>
        </customer_logout>
        <sales_quote_save_after>
            <observers><log><type>disabled</type></log></observers>
        </sales_quote_save_after>
        <checkout_quote_destroy>
            <observers><log><type>disabled</type></log></observers>
        </checkout_quote_destroy>
    </events>
</frontend>
```

^Okay this is going to effect some functionality such as reports or online users. But it is an easy trade for greater transaction throughput. At this scale there are possibly better solutions to these features.

---

# HTTP 101
## *Only modify state on HTTP POST*
### #TIP 1. This simple rule can help so many aspects of scaling

^This is how HTTP is meant to work. Developers can BREAK this quite easily if you let them but if you respect it then your application can work well with other technology

---

## Off-load functionality to third parties
### logging and tracking can be handled else where

^If you can identify appropriate functionality to off-load to 3rd parties then you should. This is a philosophy that can be applied everywhere. If you don't have to build and maintain it don't

---

## Move data and logic to the client
### If state has not changed then the client should know all it needs to know

^We have a module that moves the cart functionality to the client and is only updated on POST.

---

# Step 7
## Asynchronous *write* operations

![original](images/session-bg.png)

^If you can delay the operation until after the response has been delivered to the client then you can scale it separately.

---

## Use job queues
### non-crtical write operations can be pushed to the queue
### You then have to work with eventual consistency

---

# Step 8
## Clustering & replication

![original](images/session-bg.png)

---

# Introduce a slave database
## Replicate data to the slave database

![](images/2414578959_fd897cfb1c_o.jpg)

^This is not about high avilability this is to achieve greater write throughput by off-loading the read  operations.

---

## Use standard MySQL replication
### Enable binary logging

---

## Ensure you have compression enabled!
### Or you will flood your internal network

---

## Use *`MIXED`* Binary logging format
### for quicker replication

---

## *`STATEMENT`* Binary logging
### Can cause PK clashes... in our experience...

^In our experience this can be a real problem

---

## Single threaded replication
### Prior to MySQL 5.6 you only have one thread

^Slave lag can be a real problem at high write volumes

---

## Split read from write operations
### Across the cluster

^You can dramatically increase throughput by sending read operations to many machines and writing to a single master DB

---

## Write, read consistency
### Can be resolved with module level connections

^Replication slave lag can be a problem especially with some payment modules. To resolve this you can use module level connection overrides.

---

## Module config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config>
    <global>
        <resources>
            <module_read>
                <connection>
                    <use>core_write</use>
                </connection>
            </module_read>
        </resources>
    </global>
</config>
```

^Module level configuration of the connection can override so that you can ensure that critical data flows function even when perhaps the functionality does not respect the transaction scope.

---

# 3 Goals of High Availability

---

# Goal 1
## Zero points of failure? *Plan for failure!*

![original](images/session-bg.png)

---

# Goal 2
## *Automated* recovery of services and data

![original](images/session-bg.png)

---

# Goal 3
## Network segmentation will have zero impact on performance

![original](images/session-bg.png)

---

# Cluster & replication options

---

# RedHat Cluster Interface

---

# MySQL Fabric

^MySQL Fabric provides a framework for managing farms of MySQL servers. MySQL Fabric provides two services that can be used individually or in combination. High Availability is built on top of MySQL Replication to provide automated failure detection and failover. On the failure of a master, a slave is automatically promoted and writes from the application routed to the correct servers.

---

# Tungsten ![inline 75%](images/continuent.png)

---

## Replicator
### A multi-threaded replication process over MySQL and other databases

---

## Smart Connection manager
### A smart connection manager that can route connections based on query content

---

## High availability
### Connection manager provides active service discovery
### Magento connects to a local port 3306

---

## Connection Manager
### runs on every server and allows the master to float around the cluster

^The connection manager is the reason we chose Tungsten

---

## Hot production upgrades
### MySQL can be configured or upgraded with *zero* downtime

---

## The Master Database
### Can be moved to any node without config changes or downtime

---

## Service discovery
### All servers connect to their own *Connection Manager*

---

# Next steps...

![original](images/session-bg.png)

---

## One setting does not rule them all
### Use your own metrics to define the best settings for your application

---

## One connection does not fit all tasks
### Use many tuned connections for specific operations types

^Use a different connection for admin function to client operations. Use a very specific connection for indexing operations.

---

## Alternate replication architecture
### Fan-in for example allowing multiple masters

---

## Sharding
### The smart connector can re-write the query on the fly
### You will need to introduce UUIDs for key tables

---

# Real Numbers

![original](images/session-bg.png)

---

# A project using these recommendations

---

## ~ €850,000 revenue per day

---

## ~ 23,000 orders per day

---

## ~ 2000 orders per hour at peak

---

## These are still small numbers

---

## They will be much higher this holiday season

---

# Gotchas...

![original](images/session-bg.png)

---

## Turn off security updates because your cluster will *FAIL*

^If all your database machines restart at the same time then your cluster will be in a state from which it can not recover. You should be placing your cluster in maintenance mode before running this level of updates.

---

## Ensure enough RAM for the transaction size

^Sites with high admin modifications will have large index transaction sizes don't be surprised when this exceeds the amount of RAM you allocated. Prior to EE 1.13.* and 1.14.* then the index event queue can result in huge transactions that can exceed 8GB.

---

## Do you have enough file descriptors
### This will be limited ensure you have enough

^This is a really low level system element and depending upon your hosting partner then this may be something they will help with but if you manage your own infrastructure, ensure that this is updated accordingly.

---

## Thank you!
# Questions?

![original](images/session-bg.png)

---

# http://bit.ly/sdinmage
