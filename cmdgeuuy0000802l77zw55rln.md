---
title: "Mathematical Purity in Distributed Systems: CRDTs Without Fear"
datePublished: Mon Jan 13 2014 18:52:42 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeuuy0000802l77zw55rln
slug: mathematical-purity-in-distributed-systems-crdts-without-fear-3365ccf68988
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753303121052/c1da5ab4-1a7a-4dfd-9bea-c18244463ad1.webp

---

This is a story about distributed systems, commutativity, idempotency, and semilattices. It’s a real nail biter so put the kettle on to boil and settle in. Our story starts one bright morning at your budding mobile gaming startup a few days before the big launch. Lets watch.

### The Simplest Thing That Works, Right?

Its Thursday and the big launch is just a few days away. You are putting the finishing touches on your viral referral loop when you think to yourself, “wouldn’t it be grand if we tracked how many levels our players complete”. Since you fully expect that you are about to release the next [Candy Crush Saga](http://about.king.com/games/candy-crush-saga) you don’t want to build something that won’t scale so you bust out your favorite NoSQL database, [MonogDB](http://www.mongodb.org/) and turn on the webscale option. You lean back in your chair and after a few minutes of staring at the ceiling you come up with the perfect schema.

{  
\_id: hash(&lt;user\_name&gt;),  
level:  
}

You flip over to your mobile app and wire up a “new level” message that gets sent back to your servers. Then, in the server code you process the message and increment the appropriate MongoDB record.

### But we don’t even have that many levels!

Your app launches with a feeble roar (its ok, just wait for TechCrunch to pick it up) and you decide to check in on your analytics. You start poking around and quickly notice that some user has played to level 534 when there are only 32 levels in the game. What gives? You poke around some more and realize that this user is actually your co-worker Lindsey. You wander over to her desk to see if she can think of anything that might have caused this. The two of you pour over the logs from the app on her phone only to find a lot of network timeouts and retries. Thats when Lindsey says “You know, I was at my parents house last night showing them the game and the cell service there is pretty crappy. I wonder if the game kept resending the “new level” message thinking that the send had timed out, when in fact the server had received the message but wasn’t able to acknowledge the message fast enough.” Your stomach quickly sinks as you realize that this behavior will totally screw up your numbers. The two of you wander over to a whiteboard and in short order you realized the MongoDB increment function has to go. Instead, the two of you conclude that the game should send a “level X” method to the server and the server will simply write the level number into MongoDB. A quick update to your server and a lengthy update of the app later (because Apple) your analytics are starting to look better.

### Idempotency

What you’ve just experienced is a the pain that comes from a system that lacks [idempotency](https://en.wikipedia.org/wiki/Idempotency). Idempotent operations are ones that can be safely retried without effecting the end result of the system. In the first design, the increment function is not idempotent since when it is retried it increments the MongoDB number again even though it shouldn’t. By storing the level number that is sent along with the message you transformed the system into one that is idempotent since storing the same level number over and over will always end up with the same result.

### I know I played more levels than that!

TechCrunch is taking its sweet time to cover your awesome game so after blindly throwing half of your seed money at Google AdWords you decide to check in on the analytics again. This time, you start by looking at your personal record and notice another oddity; you’ve beaten all 32 levels and yet your analytics record has only recorded up to level 30. You mull it over for a few minutes and realize when you beat the game you were on the subway where there wasn’t any cell reception. You take out your phone and check the logs and sure enough you still have two “level X” messages waiting to be sent back to the server. You start up the game and watch the logs as the last two messages are safely sent off to your servers. For fun you’ve been tailing the server logs as well and notice something interesting; the two messages where handled by two different servers. “Just the load balancer doing its job” you think. Back to MongoDB you go, but instead of the 32 you were hoping for, you see a 31 staring you in the face. What the? And then it hits you. The “level 31” message must have been processed a little bit slower than the “level 32” message and overwrote the 32 in MongoDB. “Damn this stuff is hard”. You head back to Lindey’s desk and explain the problem to her. The two of you hit the whiteboard again and decided that what you really should do is update the value in MongoDB to be the maximum of the value in MongoDB and the value in the message. Luckily this change can be done solely on the server side (no waiting for Apple). A quick deploy later and you are feeling pretty smug.

### Commutativity

This time you were bitten by the lack of [commutativity](https://en.wikipedia.org/wiki/Commutative_property) in your system. Commutative operations are ones that can be processed in any order without effecting the end result of the system. In the second design updating the value in MongoDB was not commutative since it blindly overwrote any previous value resulting in a last write wins situation. By using the maximum function you transformed the system into one that is commutative since the maximum of a stream of numbers always returns the same result regardless of the order of the numbers.

### Monotonic Semilattices

Now we are breaking out the big words. As it turns out, if you create a system that is idempotent and commutative (and [associative](https://en.wikipedia.org/wiki/Associativity) but that almost always goes hand in hand with commutative) you have created a [semilattice](https://en.wikipedia.org/wiki/Semilattice). Moreover, in your particular case, you created a [monotonic](https://en.wikipedia.org/wiki/Monotonic_function) semilattice. That is to say, you created an idempotent and commutative system that grows only in one direction. Specifically, the number in MongoDB only increases. Now why is this interesting? Well, monotonic semilattices are the building blocks for [Conflict-free Replicated Data Types](http://muratbuffalo.blogspot.com/2013/04/conflict-free-replicated-data-types.html). These things are [all the rage](http://highscalability.com/blog/2010/12/23/paper-crdts-consistency-without-concurrency-control.html) and Riak has already implemented one CRDT for their [distributed counters](http://basho.com/counters-in-riak-1-4/) feature.

### Wrapping Up

So as you can see none of this stuff is particularly hard but when these simple principals are combined they can make life much easier. Go forth and wrangle some distributed systems!