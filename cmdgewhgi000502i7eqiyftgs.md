---
title: "Efficient Lightweight JMS with Spring and ActiveMQ"
datePublished: Fri Oct 16 2009 16:14:49 GMT+0000 (Coordinated Universal Time)
cuid: cmdgewhgi000502i7eqiyftgs
slug: efficient-lightweight-jms-with-spring-and-activemq-51ff6a135946

---

Asynchronicity, its the [number one](http://highscalability.com/blog/2009/10/7/how-to-avoid-the-top-5-scale-out-pitfalls.html) [design](http://highscalability.com/blog/2009/8/11/13-scalability-best-practices.html) [principal](http://highscalability.com/blog/2009/5/8/eight-best-practices-for-building-scalable-systems.html) for highly scalable systems, and for Java that means [JMS](http://java.sun.com/products/jms/), which in turn means [ActiveMQ](http://activemq.apache.org/). But [how do I use JMS efficiently](http://activemq.apache.org/how-do-i-use-jms-efficiently.html)? One can quickly become overwhelmed with talk of containers, frameworks, and a plethora of options, most of which are outdated. So lets pick it apart.

#### Frameworks

The ActiveMQ documentation makes mention of two frameworks; [Camel](http://camel.apache.org/) and [Spring](http://www.springsource.org/about). The decision here comes down to simplicity vs functionality. Camel supports an immense amount of [Enterprise Integration Patterns](http://en.wikipedia.org/wiki/Enterprise_Integration_Patterns) that can greatly simplify integrating a variety of services and orchestrating complicated message flows between components. Its certainly a best of breed if your system requires such functionality. However, if you are looking for simplicity and support for the basic best practices then Spring has the upper hand. For me, simplicity wins out any day of the week.

#### JCA (Use It Or Loose It)

Reading through ActiveMQ’s [spring support](http://activemq.apache.org/spring-support.html) one is instantly introduced to the idea of a [JCA](http://java.sun.com/j2ee/connector/) container and ActiveMQ’s various proxies and [adaptors](http://activemq.apache.org/resource-adapter.html) for working inside of one. However, this is all a red herring. JCA is part of the [EJB](http://java.sun.com/products/ejb/) specification and as with most of the EJB specification, Spring doesn’t support it. Then there is a mention of [Jencks](http://docs.codehaus.org/display/JCA/Home), a “lightweight JCA container for Spring”, which was spun off of ActiveMQ’s [JCA container](http://activemq.apache.org/jca-container.html). At first this seems like the ideal solution, but let me stop you there. Jencks was last updated on January 3rd 2007. At that time ActiveMQ was at version 4.1.x and Spring was at version 2.0.x and things have come a long way, a very long way. Even trying to get Jencks from the maven repository fails due to dependencies on ActiveMQ 4.1.x jars that no longer exist. The simple fact is there are better and simpler ways to ensure resource caching.

#### Sending Messages

The core of Spring’s message sending architecture is the [JmsTemplate](http://static.springsource.org/spring/docs/2.5.6/api/org/springframework/jms/core/JmsTemplate.html). In typical Spring template fashion, the JmsTemplate abstracts away all the cruft of opening and closing sessions and producers so all the application developer needs to worry about is the actual business logic. However, ActiveMQ is quick to point out the [JmsTemplate gotchas](http://activemq.apache.org/jmstemplate-gotchas.html), mostly that JmsTemplate is designed to open and close the session and producer on each call. To prevent this from absolutely destroying the messaging performance the documentation recommends using ActiveMQ’s [PooledConnectionFactory](http://activemq.apache.org/maven/activemq-pool/apidocs/org/apache/activemq/pool/PooledConnectionFactory.html) which caches the sessions and message producers. However this too is outdated. Starting with version 2.5.3, Spring started shipping its own [CachingConnectionFactory](http://static.springsource.org/spring/docs/2.5.6/api/org/springframework/jms/connection/CachingConnectionFactory.html) which I believe to be the preferred caching method. (UPDATE: In [my more recent post](http://codedependents.com/2010/03/04/synchronous-request-response-with-activemq-and-spring/), I talk about when you might want to use PooledConnectionFactory.) However, there is one catch to point out. By default the CachingConnectionFactory only caches one session which the javadoc claims to be [sufficient for low concurrency situations](http://static.springsource.org/spring/docs/2.5.6/api/org/springframework/jms/connection/CachingConnectionFactory.html#setSessionCacheSize%28int%29). By contrast, the PooledConnectionFactory [defaults to 500](http://www.docjar.com/html/api/org/apache/activemq/pool/PooledConnectionFactory.java.html). As with most settings of this type, some amount of experimentation is probably in order. I’ve started with 100 which seems like a good compromise.

#### Receiving Messages

As you may have noticed, the [JmsTemplate gotchas](http://activemq.apache.org/jmstemplate-gotchas.html) strongly discourages using the recieve() call on the JmsTemplate, again, since there is no pooling of sessions and consumers. Moreover, all calls on the JmsTemplate are synchronous which means the calling thread will block until the method returns. This is fine when using JmsTemplate to send messages since the method returns almost instantly. However, when using the recieve() call, the thread will block until a message is received, which has a huge impact on performance. Unfortunately, neither the [JmsTemplate gotchas](http://activemq.apache.org/jmstemplate-gotchas.html) nor the [spring support](http://activemq.apache.org/spring-support.html) documentation mentions the simple Spring solution for these problems. In fact they both recommend using [Jencks](http://docs.codehaus.org/display/JCA/Home), which we already debunked. The actual solution, using the [DefaultMessageListenerContainer](http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/jms/listener/DefaultMessageListenerContainer.html), is buried in the [how do I use JMS efficiently](http://activemq.apache.org/how-do-i-use-jms-efficiently.html) documentation. The DefaultMessageListenerContainer allows for the asynchronous receipt of messages as well as caching sessions and message consumers. Even more interesting, the DefaultMessageListenerContainer can dynamically grow and shrink the number of listeners based on message volume. In short, this is why we can completely ignore JCA.

#### Putting It All Together

#### Spring Context XML

%[https://gist.github.com/3e3d5970f337bd8bafc75f3da42a76f8]

There are two things to notice here. First, I’ve added the amq and jms namespaces to the opening beans tag. Second, I’m using the Spring 2.5 [annotation based configuration](http://static.springsource.org/spring/docs/2.5.x/reference/beans.html#beans-annotation-config). By using the annotation based configuration I can simply add [@Component](http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/stereotype/Component.html) annotation to my Java classes instead of having to specify them in the spring context xml explicitly. Additionally, I can add [@Autowired](http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/beans/factory/annotation/Autowired.html) on my constructors to have objects such as JmsTemplate automatically wired into my objects.

#### QueueSender

%[https://gist.github.com/1502e50db47bccc7a05c566c2603e893]

#### Queue Listener

%[https://gist.github.com/d0adb31efda90f2482ba21e053ae8270]

#### JmsExceptionListener

%[https://gist.github.com/d3c8f0740315e8e2bd1fd08bf6618f92]

#### Update

I have finally updated the wiki at activemq.apache.org. The following pages now recommend using MessageListenerContainers and JmsTemplate with a Pooling ConnectionFactory instead of JCA and Jencks.

*   [http://activemq.apache.org/spring-support.html](http://activemq.apache.org/spring-support.html)
*   [http://activemq.apache.org/how-do-i-use-jms-efficiently.html](http://activemq.apache.org/how-do-i-use-jms-efficiently.html)
*   [http://activemq.apache.org/jmstemplate-gotchas.html](http://activemq.apache.org/jmstemplate-gotchas.html)