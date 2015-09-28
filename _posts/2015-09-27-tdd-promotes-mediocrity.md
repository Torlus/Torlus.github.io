---
layout: post
title: "TDD promotes mediocrity"
description: "TDD"
category:
tags: []
---
{% include JB/setup %}

# assert (obvious == true)

*TDD* does more harm than good. Of course, at a first glance, it doesn't. After all, why having thousands of tests wrapping some piece of code could be a Bad Thing? Because your time is limited, your patience too, and you have much more useful things to spend your time to, like becoming a *better programmer*.

# Writing software is like Writing

That's a comparison many people use to showcase how programming works, and it is not bad at all, so I will use it myself.

Some people state that even good writers are unable to produce a whole paragraph without giving it many attempts, change its sentences, its structure. And so are programmers, that need many attempts to write a piece of code that would be eventually be satisfying at the end.

Fair enough, but let me state something more: becoming a *good writer* starts with being an *avid reader*. At school, you're learning to read before learning to write, right? Why should it be different for software programming?

You have to enhance your ability to program, and that can not be achieved by writing code over and over and again. Sure, you have to practice, but you have to *learn* too. And learning implies that you take a rest, grab a cup of coffee/tea/whatever, and read some articles, books, watch videos, etc.

Easier said than done, as your manager popping at your desk might not be very happy seeing you staring at a screen, instead of hearing the clickety-click of your keyboard.

# Writing tests is boring

> Measuring programming progress by lines of code is like measuring aircraft building progress by weight. -- Bill Gates

Let's face it: as a programmer, you want to write efficient, clean and working code. When it comes to writing tests, well, your motivation is lower. So you end up writing subpar code (I dare you to prove me wrong). Lots of annoying, repetitive code.

Tests should ease up refactoring, but in many cases, many tests become irrelevant afterwards, so you end up spending more time maintaining tests, than maintaining code.

# I am an experienced programmer so I know what I'm doing

> The most effective debugging tool is still careful thought, coupled with judiciously placed print statements. -- Brian Kernighan

Allow me to confess something: I *never* test my own code. *Never*. Why? Because I'm an ace programmer, and I want my colleagues to know it. I know what I'm doing. As a writer who would not want to disclose his drafts, I don't want to release some code that I wouldn't be proud of.

If my code is average, or not as good as I want it to be, then it has no chance to hit some project's repository. Do what you want on your local *git* repository, but push only code you're ultimately satisfied with.

# We cannot be both judge and jury

I'm not stating that *nobody* should test my code. Of course, you, as a user of my code, should.

But you simply cannot write anything but obvious, or biased tests, when you're the one both writing and testing your own code.

So let other people write some tests for your code, be them written before or after your code is released, it is not important. And ultimately, ditch this awfully written test code, and perform code review instead. Team building and confidence in your colleagues will not be ensured by robots running soon-irrelevant tests, but by proving with careful thought that both you and them know what they're doing.

Tests and code don't matter. People do.
  
