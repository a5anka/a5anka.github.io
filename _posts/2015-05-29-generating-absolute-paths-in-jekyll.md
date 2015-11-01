---
layout: post
title: "Generating absolute paths in Jekyll"
description: "How to generate absolute paths in Jekyll"
tags: [jekyll]
comments: true
share: true
---

To create an absolute path for a resource without hard coding the host-name, simply prepend the path with `{% raw %}{{ site.url }}{% endraw %}` to the relative path.

See the code below for an example.

{% highlight html %}
<figure>
    <a href="{% raw %}{{ site.url }}{% endraw %}/images/tetsImage_a.png">
        <img src="{% raw %}{{ site.url }}{% endraw %}/images/testImage_b.png" alt="">
    </a>
</figure>
{% endhighlight %}
