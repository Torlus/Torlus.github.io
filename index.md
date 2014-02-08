---
layout: page
title: Welcome!
tagline: You have just reached my home page. Enjoy your stay... 
---
{% include JB/setup %}

## About

I am Gregory Estrade, a.k.a **Torlus**, a software developer and a self-taught hardware hacker. I especially like reverse-engineering stuff, and use FPGAs to emulate ASICs.

I also like what is called nowadays "retro-computing" or "retro-gaming", so my projects are almost all about emulation of old machines, or making it easier to use obsolete hardware on current technology.

## Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

