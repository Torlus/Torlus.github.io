---
layout: page
title: Welcome to Gregory's lair!
tagline: You have just reached my home page. Enjoy your stay... 
---
{% include JB/setup %}

## About

I am Gregory Estrade, a.k.a **Torlus**, a software developer and a self-taught hardware hacker. I especially like reverse-engineering stuff, and use *FPGAs* to emulate ASICs.

I also like what is called nowadays "retro-computing" or "retro-gaming", so my projects are almost all about *emulation* of old machines, or making it easier to use obsolete hardware on current technology.

## Projects

They are available on [GitHub](https://github.com/torlus/), but if you fell like there is something missing, please contact me.

Here are my latest projects so far:

- *Raspberry Pi* (partial) support in *QEMU*. Please check my [QEMU fork](https://github.com/Torlus/qemu).

- *Atari Jaguar* in a FPGA (videos [here](http://www.youtube.com/watch?v=l6KWd-LPwKg) and [here](http://www.youtube.com/watch?v=Mk850f7ICVM)). 

- *Nec PC-Engine* (or TurboGrafx-16) in a FPGA (videos [here](http://www.youtube.com/watch?v=V0jXQXZHToE) and [here](http://www.youtube.com/watch?v=gVt4fZFnMpw)).

- *SEGA Megadrive* (or Genesis) in a FPGA (video [here](http://www.youtube.com/watch?v=ilzKiW21T9Y)).

I am also hosting a [blog](http://torlus.com/floppy/) and [forum](http://torlus.com/floppy/forum) for the [HxC Floppy Drive Emulator](http://hxc2001.free.fr/floppy_drive_emulator/index.html), which is an amazing project from a friend of mine.

## Blog

I am not really a blogger. However, I will try to keep this part updated.

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

