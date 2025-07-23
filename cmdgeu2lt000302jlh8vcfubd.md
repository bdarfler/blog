---
title: "ConnectionFactories and Caching with Spring and ActiveMQ"
datePublished: Wed Jul 14 2010 14:19:50 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeu2lt000302jlh8vcfubd
slug: connectionfactories-and-caching-with-spring-and-activemq-681fd912a678

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302155948/77978b64-38f7-4088-8579-2a6ca34cebd1.jpeg)

© Sean Ryan

In my previous [two](http://codedependents.com/2009/10/16/efficient-lightweight-jms-with-spring-and-activemq/) [posts](http://codedependents.com/2010/03/04/synchronous-request-response-with-activemq-and-spring/) on ActiveMQ and Spring I’ve shown how to implement both asynchronous and synchronous messaging using MessageListenerContainers and JmsTemplates. Since then I have continued to tweak and improve my use of Spring’s JMS support and have uncovered a few more details about the ConnectionFactories supplied by both ActiveMQ and Spring and how they play together nicely and not so nicely. Some of this info is new while some was covered in my second post and lead to an edit in my initial post but I will reiterate that information here to keep it all in one place.

#### [PooledConnectionFactory](http://activemq.apache.org/maven/activemq-pool/apidocs/org/apache/activemq/pool/PooledConnectionFactory.html)

This bad boy is the ActiveMQ provide connection pooler. It pools connections, sessions and producers and does a fine job of it. There is nothing particularly wrong with this connection factory though it does strike me as odd that it caches connections since, according to the JMS spec, connections support concurrent use.

#### [SingleConnectionFactory](http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/jms/connection/SingleConnectionFactory.html)

It might seem odd to have a connection factory that ensures only one connection but in reality this is quite useful. When using multiple JmsTemplates and MessageListenerContainers in Spring they will end up each creating their own connection since the default ConnectionFactory will create a new connection for each createConnection() call. Initially I thought this would be fine but I ran into some very weird issues where producers and consumers couldn’t see each other. I’m not sure if this had something to do with using embedded brokers and the VM Transport but it wasn’t good. Using a SingleConnectionFactory as the initial decorator around the default ConnectionFactory solved these issues.

#### [CachingConnectionFactory](http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/jms/connection/CachingConnectionFactory.html)

This is the real heavy lifter. It will cache sessions and can optionally cache consumers and producers. As any post on JmsTemplate will tell you, you MUST use caching for any sort of reasonable performance if you are not running in a JCA container. As such, this is the factory to use when using a JmsTemplate. A few points should be made clear though. First, the default is for only one session to be cached. The javadoc claims this is sufficient for low concurrency but if you are doing something more high end you probably want to increase the SessionCacheSize. Additionally, there is some weirdness around cached consumers. They don’t get closed until they are removed from the pool and they are cached using a key that contains the destination and the selector. This lead to essentially a memory leak when trying to use cached consumers for request response semantics using JMSCorrelationIds. In general caching consumers is hard which is why MessageListenerContainers should be used instead.

#### How’s it all work

%[https://gist.github.com/09fe64a6cd87ee24260bdb53f8d98100]

First we start with a basic ActiveMQ ConnectionFactory. From there we wrap it in a SingleConnectionFactory and then wrap that in a CachingConnectionFactory. I’ve increased the SessionCacheSize on the CachingConnectionFactory to 100, this might be overkill but my application is anything but memory hungry so I figured I could err on the side of too many sessions. Additionally, and this is important, ReconnectOnException is set to true on the SingleConnectionFactory. The CachingConnectionFactory requires this and will complain loudly but in such as way as to make it unclear what is going on if this is not set. From there we use the CachingConnectionFactory to create a JmsTemplate and we use the SingleConnectionFactory to create a MessageListenerContainer since it takes care of any caching of sessions and consumers that it needs.