---
title: "Testing AWS Scala Microservices"
datePublished: Sat May 02 2020 13:13:20 GMT+0000 (Coordinated Universal Time)
cuid: cmdgetfaf000302ju3mlrfgyz
slug: testing-aws-scala-microservices-7f5ef4c391d4
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753302125867/4d679ba1-42e0-40c7-b718-9885d498b36c.jpeg

---

*This article was* [*originally posted*](https://eng.localytics.com/testing-aws-scala-microservices/) *on the* [*Localytics Engineering blog*](https://eng.localytics.com/)*.*

Photo Credit: [NatalieMaynor](https://www.flickr.com/photos/nataliemaynor/)

In January and February of this year we open-sourced three SBT plugins that have had a huge impact on our ability to test our AWS based Scala microservices architecture:

*   [sbt-s3](https://github.com/localytics/sbt-s3)
*   [sbt-dynamodb](https://github.com/localytics/sbt-dynamodb)
*   [sbt-sqs](https://github.com/localytics/sbt-sqs)

This is our story[.](https://www.youtube.com/watch?v=gP3MuUTmXNk)

Stop me if you’ve heard this one before. After one too many production bugs in the early days of the company, some engineer (me) goes off and builds up a minimal set of end to end tests. Feeling smug, this engineer moves on with his life and gets back to shipping features. Time moves on, the team grows (hey everyone), revenue flows in (sweet), microservices flourish (awesome) and the end to end tests grow like a Kudzu (dear god). What once was a fast and stable set of tests that covered the integration of two microservices is now a massive, slow and brittle test suite that attempts to test the complex interactions between nearly a dozen services. Yes, ours is not a unique story but we’d like to think that we have brought our unique brand of pragmatism to bear on the problem.

### Understanding The Problem

Our progression towards a sane testing setup began at our fall [hackathon](http://hackathon.localytics.com/). I had a suspicion that our end to end tests were slowing us down but I wanted to lead with data. Thanks to the awesome work of [Nate Tenczar](https://www.linkedin.com/in/nate-tenczar-79057b90) we use a [slack bot](http://eng.localytics.com/serverless-slackbots-powered-by-aws/) named Dibs to moderate access to a number of shared resources including our end to end test environment. Dibs is a pleasure to work with and on the side it dumps metrics into our internal “dogfood” system. (Yes, we use Localytics to monitor parts of Localytics.) After some quick digging, I found that it often took developers 3 hours to run the end to end tests. Since our build server clocked the tests at around 15 minutes per run this meant that folks were frequently rerunning the test suite multiple times due to flaky tests and broken infrastructure.

With this data in hand, it didn’t take long to drum up support for addressing the issue but first we wanted to know what, specifically, was causing the pain. We knew the tests felt “icky” but could we quantify that better?

After a few rounds of lively discussion we came up with three core issues:

1.  The tests ran serially — we had only one shared test environment
2.  The tests were fragile — any team’s service could break the tests and block everyone else
3.  The tests were overused — the end to end test suite was a catch-all for anything with more scope than a unit test

### A Solution Appears

With the problem more clearly understood we turned to the literature and found Martin Fowler’s excellent slide deck on [Testing Strategies in a Microservice Architecture](http://martinfowler.com/articles/microservice-testing). His diagnosis was spot on:

> *Since end-to-end tests involve many more moving parts than the other strategies discussed so far, they in turn have more reasons to fail. End-to-end tests may also have to account for asynchrony in the system, whether in the GUI or due to asynchronous backend processes between the services. These factors can result in flakiness, excessive test runtime and additional cost of maintenance of the test suite.*

It was now very clear to us that we were missing some of these “other strategies” that Fowler spoke of. More specifically we needed a way to test an entire service in isolation. This would allow each developer to test locally in parallel without fear of broken external dependencies. Moreover, it would give us a place to migrate most of the end to end tests. We still wanted a limited set of end to end tests as a sanity check but the vast majority of tests were really service level tests with no other place to live.

### AWS Dependencies

The question then turned to one of execution. The end to end tests were initially set up to address the difficulty of testing locally with all the necessary AWS service dependencies. While some might propose refactoring our systems to allow for injecting fake test dependencies we tend to take a more pragmatic approach to testing that doesn’t require abstracting away our underlying services just so we can test. This does blur the line between a unit and an integration test but we find it gives us higher code coverage with less time spent writing tests. 5 years ago we had no other options than to test directly against AWS services. Thankfully things have progressed since then.

At our first hackathon, a small team of engineers built a Docker setup that allowed developers to run the end to end tests on a single laptop. While it served its purpose it was a bit of a pain to setup and even more of a pain to maintain. After struggling with orchestration, networking, and the manic pace of Docker development we came to a simple conclusion. Docker, with all of its idiosyncrasies, was adding more overhead than value to our development process.

However, when you scratched beneath the Docker surface, there was a heart of gold. The crew that built this project had found a whole suite of open-source, stand-alone mocks for a variety of AWS services, and, after some hunting around, we discovered [Graham Rhodes](https://github.com/grahamar) from [Gilt](http://tech.gilt.com/) had written an SBT wrapper for [one of them](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.DynamoDBLocal.html). We quickly jumped on this pattern and ran with it. Graham [hadn’t had the time](https://twitter.com/bdarfler/status/679695807214436352) to keep up with the project so we gave it a [new home](https://github.com/localytics/sbt-dynamodb) and a few siblings in the form of an SBT wrapper for [SQS](https://github.com/localytics/sbt-sqs) and for [S3](https://github.com/localytics/sbt-s3).

These SBT plugin projects have a number of significant wins over the previous Docker setup. Once the SBT plugins are integrated into a project they are responsible for downloading their respective mock services. This zero setup configuration is not only super convenient for developers but it also makes it trivial to run in our continuous build system. Additionally, the plugins are designed to hook into the SBT lifecycle. This allows them to be transparently started before the tests begin and spun down after the tests complete. Again, this removes a significant barrier for both developers and the continuous build system. Finally, by removing the pain of the Docker setup, they make testing easier and more enjoyable and that leads to more tests, better code coverage, and improved code quality.

### Wrapping Up Loose Ends

With the AWS dependencies settled we turned towards dealing with SQL. Thankfully this is a bit more of a solved problem. We had been slowly migrating over to [ScalikeJDBC](http://scalikejdbc.org/) as our SQL library of choice and their [testing support](http://scalikejdbc.org/documentation/testing.html) is fantastic. We whipped up a small utility for loading schema and seed data and, after some small tweaks to our SQL queries, settled on [H2](http://www.h2database.com/html/main.html) for our testing database.

With all of our dependencies taken care of we enabled the [integration test support in SBT](http://www.scala-sbt.org/0.13/docs/Testing.html#Integration+Tests) and were able to move our first end to end test into a locally runnable integration test.

We still have a ways to go to dismantle the end to end tests but we now have a vision for how to get there and a set of tools to help us along the way.