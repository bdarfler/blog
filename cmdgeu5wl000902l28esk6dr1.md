---
title: "Approximating Java Case Objects without Project Lombok"
datePublished: Fri Jul 09 2010 14:20:55 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeu5wl000902l28esk6dr1
slug: approximating-java-case-objects-without-project-lombok-8a6a8fbfaae7

---

![Lombok](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302160222/b0058649-f1c9-4dd9-86f0-1644d17c2dea.jpeg)

Over the past few months [Dick Wall](http://dickwallsblog.blogspot.com/) of the [Java Posse](http://javaposse.com/) has been talking up [Project Lombok](http://projectlombok.org/) and for good reason. As he points out, its great for reducing boilerplate code in basic Java data objects. However, the more important point that Dick makes is that it prevents stupid errors, particularly when adding new object fields later on in the development cycle. The only unfortunate issue is that Project Lombok requires a compiler hack that can be rather confusing to those not using a supported IDE or for those coding outside of an IDE. However, Apache Commons provides an elegant solution to this issue.

#### Project Lombok

Lets start with a quick overview of Project Lombok. At its most basic level it provides a set of annotations (i.e. @Data) that you add to your Java data objects. You simply create private fields in an annotated object and the annotation will cause a full set of constructors, getters/setters, equals/hashCode and toString to be generated in the class file at compile time while keeping a simple and minimal source file. Its an elegant solution but it comes with a fair amount of magic surrounding it.

#### Alternatives

The simple alternative to Project Lombok is to have the IDE generate the code for you. Eclipse, for instance, is very good at doing this with auto generation of constructors, getters/setters, hashCode/equals and toString. This works wonders when creating the object for the first time but, as Dick points out, its too easy to add a new field and forget about updating toString and hashCode/equals leading to confusing and subtle bugs down the road.

The other alternative, and the one I am promoting, strikes a balance between Project Lombok and IDE generation by auto generating the hashCode/equals and toString and leaving the getters/setters and constructors up to the developer as these are necessary for the new field to be of any use.

#### Apache Commons

Buried deep in the Apache Commons is the [builder package](http://commons.apache.org/lang/api-2.4/org/apache/commons/lang/builder/package-summary.html) containing utilities such as EqualsBuilder, HashCodeBuilder, and ReflectionToStringBuilder. I don’t remember when I first discovered these gems but once I did the lightbulb went off; if used them in an Abstract base class they would ensure proper implementations of equals/hashCode and toString without having to mess with every data object I create.

%[https://gist.github.com/b6412e3e204660363596743043bf76bb]

#### Drawbacks

Of course this option is not with out its own drawbacks compared to Project Lombok. First off, as mentioned before, it does not deal with getters/setters or constructors. However, as I pointed out, I don’t believe this to be a huge imposition since creating a new field without creating getters/setters and/or a constructor using that field is fairly useless. Additionally, I like that this gives ease and flexibility over which constructors are created. This has been quite handy in the most recent project I’ve been working on.

Another draw back is that it requires an Abstract base class from which all objects must inherit. Annotations are certainly a more elegant solution since it doesn’t require a given object hierarchy. However there are often good reasons for a project specific base object for reasons beyond just equals/hashCode and toString. Additionally, the base class is fully owned by the project with just the few methods necessary being implemented. This is significantly better than having to inherit from a class provided by some 3rd party jar.

The important drawback, however, is that the Apache Commons solution uses reflection instead of actually generating code to implement equals/hashCode and toString. Depending on how frequently these are used and how performant they need to be this might be a real consideration to use either Project Lombok or to write all the code directly. However, I have not found this to be the case in my project as it never seems to show up as a hot spot in any profiling I have done.