---
title: "Integrate ant with dbdeploy not in the classpath"
datePublished: Mon Jan 26 2009 22:47:37 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeuajo000302kz2o3q9gb5
slug: integrate-ant-with-dbdeploy-not-in-the-classpath-c857390b6e66

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302166406/7cc71d94-a4a7-4ee2-baba-e424df52b885.jpeg)

The need for a schema versioning system at [LocaModa](http://www.locamoda.com) finally became a priority this sprint so I’ve spent the past few days researching and diving into implementation. After comparing [LiquiBase](http://www.liquibase.org/), [dbdeploy](http://dbdeploy.com/), [MIGRATEdb](http://migratedb.sourceforge.net/), and [dbmigrate](http://code.google.com/p/dbmigrate/) I settled on the simple SQL driven solution that dbdeploy provides and started integrating it with our [Ant](http://ant.apache.org/) build. [Pramod Sadalage](http://www.linkedin.com/in/pramodsadalage) over at [Agile DBA](http://www.sadalage.com/) put up a [nice post](http://www.sadalage.com/2008/01/exprerience_using_dbdeploy_on_1.html) on the basics but I quickly hit a wall, the [MySQL](http://www.mysql.com/) driver was not in my classpath.

As I [mentioned earlier](http://blog.bdarfler.com/2008/11/11/pimpin-your-ant-build-with-ivy/) we are using [Ivy](http://ant.apache.org/ivy/) for our dependancy management which makes it difficult to throw jars into the Ant classpath since they are automatically downloaded at build time. So far this hasn’t been an issue since most Ant tasks allow you to specify the classpath as a parameter within the task. Dbdeploy does not. Goggling around I quickly found [a posting](http://groups.google.com/group/db-deploy-users/browse_thread/thread/a381c9cc3c8e0d9f/050ace04ddaf7519) that suggested patching the dbdeploy ant task as the only way to go, so off I went. However, a few hours later, after wandering through the Java classloader abyss I was stymied.

I backed out of the changes and decided a different tactic was needed; back to Goggle I went. This time I came across [a post](http://marc.info/?t=112482713500002&r=1&w=2) which suggested all I needed was to add the jar to the taskdef classpath Could it really be that easy? Switching back to my build.xml I doctored up the dbdeploy taskdef, kicked off a build and sure enough the damn thing worked.

So, if you want to centrally manage your ant dependencies, just remember to add the necessary classpath references to your taskdefs, like so:

%[https://gist.github.com/ed1ef53d1e834baa2a410e7064923edd]