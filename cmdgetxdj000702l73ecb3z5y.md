---
title: "A Sequential I/O Reading List"
datePublished: Sun Jul 08 2012 18:40:32 GMT+0000 (Coordinated Universal Time)
cuid: cmdgetxdj000702l73ecb3z5y
slug: a-sequential-i-o-reading-list-3211fae28f2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753303743799/d27aa84a-a3cf-46fb-b88c-8b33231416be.jpeg

---

Over the past year I’ve been collecting links on the growing trend of rooting out all random I/O in large scale distributed systems. It has always been apparent that rotational media suffered a random I/O penalty but as bandwidth improvements continue and latency improvements languish the difference between sequential and random I/O is becoming unbearable. SSDs have been hailed as a fix, and in large part they remove the pain of random reads, but at the cost of requiring sequential writes. And now, as it now turns out, even RAM benefits from sequential I/O due to the increasingly complex structure of new and faster chips. In an effort to help others who might be stumbling into this area here is a curated list of the best posts and papers I have found on the topic.

#### Rotational Media

*Log Structured Merge Trees*

* The grandaddy of all sequential I/O data structures
    
* [http://citeseer.ist.psu.edu/viewdoc/summary?doi=10.1.1.44.2782](http://arstechnica.com/information-technology/2012/06/inside-the-ssd-revolution-how-solid-state-disks-really-work/)
    

*Cache Oblivious Streaming B-trees*

* Explains COLAs which works much like SSTables in Cassandra/HBase/BigTable
    
* Explains Shuttle Trees where are commercialized by Tokutek and rebranded Fractal Trees
    
* [http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.114.3367](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.114.3367)
    

*Overflow to disk*

* A look at various disk based data structures from one of the developers of VoltDB
    
* [http://www.afewmoreamps.com/2012/03/overflow-to-disk.html](http://www.afewmoreamps.com/2012/03/overflow-to-disk.html)
    

*Write Optimization: Myths, Comparison, Clarifications*

* A look at various disk based data structures from the Tokukek team
    
* [http://www.tokutek.com/2011/09/write-optimization-myths-comparison-clarifications/](http://www.tokutek.com/2011/09/write-optimization-myths-comparison-clarifications/)
    
* [http://www.tokutek.com/2011/10/write-optimization-myths-comparison-clarifications-part-2/](http://www.tokutek.com/2011/10/write-optimization-myths-comparison-clarifications-part-2/)
    

#### Solid State Drives

*Block Erasure*

* One reason why sequential writes are critical
    
* [http://en.wikipedia.org/wiki/Flash\_memory#Limitations](http://en.wikipedia.org/wiki/Flash_memory#Limitations)
    

*Write Amplification*

* Another reason why sequential writes are critical
    
* [http://en.wikipedia.org/wiki/Write\_amplification#Sequential\_writes](http://en.wikipedia.org/wiki/Write_amplification#Sequential_writes)
    

*Solid-state revolution: in-depth on how SSDs really work*

* Honestly more than you ever wanted to know about how SSDs work
    
* [http://arstechnica.com/information-technology/2012/06/inside-the-ssd-revolution-how-solid-state-disks-really-work/](http://arstechnica.com/information-technology/2012/06/inside-the-ssd-revolution-how-solid-state-disks-really-work/)
    

#### Memory

*What every programmer should know about the memory system*

* RAM is not really random anymore
    
* [http://www.futurechips.org/chip-design-for-all/what-every-programmer-should-know-about-the-memory-system.html](http://www.futurechips.org/chip-design-for-all/what-every-programmer-should-know-about-the-memory-system.html)