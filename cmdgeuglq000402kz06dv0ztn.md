---
title: "Controlling Log4j Logs with Contexts and Filters"
datePublished: Fri Nov 21 2008 22:41:04 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeuglq000402kz06dv0ztn
slug: controlling-log4j-logs-with-contexts-and-filters-acbb6b19fd71

---

Its a problem we’ve all had. Something has gone awry, you jack up the log level to debug and all of the sudden you are inundated with everything under the sun. While one option could be changing some of the logs to trace level there is another option, using Log4j contexts. Log4j contains two different contexts [mapped](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/MDC.html) and [nested](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/NDC.html). In this post I’ll focus on nested.

The first thing to do is to isolate the section of your code that you want to create a context for. Once you have accomplished this you make a static call to [NDC](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/NDC.html).push(). The parameter passed in is the string name for the context you would like create. I recommend pulling these strings out into a constants file ease of use and additionally calling .intern() on them so you can use pointer equality checks later when we code up the filters. When you leave the context you call [NDC](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/NDC.html).pop() and life goes back to normal. Finally, remember to call [NDC](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/NDC.html).remove() when you leave the thread.  
The code ends up looking like this:

%[https://gist.github.com/6716fd2eb9ef985cd64a574e39929f8c]

Now that we have the logs tagged with the correct context we need to create a log4j filter by extending the [filter](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/spi/Filter.html) class and overriding the decide(LoggingEvent event) method. The important concept to remember here is that filters can be chained which means your decision results in either a DENY, ACCEPT, or NEUTRAL decision where deny forces the log to be rejected, accept force the log to be written and neutral passes the decision down the chain. Additionally, you access the context by calling getNDC() on the LoggingEvent object passed in to the decide method.  
The code then looks like this:

%[https://gist.github.com/3cd0a0992f2454b16d547ff3cf7451eb]

Finally we have to add this filter to our log4j configuration. For this we have to make sure we are using the xml configuration file type as it is more expressive and allows us to define filters to use where as the property configuration file type does not. All we have to do is add a filter tag to an appender in the xml and point it at our class.  
It looks something like this:

%[https://gist.github.com/0621cf8e168c4c9e9e7ed7cc895c7859]