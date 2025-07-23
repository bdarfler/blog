---
title: "Using Enumerations to Make a Faster Activity Feed in Rails"
datePublished: Thu Jan 30 2014 20:00:02 GMT+0000 (Coordinated Universal Time)
cuid: cmdgettvt000202jlb2bobovs
slug: using-enumerations-to-make-a-faster-activity-feed-in-rails-321b2b69f6ca
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753302886714/3ec40c98-fd18-4186-9d7f-d339992fab7c.jpeg

---

A month ago the bright guys over at [Thoughtbot](http://thoughtbot.com/) wrote a blog post about [using polymorphism to make a better activity feed in rails](http://robots.thoughtbot.com/using-polymorphism-to-make-a-better-activity-feed-in-rails). Now, I have great respect for the work that Thoughtbot does and in particular I’m a huge fan of their blog content. However, on this specific issue I have to take exception. As it turns out, we’ve stared this issue in the face over at [GiveGab](https://www.givegab.com/) and while the solution that Thoughtbot proposes may seem tempting, there are some serious issues to consider. I’ll explain my solution shortly but before that I’d like to take a step back and talk about activity feed design in the abstract.

### Fanning In or Out

First off, I highly recommend [Thierry Schellenbach](https://twitter.com/tschellenbach)’s [post](http://highscalability.com/blog/2013/10/28/design-decisions-for-scaling-your-high-traffic-feeds.html) over at [High Scalability](http://highscalability.com/). It does a great job giving an overview of the design decisions that go into building an activity feed. However, the decision I particularly want to call out is wither to fan out on write or fan in on read. That is to say, do you materialize the activity feed on write by updating a table as the activities happen or do you materialize the activity feed on read by selecting from all the tables that populate the activity feed? There is the option of a hybrid approach, but the complexity prevents this implementation from making sense in most cases.

### Where Thoughtbot Went Wrong

In the Thoughtbot post, they walk through an example of an ebay like site where users can both bid and comment on items. From there, they advocate creating a fan out on write feed. In the post, they juxtapose this with a fan in on read example which they label as an “anti-pattern”. Unfortunately, the fan in on read example that they refer to is the most naive implementation that one could conceive of.

%[https://gist.github.com/70fe90d3267a0f1c7ff016208de72ca7] 

This code suffers from two egregious issues. First, the entirety of the activity sources are being eagerly loaded from the database and turn into Ruby objects. This obviously won’t scale. Second, once all the records are loaded they are then sorted in Ruby. It’s quite clear that we should be pushing most of this work down to the database. However, instead of attacking these problems, Thoughtbot adds another problem; significant additional write load on the database. Without the feed, every bid used to result in one record being created. With the feed, a new bid now creates both bid record and an activity record. The comments situation is even worse, since the number of commenters on popular items is unbounded and every new comment triggers a write for every other commenter on the thread. I would argue this won’t scale beyond a trivial example either.

### A Better Approach

We’ve seen that a fan out on write approach can cause some serious write time pain, so lets flip back to the fan in on read approach. The initial naive implementation that Thoughtbot held up is flawed, but can we improve on it? Well, as it turns out we can, and by a large margin. The first step is to stop eagerly loading all of the results. This is as simple as using the [find\_each](http://api.rubyonrails.org/classes/ActiveRecord/Batches.html#method-i-find_each) method on each model and allowing Rails to batch the database loads for us behind the scene. We can even go one step further and convert the result to an enumerator using enum\_for. We will see how this helps in a second. The second issue with the naive approach was sorting, which we can also push to the db in part by using a simple order(created\_at).

%[https://gist.github.com/4eb43db85e6a0c92095fb907b6c29d25] 

However, we still have to contend with sorting between the various streams. This is where the magic comes in. We have a collection of ordered streams of records, so let’s create a simple aggregator that pulls the next newest record from amongst all the streams and continues in a loop until it has pulled enough records to satisfy the limit parameter.

%[https://gist.github.com/7fe3d3fd3ab35957cb6ed5ca8a23d03a] 

When we put this all together, we end up with an elegant solution that lazily loads data as needed from the database and pushes most of the heavy sorting down to the database level. This solution results in a small number of database queries and can be further tuned by matching the batch size of the find\_each to more closely align with the limit parameter. Moreover it greatly reduces the write load on the database and keeps everything humming long much more smoothly.

%[https://gist.github.com/74a2cc9f8f70de31b5515d889085b182]