---
layout: post
title: "Creating an Eclipse project using Maven"
description: "Maven is a build automation tool used in Java projects. Eclipse is a popular IDE used in developing Java projects. Therefore it makes Java development very productive if we can combine these two tools."
modified: 2014-05-14 10:21:22 +0530
tags: [java,maven,eclipse]
image:
  feature: feature-mvn_eclipse_basics.png
comments: true
share: true
---

Maven is a build automation tool used in Java projects and excellent
in handling dependencies between Java projects. Eclipse is a popular
IDE used in developing Java projects. Therefore it makes Java
development very productive if we can combine these two tools. How
hard can it be? Well, It turns out to be a very simple task. You will
also not need any extra tools to get this working. Only having Eclipse IDE
and Maven installed in the system will be enough.

## Creating the Maven project

First go to the directory where you want you Java project to reside.
Normally this will be the Eclipse workspace directory.

{% highlight bash %}
$ cd /path/to/workspace
{% endhighlight %}

Then issue following Maven command to initiate the project creation
process.

{% highlight bash %}
$ mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart
{% endhighlight %}

Maven will start creating the project using interactive mode. Enter
the information it asks in project creation process. Following is a
small explanation of the things asked.

* **groupid** - will help to identify the project uniquely. You can
  enter a valid package name for this.
* **artifactid** - will be the name of the jar without version.
* **version** - will be the version number of the jar.
* **package** - package name for the project. You can enter the same
  value you entered for groupid.

After this step you will have your java project created under a
directory named with the given artifactid value.

## Importing to Eclipse IDE

Go to newly created project directory and run the following command to
generate eclipse project meta files.

{% highlight bash %}
$ mvn eclipse:eclipse
{% endhighlight %}

Now you can import the project in to Eclipse IDE. Use "Existing
Projects into Workspace" option in import project dialgo box. Have a look at
[this article](http://www.mkyong.com/maven/how-to-convert-maven-java-project-to-support-eclipse-ide/)
for more details.

That's all and you can start developing your awesome Java project in
Eclipse.

## Resources
1. [Maven - Maven in 5 Minutes](http://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)
2. [How to convert Maven based Java Project to support Eclipse IDE](http://www.mkyong.com/maven/how-to-convert-maven-java-project-to-support-eclipse-ide/)
