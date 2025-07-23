---
title: "Introducing Chronophage"
datePublished: Tue Jun 04 2013 01:17:24 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeuixr000502lb7ly7bywy
slug: introducing-chronophage-2c51ff773deb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753303378705/01d1f1f4-1a25-4244-a67f-b82825faba9c.webp

---

At [Localytics](http://www.localytics.com/) we deal with data, lots of data, and that data is never as clean as you want it. When we developed the first version of our upload API, thrown together during sleep deprived coding binges while participating in [TechStars](http://www.techstars.com/), the decision was made to upload datetimes as [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) formatted date strings. Code was written, uploads were sent, and life was grand; or at least we thought it was. You see Ruby, being the helpful language that it is, very kindly parses all manor of garbage, often quietly ignoring huge swaths of the string that it couldn’t make sense of. But I’m getting ahead of myself.

#### Putting on Grownup Pants

When I was brought on board, I was tasked with rewriting the processing pipeline which was beginning to creak under the weight of the data we were receiving. Being a fan of the JVM and wanting to hang out with all the cool kids, I decided to use Scala for the rewrite. Piece by piece the pipeline was rewritten, care was taken to unit test and code coverage was high. Then the fateful day came when we threw the switch and migrated to the new system. No sooner had we completed the migration then alerts started flying. Date strings were all over the map. Twelve hour time? Hindi? Kanji? What was this garbage? Only now were we seeing the dirt that Ruby had been silently sweeping under the rug for us. With a bit of scrambling the first iteration of what would become Chronophage was unleashed upon the world.

#### The Problem

It turns out, what we thought was ISO formatting was in fact a crap shoot based upon clients locale settings, language settings, and who knows what else. Dates were frequently formatted in 12 hr time and then kindly translated to whatever language the client was in. Did you know priekšpusdienā is Latvian for AM? Neither did I.

#### Chronophage

Fast forward a few years and Chronophage has been battle-hardened by trillions of datapoints. It supports twelve hour time in dozens of languages and locales, thanks in large part to [Dave Rolsky](https://github.com/autarch)’s [DateTime-Locale](http://search.cpan.org/~drolsky/DateTime-Locale-0.45/) project for the locale translations, and a wide variety of other odd formattings, thanks to the [Joda Time](http://joda-time.sourceforge.net/) library’s ISO datetime parser. And now, it is with great pleasure that I can announce we are releasing Chronophage as an open source project. Go have a look at the [GitHub](https://github.com/localytics/chronophage) project and if you find yourself drowning under a deluge of unintelligible date strings give it a spin.