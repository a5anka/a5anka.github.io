---
layout: post
title: "Adding a Maven dependency to an Eclipse project"
description: "Managing dependencies is one of the important task in developing a complex Java project. This normally includes searching for depending libraries and adding them to the Eclipse project."
tags: [java,eclipse,maven]
image:
  feature: feature-mvn_eclipse_basics.png
comments: true
share: true
---

Managing dependencies is one of the important task in developing a
complex Java project. This normally includes searching for depending
libraries and adding them to the Eclipse project. Sometime this is not
a very pleasant task. But the good news is, Maven make it easy to
handle dependencies in Java projects.

I wrote an article on [creating an Eclipse project using Maven]({% post_url 2014-05-14-creating-an-eclipse-project-using-maven %}) previously. This article is a follow-up on that.

## Searching for dependency

Imagine that you want to use a particular Java class in you project
and you want to find and include the corresponding Java library in
your project. You can use [http://search.maven.org](http://search.maven.org) to search for a
Maven dependency. They have an
[advanced search](http://search.maven.org/#advancedsearch|gav) which
is very helpful if you only know the name of the required class.

## Adding the dependency to Maven

From the results you get from the search identify the required
library. By clicking on a version number in the search results you can
get the dependency information for a artifact. Following is an example
of Maven dependency snippet.

{% highlight xml %}
<dependencies>
  <dependency>
    <groupId>org.apache.synapse</groupId>
    <artifactId>synapse-core</artifactId>
    <version>2.1.0</version>
  </dependency>
  <!-- Add more dependencies if required -->
</dependencies>
{% endhighlight %}

## Updating the eclipse project

Add this dependency information to pom.xml (I assume you already have
and pom.xml in your project directory. If not add a pom.xml or read
[this post]({% post_url 2014-05-14-creating-an-eclipse-project-using-maven %})
to know about creating an Eclipse project using Maven) and run following command to
update the eclipse project.

{% highlight bash %}
mvn eclipse:eclipse
{% endhighlight %}

Alternatively you can also use an Eclipse plugin to handle Maven
dependencies. An example for such a plugin is the
[m2eclipse plugin](https://www.eclipse.org/m2e/).
