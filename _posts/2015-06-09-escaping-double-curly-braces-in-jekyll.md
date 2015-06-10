---
layout: post
title: "Escaping double curly braces in Jekyll"
description: "How to escape double curly braces in Jekyll"
tags: [jekyll]
image:
  feature: abstract-4.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: 
share: 
---
Double Curly braces are reserved characters in the Jekyll world. Double curly braces can be used to refer to internal variables and functions. Therefore we have to escape them if we want to have them as texts. The common use case is when you want to show some Jekyll examples like in [this post][1]. You can achieve this by using the "raw tag" available in Jekyll.

See the code below for an example.

{% highlight ruby %}
{{ "{% raw  " }}%} {{ "{{ some.var "}} }} {{ "{% endraw "}} %}
{% endhighlight %}

An alternative method is to use double quotes to make part of it a string literal.

{% highlight ruby %}
{% raw  %} {{ "{{ some.var "}} }} {% endraw %}
{% endhighlight %}

Hope this helps you in writing your next Jekyll posts.

[1]: {{ site.url }}/generating-absolute-paths-in-jekyll/
