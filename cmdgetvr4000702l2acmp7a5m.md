---
title: "Web Scale Analytics Reading List"
datePublished: Thu Aug 30 2012 11:49:36 GMT+0000 (Coordinated Universal Time)
cuid: cmdgetvr4000702l2acmp7a5m
slug: web-scale-analytics-reading-list-b26794518341
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753303658928/d790264e-ae17-4b52-8f83-7dc0cfea2cb1.jpeg

---

Big data is taking the world by storm and with it comes an explosion of new ideas and technologies looking to help us understand what this data is telling us. With [VLDB 2012](http://www.vldb2012.org/) under way I decided to take another look at the literature to see what advances are out there as well as refresh on the classics. The result of this deep dive is the web scale analytics reading list below. The list is grouped at a high level into [column oriented database](http://en.wikipedia.org/wiki/Column-oriented_DBMS) solutions and [online analytical processing (OLAP)](http://en.wikipedia.org/wiki/Online_analytical_processing) solutions. Column oriented databases are by far more powerful but also more complicated to implement. As such, much of the work on column oriented databases is being done at companies that are building one as a product. The obvious exception being Google. OLAP systems on the other hand, while less powerful, are simpler to implement. For this reason we see a variety of companies rolling their own solutions in response to their growing analytics problems.

#### Column Oriented Databases

*Brighthouse*

* An architectural overview of the Brighthouse (now Infobright) database
    
* [http://stencel.mimuw.edu.pl/sem/sbd2/Infobright/Slezak%20et%20al%202008.pdf](http://stencel.mimuw.edu.pl/sem/sbd2/Infobright/Slezak%20et%20al%202008.pdf)
    

*C-Store*

* The academic system that was commercialized as Vertica
    
* [http://db.csail.mit.edu/projects/cstore/vldb.pdf](http://db.csail.mit.edu/projects/cstore/vldb.pdf)
    

*Vertica*

* A follow up to the C-Store paper 7 years later
    
* [http://vldb.org/pvldb/vol5/p1790\_andrewlamb\_vldb2012.pdf](http://vldb.org/pvldb/vol5/p1790_andrewlamb_vldb2012.pdf)
    

*Dremel*

* Google’s column oriented query system optimized for large data on disk
    
* [http://www.vldb.org/pvldb/vldb2010/papers/R29.pdf](http://static.googleusercontent.com/external_content/untrusted_dlcp/research.google.com/en/us/pubs/archive/36632.pdf)
    

*PowerDrill*

* Google’s in memory column oriented database based on Infobright and Vertica
    
* [http://vldb.org/pvldb/vol5/p1436\_alexanderhall\_vldb2012.pdf](http://vldb.org/pvldb/vol5/p1436_alexanderhall_vldb2012.pdf)
    

*Drill*

* An Apache project to create an open source version of Dremel
    
* [http://wiki.apache.org/incubator/DrillProposal](http://wiki.apache.org/incubator/DrillProposal)
    

*Impala*

* Cloudera’s open source answer to Dremel
    
* [http://blog.cloudera.com/blog/2012/10/cloudera-impala-real-time-queries-in-apache-hadoop-for-real/](http://blog.cloudera.com/blog/2012/10/cloudera-impala-real-time-queries-in-apache-hadoop-for-real/)
    

*Supersonic*

* An open source query library from Google; storage backend forthcoming
    
* [http://www.infoq.com/news/2012/10/Google-Supersonic](http://www.infoq.com/news/2012/10/Google-Supersonic)
    

*ParStream*

* A commercial column oriented database making extensive use of compressed bitmap indexes
    
* [http://www.google.com/patents/EP2531939A1](http://www.google.com/patents/EP2531939A1)
    

#### Online Analytical Processing

*High-Dimensional OLAP: A Minimal Cubing Approach*

* A novel approach to handling high dimensional data that would normally explode an OLAP cube
    
* [http://www.vldb.org/conf/2004/RS14P1.PDF](http://www.vldb.org/conf/2004/RS14P1.PDF)
    

*FastIp (now Boundary)*

* A real time OLAP solution supporting arbitrary queries
    
* Based on the High-Dimensional OLAP paper
    
* [http://strataconf.com/strata2011/public/schedule/detail/17058](http://strataconf.com/strata2011/public/schedule/detail/17058)
    

*Boundary (formerly FastIp)*

* A real time OLAP solution supporting a more limited set of queries
    
* No longer based on the High-Dimensional OLAP paper
    
* [http://boundary.com/blog/2012/08/21/boundary-techtalk-large-scale-olap-with-kobayashi/](http://boundary.com/blog/2012/08/21/boundary-techtalk-large-scale-olap-with-kobayashi/)
    

*BackType (acquired by Twitter)*

* A hybrid OLAP system pairing a batch system with a real time system for recent data
    
* [http://www.readwriteweb.com/hack/2011/01/secrets-of-backtypes-data-engineers.php](http://www.readwriteweb.com/hack/2011/01/secrets-of-backtypes-data-engineers.php)
    

*LinkedIn*

* A batch only OLAP solution supporting a very constrained set of use cases
    
* [http://static.usenix.org/events/fast12/tech/full\_papers/Sumbaly.pdf](http://static.usenix.org/events/fast12/tech/full_papers/Sumbaly.pdf)
    
* [http://vldb.org/pvldb/vol5/p1874\_liliwu\_vldb2012.pdf](http://vldb.org/pvldb/vol5/p1874_liliwu_vldb2012.pdf)
    

*Druid*

* An in memory OLAP solution from Metamarkets
    
* [http://metamarkets.com/technology/druid/](http://metamarkets.com/technology/druid/)
    
* [http://www.dbms2.com/category/products-and-vendors/metamarkets-druid/](http://www.dbms2.com/category/products-and-vendors/metamarkets-druid/)
    
* [http://metamarkets.com/2012/metamarkets-open-sources-druid/](http://metamarkets.com/2012/metamarkets-open-sources-druid/)