---
title: "11 Best Practices for Low Latency Systems"
datePublished: Mon Jan 27 2014 12:31:06 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeto37000602kw21yscfvp
slug: 11-best-practices-for-low-latency-systems-a00fc6e0dfda
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753302956375/9f1d0367-3a58-4969-8a20-4787abd1b623.jpeg

---

Its been 8 years since Google noticed that [an extra 500ms of latency dropped traffic by 20%](http://glinden.blogspot.com/2006/11/marissa-mayer-at-web-20.html) and Amazon realized that [100ms of extra latency dropped sales by 1%](http://highscalability.com/latency-everywhere-and-it-costs-you-sales-how-crush-it). Ever since then developers have been racing to the bottom of the latency curve, culminating in front-end developers squeezing every last millisecond out of their [JavaScript](http://shop.oreilly.com/product/9780596802806.do), [CSS](http://islovely.co/blog/writing-high-performance-css/), and even [HTML](http://www.slideshare.net/souders/high-performance-html5-sf-html5-ug). What follows is a random walk through a variety of best practices to keep in mind when designing low latency systems. Most of these suggestions are taken to the logical extreme but of course tradeoffs can be made. (Thanks to an anonymous user for asking [this question](https://www.quora.com/Scalability/How-do-you-design-a-web-backend-that-minimizes-latency) on [Quora](https://www.quora.com/) and getting me to put my thoughts down in writing).

#### Choose the right language

Scripting languages need not apply. Though they keep getting faster and faster, when you are looking to shave those last few milliseconds off your processing time you cannot have the overhead of an interpreted language. Additionally, you will want a strong memory model to enable lock free programming so you should be looking at Java, Scala, C++11 or Go.

#### Keep it all in memory

I/O will kill your latency, so make sure all of your data is in memory. This generally means managing your own in-memory data structures and maintaining a persistent log, so you can rebuild the state after a machine or process restart. Some options for a persistent log include [Bitcask](https://github.com/basho/bitcask), [Krati](https://github.com/jingwei/krati), [LevelDB](https://code.google.com/p/leveldb/) and [BDB-JE](http://www.oracle.com/technetwork/products/berkeleydb/overview/index-093405.html). Alternatively, you might be able to get away with running a local, persisted in-memory database like [redis](http://redis.io/) or [MongoDB](http://www.mongodb.org/) (with memory &gt;&gt; data). Note that you can loose some data on crash due to their background syncing to disk.

#### Keep data and processing colocated

[Network hops are faster than disk seeks](http://www.eecs.berkeley.edu/~rcs/research/interactive_latency.html) but even still they will add a lot of overhead. Ideally, your data should fit entirely in memory on one host. With AWS providing almost 1/4 TB of RAM in the cloud and physical servers offering multiple TBs this is generally possible. If you need to run on more than one host you should ensure that your data and requests are properly partitioned so that all the data necessary to service a given request is available locally.

#### Keep the system underutilized

Low latency requires always having resources to process the request. Don’t try to run at the limit of what your hardware/software can provide. Always have lots of head room for bursts and then some.

#### Keep context switches to a minimum

Context switches are a sign that you are doing more compute work than you have resources for. You will want to limit your number of threads to the number of cores on your system and to pin each thread to its own core.

#### Keep your reads sequential

All forms of storage, wither it be rotational, flash based, or memory [performs significantly better when used sequentially](http://codedependents.com/2012/07/08/a-sequential-io-reading-list/). When issuing sequential reads to memory you trigger the use of prefetching at the RAM level as well as at the CPU cache level. If done properly, the next piece of data you need will always be in L1 cache right before you need it. The easiest way to help this process along is to make heavy use of arrays of primitive data types or structs. Following pointers, either through use of linked lists or through arrays of objects, should be avoided at all costs.

#### Batch your writes

This sounds counterintuitive but you can gain significant improvements in performance by batching writes. However, there is a misconception that this means the system should wait an arbitrary amount of time before doing a write. Instead, one thread should spin in a tight loop doing I/O. Each write will batch all the data that arrived since the last write was issued. This makes for a very fast and adaptive system.

#### Respect your cache

With all of these optimizations in place, memory access quickly becomes a bottleneck. Pinning threads to their own cores helps reduce CPU cache pollution and sequential I/O also helps preload the cache. Beyond that, you should keep memory sizes down using primitive data types so more data fits in cache. Additionally, you can look into [cache-oblivious algorithms](http://en.wikipedia.org/wiki/Cache-oblivious_algorithm) which work by recursively breaking down the data until it fits in cache and then doing any necessary processing.

#### Non blocking as much as possible

Make friends with non blocking and wait free data structures and algorithms. Every time you use a lock you have to go down the stack to the OS to mediate the lock which is a huge overhead. Often, if you know what you are doing, you can get around locks by understanding the memory model of the JVM, C++11 or Go.

#### Async as much as possible

Any processing and particularly any I/O that is not absolutely necessary for building the response should be done outside the critical path.

#### Parallelize as much as possible

Any processing and particularly any I/O that can happen in parallel should be done in parallel. For instance if your high availability strategy includes logging transactions to disk and sending transactions to a secondary server those actions can happen in parallel.

#### Resources

Almost all of this comes from following what LMAX is doing with their Disruptor project. Read up on that and follow anything that [Martin Thompson](https://twitter.com/mjpt777) does.

* [Disruptor Project](http://lmax-exchange.github.io/disruptor/)
    
* [The LMAX Architecture](http://martinfowler.com/articles/lmax.html)
    
* [Mechanical Sympathy](http://mechanical-sympathy.blogspot.com/)
    
* [Bad Concurrency](http://bad-concurrency.blogspot.com/)
    
* [Disruptor Blogs](https://github.com/LMAX-Exchange/disruptor/wiki/Blogs-And-Articles)
    
* [Disruptor Presentations](https://github.com/LMAX-Exchange/disruptor/wiki/Presentations-And-Interviews)
    

Additional Blogs

* [Java is the new C](http://java-is-the-new-c.blogspot.com/)
    
* [Psychosomatic, Lobotomy, Saw](http://psy-lob-saw.blogspot.com/)
    
* [Preshing on Programming](http://preshing.com/)
    
* [Vanilla #Java](http://vanillajava.blogspot.com/)
    
* [1024cores](http://www.1024cores.net/)