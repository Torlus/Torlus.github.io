---
layout: post
title: "Why the Bitcoin blockchain is Einsteinian"
description: "General Relativity in the blockchain area"
category:
tags: []
---
{% include JB/setup %}

# Over the top

As of today, the blockchain is a hot topic. According to many people and institutions, including banks (who were among the first to disregard the Bitcoin phenomenon) it looks like a revolution in both the computer science and the the economics fields.

![over-the-top](/images/over-the-top.jpg)

> Sly, a truck and a giant bird. It lacks unicorns, though.

The 9-pages long [Bitcoin paper](https://bitcoin.org/bitcoin.pdf) (mis)interpretation seems to be an infinite source of "brilliant" ideas and articles (including this one, obviously).

![bullshit-meter](/images/bullshit-meter.jpg)

> "Two things are infinite, the universe and human stupidity, and I am not yet completely sure about the universe." -- (usually attributed to) Albert Einstein

That said, I couldn't resist the urge of raising the bar a little bit, by sharing what my years-long interest and experience in this field finally taught me.

![emmett-brown](/images/emmett-brown.png)

> "And this morning, I was standing on the edge of my toilet hanging a clock, the porcelain was wet, I slipped, hit my head on the sink, and when I came to I had a revelation!" -- Doc Emmett Brown

Let's put an end to this unbearable suspense: **The Bitcoin blockchain is an Einsteinian solution to a Newtonian problem.**

# Distributed ledger issues

Bitcoin, as any financial system, needs a ledger. This distibuted ledger guarantees the transfer of ownership of a given "coin", by the use of cryptograhic signatures from both the current and the future owner.

Fair enough. Now enters the issue of *double-spending*. In a distributed system, one malicious attacker could spread many  transfer-of-ownership transactions issued from the very same coin.

Now let's forget for a while what we already know about Bitcoin and its blockchain. If the ledger was held by a single, central authority, then this issue wouldn't exist. The first transfer-of-ownership would be written into the ledger, and the subsequent ones would be refused.

The idea of **first** and **subsequent** transfer-of-ownership transactions rely on the idea of **time**. Even with Einstein's (and others) works on Relativity, we still live with the idea of time perceived as an absolute, regular, infinite phenomenon.

Our world may be **Einsteinian**, but our perception of it is **Newtonian**.

# Open your book at chapter three

Chapter #3 of [Bitcoin paper](https://bitcoin.org/bitcoin.pdf) is entitled **Timestamp server**.

Its first sentence is:

> The solution we propose begins with a timestamp server.

There comes the idea of [Trusted Timestamping](https://en.wikipedia.org/wiki/Trusted_timestamping). The idea isn't new, and has been successfully used in many fields such as law or finance, in order to give documents a probative value.

However, giving trust to some external entity, being it only for applying a timestamp to a given document, is against Bitcoin's philosophy.

And what is the proposed solution? Yes, you already know it: the (in)famous **proof-of-work**.

The **proof-of-work** is the idea of finding a solution to a given problem of cryptographic computation, that cannot (as of today's knowledge) be solved in a reasonable amount of time by brute force, or even a **predictable amount of time**.

Think about it as a **giant lottery**.

# From Einstein to Newton... wait, what?

Now, let's go further: no matter how much time is spent by computers all around the world to find a solution, the idea of a **continuous flow of time** is dictacted by a chain of "lucky ones" solving the problem.

And as the Bitcoin universe expands (to a limit), the problem to solve increases in difficulty, adapting itself to the whole computing power of its members.

This is where the "magic" resides: for most problems in computer science, the **time** spent to solve them decreases (Moore's law et al.), whereas in Bitcoin's blockchain case, it seems steady...

![coin-supply](/images/coin-supply.png)

> Total Bitcoins over time

And this is where the paradox relies: Bitcoin uses an **Einsteinian, energy-based, spacetime** timestamping scheme to enforce what looks like a **Newtonian, continuous flow of time**.

![snbrightness](/images/snbrightness.jpg)

> This curve is about cosmology, supernovas, etc. I don't know how it is related to the curve above, but they look similar. Math don't lie, so it proves my whole point.

Mind. Blown.
