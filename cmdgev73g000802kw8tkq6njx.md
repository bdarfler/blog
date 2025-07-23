---
title: "How I Distill the Internet in 2023"
datePublished: Mon Oct 30 2023 12:22:53 GMT+0000 (Coordinated Universal Time)
cuid: cmdgev73g000802kw8tkq6njx
slug: how-i-distill-the-internet-in-2023-111b9dc29b91
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753302208132/d19eb9e7-0541-470e-8d2e-5f2e271fb71f.jpeg

---

I recall a time when the internet felt smaller. When you could keep on top of your corner of the internet armed with a feed reader (R.I.P. [Google Reader](https://en.wikipedia.org/wiki/Google_Reader)) and a curated set of feeds.

Those days are gone.

These days, content comes fast and furious, and it’s a challenge to stay on top of it, let alone find the signal in the noise. (Have you noticed that more and more posts feel like ChatGPT wrote them? Or ChatGPT should have?)

I receive frequent appreciation for the high-quality content I share on my [personal website](https://bdarfler.com/), [LinkedIn](https://www.linkedin.com/in/bdarfler/) and [Twitter](https://twitter.com/bdarfler) (I’m not calling it X). So how do I do it?

Below is the set of tools I’ve compiled and the process I’ve put in place to distill the internet in 2023.

### Acquire

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302202299/5f3f6959-42bb-42b1-a2d6-faaefe586ae6.jpeg)

The first step is acquisition. As I said, there was a time when you could keep on top of things with a feed reader and a good set of feeds. That time has passed. Don’t get me wrong, I still use a feed reader ([Feedly](https://feedly.com/)) but at this point I only use it to follow friend’s blogs and local news.

Instead, these days, I rely on the outstanding work of others:

*   [https://techproductivity.co/](https://techproductivity.co/)
*   [https://softwareleadweekly.com/](https://softwareleadweekly.com/)
*   [https://www.pointer.io/](https://www.pointer.io/)
*   [https://levelup.patkua.com/](https://levelup.patkua.com/)
*   [https://investing.io/](https://investing.io/)
*   [https://newsletter.leadershipintech.com/](https://newsletter.leadershipintech.com/)

These newsletters curate the best links of the week and deliver them directly to my email inbox. From there, I can select which links are worth reading further. That said, there is a very select list of writers that I do follow:

*   [https://www.edbatista.com/](https://www.edbatista.com/)
*   [https://stratechery.com/](https://stratechery.com/)
*   [https://newsletter.pragmaticengineer.com/](https://newsletter.pragmaticengineer.com/)
*   [https://www.rubick.com/](https://www.rubick.com/)
*   [https://www.theengineeringmanager.com/](https://www.theengineeringmanager.com/)
*   [https://lethain.com/](https://lethain.com/)

These authors write some of the best content out there, and most or all of their posts are worth reading. In the past, I would have used a feed reader to follow these writers, but that added an extra step to the process. Instead, I now skip that step and push new posts to Pocket using [Read Email Later](https://www.reademaillater.com/). And that brings me to …

### Buffer

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302203952/a0043d92-9e56-46cb-9b51-9539ae125cc2.jpeg)

Reading content as you find it is a poor use of time. Even worse is letting it sit in a browser tab, forever intending to get back to it, until your browser hard crashes, and you lose it all. Instead, you need a place to buffer the content.

The tool I use is [Pocket](https://getpocket.com/). I’ve tried [Instapaper](https://www.instapaper.com/) a few times over the years when a Pocket update irritated me in some way, but I always end up back with Pocket for my personal workflow.

If an article is particularly short (less than 5 minutes read according to Pocket) I’ll read it right in the android app. However, if the article is a longer read, I’ll use [Push to Kindle](https://play.google.com/store/apps/details?id=org.fivefilters.pushtokindle&hl=en_US&gl=US) on my android to send it to my Kindle. This flow works 95% of the time. For sites that don’t render well due to poor formatting or aggressive paywalling, I’ll fall back to [Send to Kindle](https://chromewebstore.google.com/detail/send-to-kindle-for-google/cgdjpilhipecahhcilnafpblkieebhea) in the browser.

This flow is fantastic for written content, but I do come across audio and video content that I would also like to buffer. If you know of good, free tooling in this space, I would love to hear about it, but for now, I have had to hack together my own set of scripts.

For videos, I [download them and convert them to mp3s](https://github.com/bdarfler/dotfiles/blob/ddac1d41318df2fa9da5271eb6acb9014293ec47/.zshrc#L59-L61). Once they are in mp3 format, they join any other audio content in my personal podcast feed, which I [generate and push to AWS S3](https://github.com/bdarfler/dotfiles/blob/master/bin/podup). This works, quite well even, but I wouldn’t mind a prebuilt tool.

### Consume

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302205044/9282d4d8-54d8-47f3-8d02-4b00261b04f3.jpeg)

As you might expect, the core of this step is to actually read. However, that can be easier said than done. I like having short content in Pocket, so I can read a quick article when I have a bit of downtime. However, for those longer articles, it takes carving out consistent time in your day. That could be early in the morning with a cup of coffee, on a lunch break, or, like me, as evening reading before bed. Whatever the process, the content won’t read itself.

One tip is to be judicious in what you spend time reading. If an article isn’t sparking your interest, switch to skimming rather than detailed reading. If it still isn’t intriguing, archive it and move on. You can also keep a general awareness of which content you read vs skip and feed that back into the acquire step. There are some newsletters where I’ll add every single link to Pocket. There are others where I will add only one or two links per email. And of course, I have added and then unsubscribed from dozens upon dozens of newsletters at this point.

Beyond reading, there is one more part to this step in the process; highlighting. It is easy to think “of course I’ll remember what stood out to me about this article” and you will be dead wrong. Instead, as you are reading, make use of the highlighting feature in whatever tool you are using. Both Pocket and Kindle have this functionality. In many ways, this is yet another buffer leading up to the final step …

### Distribute

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302206480/18e931fb-de4f-4f0e-b36f-eee09f109b6d.jpeg)

I know plenty of folks who follow the process up to this point, but then store those highlights away in their own knowledge base. I have always found that approach suboptimal.

First, I shy away from digital stockpiling generally. It offends my deep need for minimalism. If I’m being honest, this is my primary motivator. However, there is also a real benefit to sharing these articles. At this point in the process, I’ve already done 95% of the distilling work. With that last 5% effort, I get to share the value of the article with others and in return, I get the social benefit of being known as a deep reader and thinker. It's a very high leverage 5% of effort.

Both [Pocket](https://support.mozilla.org/en-US/kb/highlighting-in-pocket#w_view-share-or-delete-your-highlights) and [Kindle](https://techwiser.com/export-kindle-highlights/) provide options for exporting highlights. About once a week, I’ll then go through the highlights and turn them into posts on my [personal website](https://bdarfler.com/). At the same time, I’ll queue up social posts using [Buffer](https://buffer.com/) to post later on [Twitter](https://twitter.com/bdarfler) and [LinkedIn](https://www.linkedin.com/in/bdarfler/).

### As Easy as ABCD

There you have it, a simple four-step process for distilling the internet; finding, managing, reading, and sharing content. As with all my systems, the key is simplicity and habit building. If you can put together a small set of tools and carve out a little dedicated time for reading, you can take this on. Over time, the value compounds. When I started my personal website 15 years ago, it was a place to throw random links. But over time it has grown to over 1,500 posts, which I regularly refer to and share with those around me.

> The best time to plant a tree was 30 years ago, the second best time to plant a tree is now.