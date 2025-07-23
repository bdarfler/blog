---
title: "Benchmarking More Seq Traversal Idioms in Scala"
datePublished: Mon Apr 30 2012 13:24:34 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeu0md000202kz0f6n84gs
slug: benchmarking-more-seq-traversal-idioms-in-scala-fd8ef471495b

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302151419/d2a6e89e-9894-486d-adfb-2883555fc131.jpeg)

Last week I had the luxury of spending some quality time with [YourKit](http://yourkit.com/) and our production system at [Localytics](http://www.localytics.com) and was pleasantly surprised to see things humming right along. Most of our time was spent building collections and iterating over them which got me thinking. What is the most efficient way to traverse a collection in Scala? After a quick trip to google I had two blog posts in hand.

The [first post](http://blog.juma.me.uk/2009/10/26/new-jvm-options-and-scala-iteration-performance/) was from way back in 2009. While its main focus was on the impact of JVM options on Scala iteration it pointed out that Vectors seem to be more performant than Lists for iteration. However, being from 2009, I was curious if this result still stood. The [second post](http://paradigmatic.streum.org/2012/02/benchmarking-scala-list-traversal-idioms/) was from 2012 and benchmarked various ways of iterating over a List while applying two different transformations. The post didn’t benchmark Vector but the author was particularly rigorous with his methodology; making use of [Caliper](http://code.google.com/p/caliper/) and posting his code on [github](https://github.com/paradigmatic/seqBench). I highly recommend reviewing the post.

I [forked](https://github.com/bdarfler/seqBench) the repo, added two more tests, ensured the server vm was being used and re-ran the tests.

#### Setup

#### Java

java version "1.6.0\_24"  
OpenJDK Runtime Environment (IcedTea6 1.11.1) (6b24-1.11.1-4ubuntu2)  
OpenJDK Server VM (build 20.0-b12, mixed mode)

#### Scala

Scala code runner version 2.9.1 -- Copyright 2002-2011, LAMP/EPFL

#### OS

Ubuntu 12.04

#### CPU

Dual Intel(R) Xeon(R) CPU E5410 @ 2.33GHz (EC2 c1.medium)

#### Code

#### Functional Vector

%[https://gist.github.com/b2c00da6c91aea5ed88de37ce3aa197f]

#### Builder

%[https://gist.github.com/91a4fdbe483dab4e7a420016eb4a8dc8]

#### Results

Each style of iteration was tested with four different sized collections. The graph shows how many times slower each style was versus the OldSchool style (i.e. arrays and while loops).

![Results](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302153258/3084eab3-919e-424d-8e2f-cf3a0ca595ac.png)

#### Conclusions

*   Nothing beats arrays and while loops (i.e. the OldSchool solution)
*   Vector beats out List
*   Vector interestingly gets better with more items
*   In contrast to the original post none of the List optimizations seem like a clear win to me