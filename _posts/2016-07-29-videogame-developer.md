---
layout: post
title: "A few days spent in a videogame developer's shoes"
description: "A short experience working as a developer in a videogame studio"
category:
tags: []
---
{% include JB/setup %}

# Preamble

A week go, I've been given the opportunity to work during a few days as a developer for a videogame studio.

The videogame industry is a highly competitive field, so I had to sign a NDA for this special mission. Therefore I won't disclose who I've been working for, and use fake names for the different people involved in this story. About the company itself, all I can say is that it is currently working on a AAA title for PC/Windows, Xbox One and PlayStation 4, that should hit the shelves before the end of this year.

It all started out from a discussion I've had with my old friend *Heckle*, who is in charge of the development team, that talked to me about some performance issues. After some time spent to fix the most time-consuming functions, he described the current status of the code as *uniformly slow*. He already did some extra work to spot some potential issues, and asked whether I could have a look at them.

So basically, I had a few days to see what I could do on a codebase of *1.6 millions* LOC (lines of code) in the **C++** programming language (for the main engine solely), that have been written by a 50+ team of programmers over the last 2 years...

![barney](/images/barney.jpg)

# The offending code

*Heckle* noticed that a set of functions of *numerical interpolation* were used in many parts of the code, but especially by the *particle engine*. He taught me how to use an in-house *profiler* tool, that was able to spot function calls and count them. After a few attempts, we came out with the following result: *these interpolation functions were called more than 10.000 times per frame*.

Being quite convinced that they required our attention, *Heckle* and I started to look at the code of these functions. And this is at this precise moment that we discovered that our double facepalm would be the very first one of many to come...

All these functions were implemented as generic *templated* functions. The templates had no less than 3 parameters (type of return value, type of *curve*, memory allocation indicator). The *curves* were implemented as a (of course), generic, templated class, embedding *lists*... You've already guessed how lists were implemented, don't you?

Ironically, the *list* abstraction (IMO, the only interesting one in this case) fell short, as the sole implementation provided was to store values in an array. And some *public* functions provided, such as "get the pointer to this array" ensured that no other implementation (such as a linked list) could ever see the light of the day without breaking everything.

To sum up what we were dealing with: *meta-programming* using templates, *operator overloading*, *compiler directives* such as *reinterpret_cast* all the way, and a carefully crafted set of *preprocessor macros*, to "improve readability". I was about to puke.

After taking a deep breath, I started to dig into the code...

![cppmp](/images/cppmp.jpg) ![aspirin](/images/aspirin.jpg)

> Choose your weapons wisely...

# Wondering "Why", before wondering "How"

This sentence sums up my strategy, when I'm asked about the reverse-engineering techniques and methodology I'm using.
So, in this case, instead of trying to optimize the code, I wondered "who" and "why" these functions were called.

The top user of these interpolation functions was the *particle engine*, which isn't surprising.

So I imagined how the particle engine might work, and I came up with the following scheme:

- Particles are created on a given event.
- A few rounds of the *physics engine* produce some key *points* stored in a *curve*.
- *Interpolation functions* are then used, as they should be less time-consuming than running the physics engine.

It looked like a reasonable way to manage particles, however I spotted an issue there: *exposing particles to gravity is something very predictable*.

Why bother using Cubic *Bézier curves*, *B-splines* or even *NURBS*, *Catmull-Rom splines* when the path followed by (most) particles is *parabolic* and can easily be calculated straightforward?

Another thing that caught my eye was the *return types* used in these templated functions: *vec3* and *vec4* types were likely to be the most expected, but it wasn't the case here... Most calls used the *float* return type!

After a bit of head-scratching, I asked for an interview with *Jeckle*, the main user of these functions...

# Know your data

*Jeckle* gave me a precious insight on the use cases of these interpolation functions:

- They are called virtually everywhere in the game.
- They have many uses, some of them unrelated to objects and physics, such as *calculating color gradients*.
- For some basic physics, i.e particles falling due to gravity, only the Z-axis would require interpolation, hence the use of *floats* (he got me, on that one).

But what raised my eyebrow was his explanation about the lifecycle of particles: as the interactions in the game became more and more complex, the particle engine had to rely more and more on the physics engine, reducing the number of points that could be "safely" used for interpolation.

That said, I rushed to my keyboard, added some output logs to check the length of the curves used in these interpolation functions, ran a few frames of the game, and my jaw dropped.

**Over 90% of the curves contained one single point**.

One. Single. Point.

![picard-wtf](/images/picard-wtf.jpg)

# Using the Forth

To sum up, the good news was that the amount of useless calculations wasn't an issue. The bad news was of course that we had made no progress so far in the optimization field.

Then, I remembered that the *curves* data structure used at least one *list* (two, when tangents interpolation were required too).

The *list* class contained a *static* part (storing values like length, actual memory use, last index used, etc.) and a *dynamic* part (elements of the list). The dynamic part management is tailored to cope with possibly large lists, which implies the use of complex algorithms dealing with memory management and fragmentation (a.k.a. *allocators*).

Memory allocation is a tough subject, especially in a multiprocessing environment, where processor cores concurrently access the RAM, which is a shared resource.

Then I remembered (don't ask why) an optimization technique used in various *Forth programming language* implementations: the use of a dedicated *Top-of-stack* register. You can read more about it in [Bradford Rodriguez's "Moving Forth" papers](http://www.bradrodriguez.com/papers/moving1.htm).

So I started a new *list* implementation, where the *first element* of the list was stored in the *static* part, instead of the *dynamic* part. Reimplementing all methods wasn't required, as in this case, list operations were straightforward: some calls to *Append*, followed by array access (using an overloaded *operator[]*) and eventually some calls to *Clear*.

# Wrap-up

I didn't manage to test my work, as I encountered some build issues, but I hope that what I've done ends up being of some use.

*Heckle* warned me about many things that could look weird, when you're not used to work in a videogame studio, but I think he exagerrated about them.

First of all, the "code smell" was far below my expectations. The team is made of passionate and talented developers, and the amount of cursed words is suprisingly low for a team made of 95% of young male developers, gathered in an open-space office.

I expected a lack of process and a loose organization, and again, I was wrong. Tooling is impressive, from vendor-provided tools to in-house ones, automatic distributed builds, source code management, tests, everything's all clear.

Communication between teammates was great, with a sense of respect and responsibility that I would have not expected. Meetings are short, kept to a required minimum, there are few levels of management: it's efficiently applied agility, without bells and whistles.

I was delighted by this experience, and I hope someday I'll have the opportunity to do something similar, for a longer period of time.

Special thanks to *Heckle* for this enlightening moment.

> Personal message: this article is dedicated to you, Oriane. All the best.
