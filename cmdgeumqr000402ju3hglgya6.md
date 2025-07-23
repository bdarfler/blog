---
title: "My Scala Toolset — May 2013"
datePublished: Wed May 29 2013 20:31:34 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeumqr000402ju3hglgya6
slug: my-scala-toolset-may-2013-bbc0abf31106
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753303553793/d19d3eca-524f-45e1-aecb-854edcc1a3dc.png

---

A friend recently asked for about my Scala toolset. When my response quickly spiraled out of control I decided to turn it into a quick blog post so everyone could benefit from the reply. So without further ado here is the current set of tools, libraries, and frameworks that I use on a semi-regular basis.

### Tools/Frameworks

* [Akka](http://akka.io/): A very slick Actor framework.
    
* [Scalatra](http://www.scalatra.org/): A Scala port of sinatra
    
* [Scalatest](http://www.scalatest.org/): Nice testing framework, specs is the other main player
    
* [IntelliJ](http://www.jetbrains.com/idea/): Very nice IDE for Scala, I prefer it to Eclipse
    

### SBT

* [SBT](http://www.scala-sbt.org/): Its a bit of a pain to work with but its the default build tool for Scala. If you are just trying to do basic dependency management its pretty nice. Extending it is where the pain comes in.
    
* [sbt-idea](https://github.com/mpeltonen/sbt-idea) plugin: will generate intellij projects from sbt
    
* [xsbt-web-plugin](https://github.com/JamesEarlDouglas/xsbt-web-plugin): will create wars for your project
    
* [junit\_xml\_listener](https://github.com/ijuma/junit_xml_listener): outputs scalatest results as junit xml so Jenkins can process it
    
* NOTE: SBT runs everything in parallel as much as it can, this can screw with junit tests but you can turn it off by setting parallelExecution := false
    

### Java Libs

* [JodaTime](http://joda-time.sourceforge.net/): Replaces the standard Java date stuff which is quite weak. The next version of Java will actually have JodaTime built in
    
* [c3p0](http://www.mchange.com/projects/c3p0/): A solid JDBC connection pool. I’ve also heard some talk about BoneCP which the Play framework uses.
    
* [Apache Commons](http://commons.apache.org/): look here for things like, string utils, io utils, db utils, utils around codecs, really anything that seems like it should already exist.
    
* [SLF4J](http://www.slf4j.org/): The only logging framework worth using anymore
    
* [Whirlycache](https://code.google.com/p/whirlycache/): A simple in memory cache though I keep hearing Google MapMaker is very nice.
    
* [Jackson](http://jackson.codehaus.org/): The only JSON library you should use, stupid fast
    

### Scala Wrappers

* [scalaj-time:](https://github.com/jorgeortiz85/scala-time) wrapper for joda time, lets you do things like 4.minutes.ago
    
* [scalaj-collection](https://github.com/scalaj/scalaj-collection): wrapper for collections that allows you to easily convert between scala and java collections. However, this was essentially moved into the standard library in Scala 2.9 so you might just want to use that now
    
* [Jerkson](https://github.com/codahale/jerkson): scala wrapper for Jackson though this is abandoned now. Jackson has a Scala module now and Json4s is trying to unify all the Scala JSON wrappers and supports Jackson.
    
* [slf4s](https://github.com/weiglewilczek/slf4s): A Scala wrapper for slf4j
    
* [scalaj-http](https://github.com/scalaj/scalaj-http): A simple wrapper for the built in Java HTTP library, not super performant but works in a pinch
    
* [dispatch](http://dispatch.databinder.net/Dispatch.html): A Scala wrapper around Apache HTTP Async, better for high performance HTTP but a bit more complicated to use as you should use a completion handler to process the response