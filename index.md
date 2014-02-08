---
layout: page
title: Welcome!
tagline: You have just reached my home page. Enjoy your stay... 
---
{% include JB/setup %}

## About

I am Gregory Estrade, a.k.a **Torlus**, a software developer and a self-taught hardware hacker. I especially like reverse-engineering stuff, and use FPGAs to emulate ASICs.

I also like what is called nowadays "retro-computing" or "retro-gaming", so my projects are almost all about emulation of old machines, or making it easier to use obsolete hardware on current technology.

## Projects

They are available on [GitHub](https://github.com/torlus/), but if you fell like there is something missing, please contact me.

Here are my latest projects so far:

- Atari Jaguar in a FPGA (videos [here](http://www.youtube.com/watch?v=l6KWd-LPwKg) and [here](http://www.youtube.com/watch?v=Mk850f7ICVM)). 

- Nec PC-Engine (or TurboGrafx-16) in a FPGA. (videos [here](http://www.youtube.com/watch?v=V0jXQXZHToE) and [here](http://www.youtube.com/watch?v=gVt4fZFnMpw)).

## Blog

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

