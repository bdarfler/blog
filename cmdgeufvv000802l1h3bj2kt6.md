---
title: "REST calls and JSON results in Java"
datePublished: Mon Apr 20 2009 02:01:46 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeufvv000802l1h3bj2kt6
slug: rest-calls-and-json-results-in-java-4c02f498258d

---

I recently had the need interact with [Twittervision’s](http://twittervision.com/) RESTful api from Java. As usual, I started googling around for a solution but nothing obviously stood out. There are a handful of RESTful java frameworks, but these focus on providing a RESTful api and not consuming one. Finally, after an annoyingly difficult search I found an impressively simple solution based on [Jersey](https://jersey.dev.java.net/).

The point to realize is that Jersey provides both a sever and a client api. To make use of the client api requires three simple lines:

%[https://gist.github.com/be6b7ca43303a917f5aadb085f455bc7]

The result is a string of JSON, but now, how to parse it. After searching around I ended up settling on [json-simple](http://code.google.com/p/json-simple/). A few simple lines of code and we can get the address for any twitter user.

%[https://gist.github.com/c8a90f48e8135fdea134fe20def4597a]

So, once again, simple stuff but not as obvious as I was hoping it would be.