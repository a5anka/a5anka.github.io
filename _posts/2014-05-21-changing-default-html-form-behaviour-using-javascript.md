---
layout: post
title: "Changing  default HTML form behaviour using JavaScript"
description: "Have you ever faced a situation where you wanted to execute a function you want instead of sending the form data when the submit button is clicked. This post describes one way you can do just that."
tags: [javascript]
image:
  feature: abstract-1.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

Have you ever faced a situation where you wanted to execute a function
you want instead of sending the form data when the form submit button is
clicked. This post describes one way you can do just that. This
behavior is very useful in tasks like form data validation. We don't
want to send form data to server if the data validation fails.

## Code snippets 

Assume your form has the following structure,

{% highlight html %}
<form id="my-form">
  Name: <input type="text" name="name"><br />
  Age: <input type="text" name="age"><br />
  <button type="submit">Send</button>
</form>
{% endhighlight %}

You can override the default form submission behavior by using a
JavaScript function like below for the form's onsubmit event. Here the
usage of "e.preventDefault()" function is important to avoid form data
submission.

{% highlight javascript %}
var form = document.getElementById('my-form');

// Send a message when the form is submitted.
form.onsubmit = function(e) {

  //Do something... 

  e.preventDefault();
};
{% endhighlight %}
