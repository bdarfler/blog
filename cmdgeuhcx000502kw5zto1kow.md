---
title: "Pimpin’ your Ant build with Ivy"
datePublished: Tue Nov 11 2008 22:42:46 GMT+0000 (Coordinated Universal Time)
cuid: cmdgeuhcx000502kw5zto1kow
slug: pimpin-your-ant-build-with-ivy-abebcbe95299

---

As of late [Maven](http://maven.apache.org/ "Maven") has been making headway in the Java build world. However, there are some who are either in the position of working with an existing system build with Ant or who don’t want to get locked into the Maven file structure. For those folks there is [Ivy](http://ant.apache.org/ivy/ "Ivy") to the rescue.

Ivy is an “agile dependancy manager” which recently released 2.0.0-rc1. Thought jam packed with lovely features the first one that I have put to use is the ability to manage 3rd party dependancies, automagically downloading them, dealing with version conflicts and ensuring that any dependancies they in turn have are dealt with.

Ivy allows for two configuration files, one for describing where to look for jars and one to describe what jars to look for. Beyond that, the rest of the work is done within your ant build.xml file. Lets start with the ivysettings.xml file which describes where to look for jars.

%[https://gist.github.com/c5524d7e565ec82e03687bfe4c425a90]

There are a few things to point out here. First is the resolvers tag. There are a few different type of resolvers and I have chosen to use the chain resolver here. If you are lucky, everything you need with be in ibiblio and all you would need would be the middle ibiblio tag. However, if you are using any jars that are not mainstream you will have to end up specifying the url here. The url tag I’m using here is to deal with the unfortunate issue of jboss not having the latest javax.jms jar up on ibiblio. Here we specify the url pattern to down load the jar directly as long as the url points at a valid maven style repo. Notice you can insert the module revision and organization as parameters. These are passed in from the ivy.xml file which we will discuss next. Also note that in this case there is already a javax.jms jar in ibiblio, just not the right version so we put the url tag first. Additionally, you might have to deal with some jars that are proprietary or not in a valid maven style repo. In this case you will have to download these using ant (show later) or store them in source control. However you can still leverage Ivy by having it pick them up from the local filesystem.

Now onto the ivy.xml file.

%[https://gist.github.com/73333835f6782afc2a7f10d4072629e7]

Lets start with the dependencies first. Each dependancy has an org, name, rev and an optional conf. The org, name and rev correspond to the organization, module, and revision in the ivysettings.xml as you might assume. For your common jars you can find the necessary values by looking up the package at [mvnrepository.com](http://www.mvnrepository.com/ "Mvn Repository") of course they call org group, module artifact, and revision version, just to keep you on your toes. Additionally you should notice the defaultconfmapping=”\*->default”, if you forget this things blow up quick and in weird ways. The dependancy thing is pretty sweet but the real power comes when you start in on the configurations. As you can see I have defined six configurations that correspond to different sets of jars that I need to reference in my build.xml. Configurations can inherit from eachother using the extends so junit is a superset of build, runtime, and the junit specific jars. Assigning jars to configurations is as easy as listing the right configurations in the conf attribute of the dependancy tag.

Next we tie this all together in the build.xml.

%[https://gist.github.com/3b1125c5127c6cb473134c9e745c101b]

To start off we have the ivy target which takes care of a few things. First it depends on dl-ivy for bootstrap downloading of ivy. Once Ivy is downloaded it sets up the ivy path, defines the ivy ant tasks, and reads the settings file. At this point all the magic happens with the <ivy:retrieve /> tag. This goes out, reads the ivy.xml and grabs all the necessary jars. Once that is done we can create ant paths for all the different configurations we defined in ivy.xml. This will become super useful later on. Another thing to notice here is downloading of findbugs. This is done for two reasons. First this is a good example of downloading 3rd party libs using ant that are not in a maven style repo. This target goes out and gets findbugs, unzips it and then ivy can use it through the local filesystem resolver in the ivysettings.xml. The other reason it is here is that the default way of downloading findbugs through the maven style repo conflicts horribly with the findbugs ant plugin. The plugin expects the files in a very specific layout which is broken when you download from maven vs downloading it from their site. I use this idiom for downloading some other picky 3rd party jars as well as some binaries that are useful for development but are not actually needed for java.

Finally we get to see the power of these configurations.

%[https://gist.github.com/6bb17996de1c09a32b3bcf988956c56b]

All we have to do is add the build.path reference and away we go. We can do the same for findbugs, mysql, junit, adding runtime dependancies into a jar that we create in our build and so on. Need to add a new jar? Look it up in the maven repo, add it to ivy.xml and define it as a build, runtime, junit, etc dependancy and away you go. Pretty slick!