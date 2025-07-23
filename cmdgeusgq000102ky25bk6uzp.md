---
title: "11 Lesser Known 3rd Party Libraries For Every Project"
datePublished: Mon Oct 26 2009 15:17:18 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeusgq000102ky25bk6uzp
slug: 11-lesser-known-3rd-party-libraries-for-every-project-db85ac30ad73

---

![pennies_in_jar_small](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302189334/a675910a-9bbb-41da-a526-4a1ff48abe9b.jpeg)

The Java 3rd party library ecosystem is a wild wild place. While everyone has heard of the big players such as Spring and Hibernate, too often the more humble, but equally important, libraries get left out in the cold. It is for that reason that I give you the 11 lesser known 3rd party libraries that no project should be without.

#### Unit Testing

As always, the easiest place to start playing around with new libraries and languages is in unit tests. I’m convinced they would be worth writing if only as an outlet for developers to try new and interesting tools, let along the code quality improvements.

#### DBUnit

While we can quibble over including databases in unit tests or if they are integration tests the fact of the matter is that at some point most people write a unit test that requires a database with some preexisting data. Thats when [DBUnit](http://www.dbunit.org/) shines. It has the ability to use xml from a file or a string or any other type of input to clean and populate database tables with ease. It even has some nice utilities for asserting data after a test has run.

#### Mockito

On the other end of the spectrum from full blown database integration is mocking. When you need to disentangle an object from its myriad of dependancies and test it in isolation mock objects come to the rescue. There are a number of solutions out there but [Mockito](http://mockito.org/) takes the cake. To see why I prefer it to EasyMock check out their [comparison](http://code.google.com/p/mockito/wiki/MockitoVSEasyMock). They’ve even added some [niceties](http://mockito.googlecode.com/svn/branches/1.8.0/javadoc/org/mockito/BDDMockito.html) for [BDD](http://en.wikipedia.org/wiki/Behavior_Driven_Development) style testing.

#### Hamcrest Matchers

Since [Junit 4.4](http://junit.sourceforge.net/doc/ReleaseNotes4.4.html), a core set of [hamcrest matchers](http://code.google.com/p/hamcrest/) has been included with the distribution and has gone a long way to simplifying assertions by through a pseduo english DSL. For even more power you can include the whole hamcrest matcher jar and start playing with features such as asserting the contents of collections.

#### Apache Commons

#### Configuration

Most projects start with one properties file and before you know it there are two, then three, then ten and it gets out of control. You have static singletons that pull files off the class path and you’ve gone off the deep end. Thats why I say start [Commons Configurations](http://commons.apache.org/configuration/), even for that first file. Not only does it greatly simplify dealing with properties from the api point of view but it allows for pulling properties from xml, jdbc, property files, and much more. Its can even work as a simple api for dealing with xml in a jiffy.

#### DbUtils

These days with Spring JDBC, iBatis, Hibernate, ActiveObjects, and a host of other frameworks and libraries for dealing with JDBC its pretty easy to never have to directly touch the stuff. However, if you find yourself in the hell which is closing a connection, session, and statement (with all the null checks and finals and exceptions and oh dear god) then [Commons DbUtils](http://commons.apache.org/dbutils/) is for you. A simple close() method is provided which handles all the null checking and try catching and so on. So basic and yet so helpful.

#### IO

This one might be a bit more obscure but if you have ever needed to simply read the contents of a file you have no doubt been stymied by which reader gets wrapped by which buffered writer and who flushes what. [Commons IO](http://commons.apache.org/io/) provides a beautifully simple api for file reading as well as some nice utility methods, similar to DbUtils, for closing all those annoying objects with out all the hassle.

#### Lang

If there was one library on my list it would come down to this or Google Collections. [Commons Lang](http://commons.apache.org/lang/) is the place for every utility method you have ever thought of writing. Every time you have though of creating a null safe version of any of the String methods, StringUtils has your back. If you have though of doing the same for Booleans then BooleanUtils is for you. Same for Object and ObjectUtils. But it doesnt stop there, StringUtils has more methods than you can shake a stick at and then there is the Builder package. The Builder package contains a Hashcode, ToString, Equals and CompareTo builder. Better yet they contain methods to build these values dynamically based on reflection. I’ve taken to making a base class from which all my domain objects inherit that simply overrides Hashcode, ToString and Equals with the reflection based builders. Never again do you need to create these annoying methods by hand or have Eclipse generate them only to forget to regenerate when you add another field.

#### New Kids On The Block

#### SLF4J

Logging, everyone needs it. Thats probably why there are at least four major Java logging frameworks. However, they are not all created equal. [Java util logging](http://java.sun.com/javase/6/docs/api/java/util/logging/package-summary.html) is known to have a number of shortcomings when compared to [Log4J](http://logging.apache.org/log4j/1.2/index.html) and [Commons Logging](http://commons.apache.org/logging/) is just a facade that allows you to switch between the two more easily. Out of this mess comes [SLF4J](http://www.slf4j.org/). Created by the creator of Log4J it is simply a set of interfaces that anyone can implement. The default implementation, also made by the same guy, is called [logback](http://logback.qos.ch/) and takes all the best of Log4J and improves on it. Moreover there are converters that route Log4J, CommonsLogging, and Java util logging to SLF4J. Never again do you have to struggle with multiple libraries and configuration files for all of your 3rd party libraries in your project. Simply drop in the converters and everything nicely flows into SLF4J.

#### Google Collections

Like I said before, if there were one library on my list it would come down to Commons Lang and [Google Collections](http://code.google.com/p/google-collections/). If you have every used [Commons Collections](http://commons.apache.org/collections/) and cursed at its lack of type safety then Google Collections is for you. If you have ever balked at having to write List<Map<String, List<Integer>>> only to realized you have to type it all over again on the left hand side of the value assignment then Google Collections static methods such as newHashMap() are for you. If you are looking for a poor mans cache then MapMaker is for you. Finally, if you want to play around with functional programing ideas in Java look no farther than Predicate and Function and take it as a challenge to remove all for loops from your application.

#### c3p0

Database pooling, if you connectot to a database you need it. There are a few out there to choose from and the decision is always a bit of an argument but since Hibernate chose [c3p0](http://www.mchange.com/projects/c3p0/index.html) as its default connection pool it has gotten significantly more attention than the other options and the prevailing winds seem to suggest that it is the way to go.

#### Joda Time

Not everyone has to deal with time in their programs but if you do, look no farther than [Joda Time](http://joda-time.sourceforge.net/). From simple formatting to the complication of subtracting dates and everything in between, Joda Time is an easy to use api for all your date and time needs. No longer will you have deal with Calendars and wonder what exactly is a Gregorian one.