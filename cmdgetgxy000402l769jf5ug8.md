---
title: "Decoding strings with an unknown encoding"
datePublished: Thu Sep 15 2011 11:49:40 GMT+0000 (Coordinated Universal Time)
cuid: cmdgetgxy000402l769jf5ug8
slug: decoding-strings-with-an-unknown-encoding-349fe7fa57d2

---

![Unicode Snowman](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302127797/844512a5-0ef7-4aa5-a7da-5ae743487919.png)

We’ve all been there, the system is humming along when you bring up the UI only to see “Espa?ol” staring you back in the face. What is this? Why is there a question mark in there? Well, you’ve been bitten by they [mystical world of unicode](http://www.joelonsoftware.com/articles/Unicode.html). Usually this issue is an easy fix; [always use UTF-8 encoding](http://webdosanddonts.com/always-use-utf-8-encoding). However, what to do if you don’t own the whole code path? What if you have to support poorly written third party client libraries? What if you have client libraries in the wild that will never be updated? What if these client libraries out right lie about what encoding they are using? Follow me and we’ll find out.

#### Bytes To String

On the JVM there are two standard ways of converting bytes to Strings; new String(bytes\[\], encoding) and using the NIO Charset. Since they both accomplish the same feat my decision came down to performance. Luckily someone else did [the heavy lifting](http://www.javacodegeeks.com/2010/11/java-best-practices-char-to-byte-and.html) and figured out that new String(bytes\[\], encoding) ekes out a small win over NIO Charset. However, this option poses a challenge. How do we know if the decoding succeeded? The NIO option can throw an Exception (effective but slow) or insert a [Unicode Replacement Character](http://en.wikipedia.org/wiki/Specials_%28Unicode_block%29) (easy to find with a String.contains()) if it encounters a some bytes that cannot be decoded . new String(bytes\[\], encoding) does no such thing. It blindly decodes characters and will output random garbage in the resulting string if the decoding chokes on some bad byte values. We need a way to find those garbage characters.

#### Regex for non unicode characters

The clue that got me on the right track was an odd looking pattern in the [javadoc for Pattern](http://download.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html).

\[\\p{L}&&\[^\\p{Lu}\]\] Any letter except an uppercase letter (subtraction)

It seemed that \\p{L} was some magical regex incantation for any unicode letter and after some additional searching it appeared that [that was exactly the case](http://www.regular-expressions.info/refunicode.html). Of course what we really want to find are characters that are not unicode letters, spaces, punctuation or digits. Lucky for us there are matching groups for all of these, leading us to this regex:

\[^\\p{L}\\p{Space}\\p{Punct}\\p{Digit}\]

#### Performance Testing

Since performance is of particular concern it was important to test the overhead of this regex check. I fired up the Scala REPL and [tested](https://gist.github.com/1203677) the using new String(bytes\[\], encoding) with the above regex as compared to NIO Coded using String.contains() to check for the replacement character. After all that work it turns out that the regex was significantly more expensive than String.contains(). So much so that the NIO code was about 2x as fast. So, in the end, I ended up going with the simpler NIO option.

The Code

%[https://gist.github.com/d080cf34f35df6c687cd5ee5a3c899f4]

#### Update

Thanks to Joni Salonen for pointing out that I was wrong about new String() not inserting the unicode replacement char. In light of that info the following code is just a touch faster.

%[https://gist.github.com/d16fbae58b5948116fdc0d6af7eab29d]