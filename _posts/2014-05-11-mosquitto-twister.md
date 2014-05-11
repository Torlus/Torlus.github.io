---
layout: post
title: "Announcing Mosquitto-Twister"
description: ""
category:
tags: []
---
{% include JB/setup %}
# About

In my previous post, I expressed some concerns about how the *Internet of Things*
will organize itself. My opinion is that its distributed nature should be enforced
from the start.

Distributed systems are harder to grasp than centralized ones, so I wanted to
showcase the use of a fully decentralized infrastructure to make all these
so-called *Things* communicate together.

# All we need is will

During my search for technologies to use, I ended up with the conclusion that
everything is lying there, open-sourced, ready to use and modify to fit our needs.

That is not a surprise per se, but discovering the [Twister](http://twister.net.co/)
project reduced my estimated coding time of a few weeks or months to a mere two days.

The main reason is that *Twister* combines *Bitcoin* and *Bittorrent* technologies,
and also provides a model for microblogging, that fits nicely with the publish/subscribe
model of [MQTT](http://mqtt.org/).

# The project

It's available at [this GitHub respository](https://github.com/Torlus/mosquitto-twister).

And yes, it's (currently) a single Python script of 200+ lines.

Sorry for the code style and quality, my experience with Python
is minimal, so any advice and contribution is welcome.
Setting what is required to run a full test is a bit tedious, though.

Hope you'll like it anyway.
