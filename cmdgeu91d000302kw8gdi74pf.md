---
title: "Monitoring ActiveMQ Using JMX Over SSH"
datePublished: Sat May 23 2009 00:20:43 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeu91d000302kw8gdi74pf
slug: monitoring-activemq-using-jmx-over-ssh-24dabf67b9bf

---

If you want to know true confusion and frustration I suggest you attempt to monitor [ActiveMQ](http://activemq.apache.org/) using JMX over SSH port forwarding.

For those who use ActiveMQ, the JMX monitoring is some pretty impressive stuff. You can dive into connections, queues, topics, subscriptions, etc and get stats about the current state of the system. However, this nirvana of information is practically unreachable when AMQ is in a data center. For the first six months or so working on AMQ I was able to get around this using the [web console](http://activemq.apache.org/web-console.html) which gives some level of detail on topics and queues but once we moved into trying to debug bigger and bigger problems the console wasn’t cutting it any longer.

**The Investigation Begins**

As with anything AMQ, I started looking into the [JMX documentaion](http://activemq.apache.org/jmx.html). Step one was to enable JMX in the activemq.xml file.

<broker useJmx=”true” brokerName=”BROKER1">

Fire it up, forward port 1099 (the default JMX port), give it a try, and, no go. Come to find out, JMX uses two ports, one which is defaults to 1099 and one which is negotiated at runtime. No worries, there is an “Advanced JMX Configuration” seciton in the documentation which lead me to this xml configuration.

<managementContext>  
    <managementContext   
        connectorPort="2011"  
        jmxDomainName="test.domain"/>  
</managementContext>

Awesome, I can change the default 1099 port but nothing else. The AMQ documentation was failing me, as usual. Google, here I come. After some searching around I was able to dig up [AMQ-892](https://issues.apache.org/activemq/browse/AMQ-892). (Power user tip, search AMQ’s Jira site once the documentation has inevitably failed.) It turns out you can configure the RMI server port and have been able to do so since version 4.1.

<managementContext   
    connectorPort="11099"   
    rmiServerPort="11119"  
    jmxDomainName="org.apache.activemq"/>

I particularly liked the “This necessitates a documentation update in the wiki.” comment from 2007. Way to jump on that. Confident that this is bound to work I forward port 11099 and 11109, give it a whirl, and, failure. It was at this point that I started to despair.

**Deeper Down The Rabbit Hole**

Quickly running out of options I decided to try running the same configuration on a dev box in the office, and of course I could connect to it flawlessly. To add insult to injury I tried another dev box and this one failed. The only difference I could easily see was one was running a VM image and the other was native but this didn’t point at any easily understandable target. I was done. I conceded defeat. I would soldier on without JMX.

Meanwhile, my co-worker had been pressuring me to start packet sniffing with wireshark. I had resisted, mostly because I dislike getting that low level, but also because RMI is a binary protocol and I didn’t think I could learn much. However, I was bloody and beaten at this point and gave in. I downloaded the package, fired it up and started sniffing away. Sure enough the binary was a bunch of gibberish, but tucked away in there was one string, the hostname of the amq server I was trying to connect to. What the hell is this?!? That question is left to the reader but at least the issue was clear, the hostname was inside the data center and not something I could connect to from my machine. It also jived with the test on the local dev boxes. The box with the VM image had an internal hostname that was not in the office dns, where as the native machine had an addressable hostname.

I attacked the problem with renewed vigor. I found a [JConsole FAQ](http://activemq.apache.org/i-cannot-connect-to-activemq-from-jconsole.html) on the AMQ website that I had previously dismissed for its cryptic message. However, this time it made sense. I quickly added

\-Djava.rmi.server.hostname=127.0.0.1

to the Java Service Wrapper configuration for AMQ so communication would be forced back through the SSH tunnel, rebooted AMQ, and, wait for it, VICTORY.

**The Morale of the Story**

So the short answer is add the broker configuration, the management context (the second on up there), and the rmi hostname configuration, port forward 11099 and 11109, and fire up jconsole

jconsole service:jmx:rmi://127.0.0.1:11119/jndi/rmi://127.0.0.1:11099/jmxrmi

As with most of these posts, so easy once you know, but so hard to get there.