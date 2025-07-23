---
title: "No-Install Domain Setup Zen"
datePublished: Mon Jan 12 2009 02:59:04 GMT+0000 (Coordinated Universal Time)
cuid: cmdgewj2n000602lbct4x4uxd
slug: no-install-domain-setup-zen-b958caff82e9

---

![No Install](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302270590/c5b17244-f4fb-42d0-9efc-307270d97b7c.jpeg)

In this down economy the chatter about [owning](http://personalbrandingblog.wordpress.com/2008/12/12/2009-personal-branding-predictions/) [your own](http://personalbrandingblog.wordpress.com/2008/11/12/your-complete-guide-to-domain-name-branding/) [domain](http://www.otiscollier.com/buy-your-name-as-a-domain) as a way of personal branding has been on the rise. However, the elephant in the room is what to do with it once you have it? For a large majority of people, installing and maintaing blogging software like [WordPress](http://wordpress.org/), [Moveable Type](http://www.movabletype.com/), or [Drupal](http://drupal.org/) is cumbersome, expensive, and unnecessarily time consuming. Why go this route when everything else online is moving towards the cloud computing model? I’ll show you how you can set up not only a blog but email, photos and more for little to no cost and without installing a single piece of software.

#### Getting a Domain

Go out and get yourself a [GoDaddy](http://www.godaddy.com/) domain using a [promo code](http://www.retailmenot.com/view/godaddy.com) and you can be on your way for about $7.50 a year. Nothing beats the price and since they are the 800 pound gorilla of domain hosting you are more likely to find good blog posts (like this one) about how to get your hosted applications up and running on it.

#### Putting Up Content

#### blog.mydomain.com

As I mentioned in my [previous post](http://blog.bdarfler.com/2008/12/30/this-web-20-life/) there are a really two ways to get hosted blogging, [WordPress](http://wordpress.com/) and [Blogger](http://www.blogger.com/). Blogger [integrates with GoDaddy](http://help.blogger.com/bin/answer.py?hl=en&answer=58317#godaddy) freely and easily but is less polished than Wordpress. If you go the Wordpress route the cost is $10/yr for redirecting and the set up is a bit confusing but its worth it for the superior platform.

1.  Go do your Wordpress dashboard and click on the Upgrades menu
2.  Gift yourself a $10 credit
3.  Under the domains menu add blog.<your-domain>.com
4.  Create a [CNAME record in GoDaddy](http://help.godaddy.com/article/679) that points from blog.<your-domain>.com to <your-blog>.wordpress.com
5.  Wait and verify that blog.<your-domain>.com redirects to <your-blog>.wordpress.com.
6.  Follow [these directions](http://faq.wordpress.com/2007/09/09/can-i-redirect-my-blog/) to make <your-blog>.wordpress.com to redirect to blog.<your-domain>.com

#### photos.mydomain.com

Unfortunately there is no free lunch here, the only options are [Zenfolio](http://www.zenfolio.com) and [SmugMug](http://smugmug.com/). Zenfolio’s [integration](http://www.zenfolio.com/zf/help/?topic=preferences/custom-domain) seems pretty straight forward and requires a $40/yr subscription. SmugMug is currently more full featured (for instance it integrates with [S3](http://aws.amazon.com/s3/) for storing digital negatives) and more customizable (check out this [example](http://andydemo.smugmug.com/)) but it also costs more. Their [integration](http://wiki.smugmug.net/display/SmugMug/GoDaddy+CNAME+Setup) requires a $60/yr subscription.

#### **Google Apps**

One of the main reasons for getting your own domain is having an email address with that domain. Luckily Google and GoDaddy make it very easy with two simple tutorials; one for [forwarding](http://www.google.com/support/a/bin/answer.py?hl=en&answer=47610) the various subdomains and one for setting up your [mail server](http://www.google.com/support/a/bin/answer.py?hl=en&answer=33353). Better yet, there is an [automated tool](https://www.godaddy.com/gdshop/google/gmail_login.asp) for setting up the mail forwarding.

#### Miscellaneous Tools

#### wiki.mydomain.com

I’ll let my fellow bloggers [extol](http://www.lifehack.org/articles/technology/the-quick-dirty-guide-to-personal-wikis.html) the [virtues](http://www.lifehack.org/articles/technology/advice-for-students-use-a-wiki-for-better-note-taking.html) of a [personal wiki](http://eriwen.com/tools/wikify-yourself/). Now that you are convinced, go over to [Wikidot,](http://www.wikidot.com/) make yourself an account and follow the [instructions](http://community.wikidot.com/howto:set-up-a-custom-domain) on mapping it to your wiki subdomain.

#### openid.mydomain.com

More than three years after its inception [OpenID](http://en.wikipedia.org/wiki/OpenID) is finally taking off with more and more sites providing and accepting the standard. To centralize your identity at your new domain register at [myOpenID](https://www.myopenid.com/) and follow their convenient [directions](https://www.myopenid.com/product_domains).

#### Tying it Together with Tumblr

[Tumblr](http://www.tumblr.com) is one of the most under appreciated sites. As a blogging platform it [ranks last](http://widgets.alexa.com/traffic/graph/?r=6m&y=t&z=1&h=300&w=470&c=1&u%5B%5D=tumblr.com&u%5B%5D=vox.com&u%5B%5D=livejournal.com&u%5B%5D=typepad.com&x=2009-01-14T02%3A15%3A23.000Z&check=www.alexa.com&signature=992cbPOScaG2TrvrgM5kftJfABg%3D) in traffic stats. However, its ability to function as a blog, share feed (via the bookmarklet), life stream (via importing arbitrary rss feeds), or any combination, all while remaining simple to use, is astounding. For our purposes though, the killer feature is its ability to [redirect from a custom domain](http://www.tumblr.com/docs/custom_domains). With that setup you can easily start [pulling in any feeds](http://www.tumblr.com/docs/feeds) you like. I pull in both [my blog](http://blog.bdarfler.com) and [my photoblog](http://photoblog.bdarfler.com) feeds but other options might include Twitter, YouTube, digg, etc. You could also import a share feed from delicious but it pails comparison to using the Tumblr [bookmarkle](http://www.tumblr.com/goodies)t. With the bookmarklet, content to shows up directly, instead of an imported link, and you can inject your own thoughts using comments.

But the fun doesn’t stop there. Tumblr allows you to customize every last inch of your site which has resulted in a vibrant community of 3rd party [themes](http://customthemes.tumblr.com/) and [hacks](http://hacks.tumblr.com/). After some experimentation, I settled on the [nine of mine](http://nineofmine.tumblr.com/) theme by [Sid05](http://sid05.tumblr.com/). However, the theme was only the base. From there, I [replaced the default feed with feedburner](http://hacks.tumblr.com/post/67283461/feedburner-for-tumblr), added some [google analytics](http://www.tumblr.com/docs/google_analytics), and did some [very basic SEO](http://joshauerbach.com/post/62067190/kortina-very-simple-tumblr-seo-better-page). Finally, I [added comments](http://hacks.tumblr.com/post/35662700/more-details-on-how-to-add-comments-to-tumblr) using [DISQUS](http://www.disqus.com/) and used [add to any](http://www.addtoany.com/) for a [share & save link](http://www.addtoany.com/buttons/for/tumblr) as well as an improved [subscription link](http://www.addtoany.com/buttons/subscribe).

#### Happy Hunting

Well there you have it, the culmination of all the research that went into creating [my homepage](http://www.bdarfler.com). Use it wisely and don’t forget to drop me a comment.