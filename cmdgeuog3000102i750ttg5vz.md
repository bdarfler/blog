---
title: "Dropping ACID on your NoSQL"
datePublished: Mon Oct 21 2013 19:37:28 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeuog3000102i750ttg5vz
slug: dropping-acid-on-your-nosql-66eaa3ad6067
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753303176749/2fa83d83-9fad-4533-b83e-cb7e75cf51c3.webp

---

A lot has been said about [NoSQL](https://en.wikipedia.org/wiki/NoSQL) and the resulting backlash of [NewSQL](https://en.wikipedia.org/wiki/NewSQL). NoSQL reared its head in reaction to the severe pain of sharding traditional open source SQL databases and quickly took the software community by storm. However, scalability wasn’t the only selling point. Many made the argument that SQL itself was to blame and that developers should rise up and throw off the shackles of this repressive language. Fast forward a few years and, while NoSQL alternatives are here to stay, it is quite clear that SQL is far from being overthrown. Instead, we now have a new breed of SQL options, the so called NewSQL databases, that offer the familiarity of SQL, with all its tooling and frameworks, while still giving us the scalability we need in this web scale world. It is in the midst of all this commotion that a quiet trend is emerging, one that is going mostly unnoticed, a trend of dropping ACID on your NoSQL.

#### NewACID

So what is this trend? Well it is a middle ground between the NoSQL world and the NewSQL world where ACID guarantees are provided across multiple keys but without the overhead of SQL. Until recently if you wanted multi-key transactions you had to use a NewSQL database; the ideas of SQL and ACID were intertwined and no one seemed to be trying to untangle them. However, in the past year and a half or so that has changed.

#### HyperDex

The first one of these databases to come to my attention was [HyperDex](http://hyperdex.org/). I was originally drawn to the project when I learned of its approach to secondary indexes which they call “hyperspace hashing”. In a traditional key-value store the values are placed in a 1-dimensional ring based on the primary key. In HyperDex the values are placed in n-dimentional space based on the primary and any secondary keys that are specified. This allows secondary index queries to be routed to a subset of the servers in the cluster instead of every server as is the case with secondary indexes in traditional key-value stores. I highly recommend reading the research [paper](http://hyperdex.org/papers/hyperdex.pdf) to get a better understanding. However, it was only recently with the addition of [Hyperdex Warp](http://hyperdex.org/warp/) that ACID support was added. I won’t go into the details of how Warp works but instead I’ll direct you to the research [paper](http://hyperdex.org/papers/warp.pdf) describing it.

#### FoundationDB

The next database on the scene was [FoundationDB](https://foundationdb.com/). While HyperDex, being developed at [Cornell University](http://www.cornell.edu/), has a strong research influence, FoundationDB instead comes from a practical engineering background. Not only has the FoundationDB team built a compelling, ACID compliant, NoSQL store but they ended up building [Flow](https://foundationdb.com/white-papers/flow), a C++ actor library, and a [trifecta of testing tools](https://foundationdb.com/white-papers/testing) to ensure that FoundationDB is as fast and as resilient as possible. More recently they acquired [Akiban](http://www.akiban.com/), makers of an interesting SQL database, and have started building a [SQL layer](https://foundationdb.com/layers/sql/index.html) on top of their key value store. This approach is [very similar](https://foundationdb.com/blog/7-things-that-make-google-f1-and-the-foundationdb-sql-layer-so-strikingly-similar) to the approach that Google took when creating [F1](http://research.google.com/pubs/pub41344.html) and not unlike the the approach that [NuoDB](http://www.nuodb.com/) is taking. It will be interesting to follow that NewSQL trend.

#### Spanner

Finally we come to [Spanner](http://research.google.com/archive/spanner.html), Google’s geo-distributed datastore. While significantly more vast in scale than either HyperDex or FoundationDB it still shares the common thread of ACID transactions in a NoSQL store. Of course the interesting part of Spanner that everyone is talking about is TrueTime which uses atomic clocks and GPS to create a consistent ordering of transactions across the globe. Its going to be a while before that becomes common place outside of Google data centers but its a fascinating solution to the normal assumptions about synchronized clocks in distributed systems.