---
title: "Top 5 Static Analysis Plugins for Eclipse"
datePublished: Wed Jul 01 2009 14:55:57 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeu8gu000502joe8l75d3w
slug: top-5-static-analysis-plugins-for-eclipse-a73d944119af

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753302163090/ef36de64-f5e9-4076-b8d1-0c63b4411f7f.png)

#### Static Analysis

How is it that static analysis is still a best kept secret while so much lips service is paid to code reviews? We have long since understood that boring repetitive jobs should be left for the machines, but when it comes to code inspection there seems to be a mental block. Humans hold the advantage when reviewing architecture, use of design patterns and code organization, but a machine will find when a known null value is dereferenced 100% of the time and that can hardly be said for even the OCD coders I know. Therefore, I present the top five static analysis tools for [Eclipse](http://www.eclipse.org/). While you might not have the delight of working on a project running [continuous integration](http://en.wikipedia.org/wiki/Continuous_Integration) with a full unit test suite, code coverage, and static analysis, you can at least run your own private versions and rest comfortably knowing you’ve done your best.

#### Code Coverage

I assume at the very least that you’ve written some unit tests in [Junit](http://www.junit.org/) or maybe [TestNG](http://testng.org/doc/index.html), the next step is to see if you are actually testing enough of your code. Code coverage has saved me a number of times when I’ve been a little lax in my [TDD](http://en.wikipedia.org/wiki/Test-driven_development) discipline. Having written some code, followed by some tests, its very easy to forget that one code branch and fail to exercise it; [EclEmma](http://www.eclemma.org/) to the rescue. EclEmma installs a Coverage mode right next to the run and debug modes that are standard to Eclipse. Simply run your unit test with the Coverage mode and bang, your code shows up bright green, yellow or red, letting you quickly know where you forgot a test or two.

#### Bytecode Analysis

The next level of analysis comes from the well respected [FindBugs](http://findbugs.sourceforge.net/) tool. After installing the plugin, FindBugs inspects the actual .class files of your compiled code for well know bugs and other suspicious behavior. I’m constantly surprised at the lack of false positives and overall quality of the bugs it finds, from comparing strings with ==, to failing to close resources, and more, its spot on.

#### Code Complexity Analysis

Next in my list of favorites is complexity analysis. These metrics, from simple lines of code in a method to the lesser know but more powerful [Cyclomatic Complexity](http://en.wikipedia.org/wiki/Cyclomatic_complexity) and [Efferent Coupling](http://en.wikipedia.org/wiki/Efferent_Coupling), are the closest thing to a “[stinky code](http://en.wikipedia.org/wiki/Code_smell)” test that software engineering has come up with. If you strive for [loose coupling](http://en.wikipedia.org/wiki/Loose_coupling) and [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) code, then [EclipseMetrics](http://www.stateofflow.com/projects/16/eclipsemetrics) is right up your alley. Once it is installed you will see warnings where your methods are overly long or complex, or your classes are poorly organized. Let the OCD run wild.

#### Dependancy Analysis

Dependancies are one of those things that can go unnoticed until they bite you in the rear. Once [circular dependancies](http://en.wikipedia.org/wiki/Circular_dependency) creep in it can become incredibly hard to break apart and modularize your code. This might not seem important in a small to medium sized project but once a project scales up, the need to break it apart becomes critical and keeping your ducks in a line from the get go makes life that much easier. For this task we employ [JDepend4Eclipse](http://andrei.gmxhome.de/jdepend4eclipse/).

#### Source Code Analysis

Finally we come to [PMD](http://pmd.sourceforge.net/). This is the twin brother of FindBugs. Where FindBugs checks the .class file, PMD checks the .java file. There is a lot of overlap here but you can find some interesting things like unused code that javac might have optimized away in the .class file.