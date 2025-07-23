---
title: "Synchronous Request Response with ActiveMQ and Spring"
datePublished: Thu Mar 04 2010 17:32:58 GMT+0000 (Coordinated Universal Time)
cuid: cmdgetm1l000402jo32eaa9bh
slug: synchronous-request-response-with-activemq-and-spring-21359a438a86

---

![activemq-5-x-box-reflection](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302134288/f3f7f0c9-df99-4329-ba86-7e0f8faf890b.png)

A few months ago I did a deep dive into [Efficient Lightweight JMS with Spring and ActiveMQ](http://codedependents.com/2009/10/16/efficient-lightweight-jms-with-spring-and-activemq/) where I focused on the details for asynchronous sending and receiving of messages. In an ideal world all messaging would be asynchronous. If you need a response then you should set up an asynchronous listener and either have enough state stored in the service or in the message that you can continue processing once the response has been received. However, when ideals lead to complexity, we have to make the decision of how much complexity can we tolerate for the performance we need. Sometimes it just makes more sense to use a synchronous request/response.

#### Request Response with JMS

ActiveMQ documentation actually has a pretty good overview of [how request-response semantics work in JMS](http://activemq.apache.org/how-should-i-implement-request-response-with-jms.html).

> You might think at first that to implement request-response type operations in JMS that you should create a new consumer with a selector per request; or maybe create a new temporary queue per request.

> Creating temporary destinations, consumers, producers and connections are all synchronous request-response operations with the broker and so should be avoided for processing each request as it results in lots of chat with the JMS broker.

> The best way to implement request-response over JMS is to create a temporary queue and consumer per client on startup, set JMSReplyTo property on each message to the temporary queue and then use a correlationID on each message to correlate request messages to response messages. This avoids the overhead of creating and closing a consumer for each request (which is expensive). It also means you can share the same producer & consumer across many threads if you want (or pool them maybe).

This is a pretty good start but it requires some tweaking to work best in Spring. It also should be noted that [Lingo](http://lingo.codehaus.org/home) and [Camel](http://camel.apache.org/) are also suggested as options when using ActiveMQ. In my [previous post](http://activemq.apache.org/how-should-i-implement-request-response-with-jms.html) I addressed why I don’t use either of these options. In short Camel is more power than is needed for basic messaging and Lingo is built on [Jencks](http://jencks.codehaus.org/Home), neither of which have been updated in years.

#### Request Response in Spring

The first thing to notice is that its infeasible to create a consumer and temporary queue per client in Spring since pooling resources is required overcome the [JmsTemplate gotchas](http://activemq.apache.org/jmstemplate-gotchas.html). To get around this, I suggest using predefined request and response queues, removing the overhead of creating a temporary queue for each request/response. To allow for multiple consumers and producers on the same queue the JMSCorrelationId is used to correlated the request with its response message.

At this point I implemented the following naive solution:

%[https://gist.github.com/2485f3d5e8c169e8154d857596d8aa81]

This worked for a while until the system started occasionally timing out when making a request against a particularly fast responding service. After some debugging it became apparent that the service was responding so quickly that the receive() call was not fully initialized, causing it to miss the message. Once it finished initializing, it would wait until the timeout and fail. Unfortunately, there is very little in the way of documentation for this and [the best suggestion](http://forum.springsource.org/showpost.php?p=141588&postcount=7) I could find still seemed to leave open the possibility for the race condition by creating the consumer after sending the message. Luckily, according to the [JMS spec](http://technology-related.com/j2ee/1.4/docs/tutorial/doc/JMS4.html#wp79145), a consumer becomes active as soon as it is created and, assuming the connection has been started, it will start consuming messages. This allows for the reordering of the method calls leading to the slightly more verbose but also more correct solution. (**NOTE**: Thanks to Aaron Korver for pointing out that ProducerConsumer needs to implement SessionCallback and that true needs to be passed to the JmsTemplate.execute() for the connection to be started.)

%[https://gist.github.com/784cdb61d4b9161c96fb364d9d1249a0]

#### About Pooling

Once the request/response logic was correct it was time to load test. Almost instantly, memory usage exploded and the garbage collector started thrashing. Inspecting ActiveMQ with the [Web Console](http://activemq.apache.org/web-console.html) showed that MessageConsumers were hanging around even though they were being explicitly closed using Spring’s own JmsUtils. Turns out, the [CachingConnectionFactory](http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/jms/connection/CachingConnectionFactory.html)’s JavaDoc held the key to what was going on: “Note also that MessageConsumers obtained from a cached Session won’t get closed until the Session will eventually be removed from the pool.” However, if the MessageConsumers could be reused this wouldn’t be an issue. Unfortunately, CachingConnectionFactory caches MessageConsumers based on a hash key which contains the selector among other values. Obviously each request/response call, with its necessarily unique selector, was creating a new consumer that could never be reused. Luckily ActiveMQ provides a [PooledConnectionFactory](http://activemq.apache.org/maven/activemq-core/apidocs/org/apache/activemq/pool/PooledConnectionFactory.html) which does not cache MessageConsumers and switching to it fixed the problem instantly. However, this means that each request/response requires a new MessageConsumer to be created. This is adds overhead but its the price that must be payed to do synchronous request/response.