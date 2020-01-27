---
layout: post
title: "Online Pomodoro Timer"
categories: Productivity 
description: "An Online Pomodoro Timer you can use to practice Pomodoro Technique"
tags: [Productivity, Tool]
comments: true
share: true
date: 2015-11-13T14:13:31+05:30
---

Looking for an online Pomodoro timer is the first thing most of us do
after reading about [Pomodoro technique][1]. I also did the same and
found many Pomodoro Timers online. But I did not like the ones I found
very much. Most of them were Flash based (I am sorry I am not a big
fan of Flash based apps =D), and some Pomodoro Timers were more than
just Timers. I was looking for a minimalist interface.

The Pomodoro Timer created by [Ryhan Hassan][2] was a very good fit
for what I was looking. He had done a very good job in creating a
Pomodoro timer. But it was not using desktop notifications and the
timer was not available online.

Therefore I decided to fork the [his github repo][3] and add the missing
features to suit my requirements. I also hosted the modified tool along
with my blog.

You can access the Pomodoro Timer from
[http://a5anka.github.io/pomodoro/](http://a5anka.github.io/pomodoro/).

## Screenshots

<figure>
	<a href="{{ site.url }}/images/pomodoro_timer_front.png">
        <img src="{{ site.url }}/images/pomodoro_timer_front.png" alt="Pomodoro Timer">
    </a>
    <figcaption>Pomodoro Timer start page with a brief introduction on Pomodoro technique</figcaption>
</figure>

<figure>
	<a href="{{ site.url }}/images/pomodoro_timer_pomodoro_started.png">
        <img src="{{ site.url }}/images/pomodoro_timer_pomodoro_started.png" alt="Pomodoro Timer">
    </a>
    <figcaption>A digital clock and a progress bar is shown when you start a Pomodoro</figcaption>
</figure>

<figure>
	<a href="{{ site.url }}/images/pomodoro_timer_break_started.png">
        <img src="{{ site.url }}/images/pomodoro_timer_break_started.png" alt="Pomodoro Timer">
    </a>
    <figcaption>Timer for the break is automatically started when a Pomodoro is done</figcaption>
</figure>


[1]: http://pomodorotechnique.com/
[2]: https://github.com/ryhan
[3]: https://github.com/ryhan/Pomodoro
