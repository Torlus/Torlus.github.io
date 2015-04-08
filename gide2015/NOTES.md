# Front cover

I'm Gregory Estrade, also known as Torlus on Twitter, GitHub and some forums.
I work for Lyra-Network, a payment processing company, where I am the Research and Development Director.
I'm also a hardware hacker and an embedded software developer in my spare time.

Today's talk is about security concerns for Internet as we know it,
which in the opinion of many will be more and more prone to privacy issues,
especially with all these “things" based on low-end embedded systems connected to the Internet, and to various parts of your body.
Yes, put like that, it sounds a little scary, doesn't it ?


# IoT - State of the art

One of the most popular field of application of the IoT is the Home Automation.
MQTT is a good candidate for this purpose.
As you may already have heard about it this morning, I won't detail how it works.
The important things to know, is that it works on a pub/sub model, and that devices connect to a broker.
The brokers themselves can be chained together, to form a kind of tree, which is fine,
as you can add more and more security the closer you get to the root of your tree.

# Home automation using MQTT

Let's start with a simple user story.
You'll probably want some sensors in your home, for humidity and temperature, as well as some commands.
All the devices will be connected to a single broker connected to the Internet.

> Next

Now it's time to decide what you want to do with the data coming from and to your devices.
For instance, the outside temperature of my house could be available to anyone.
These data may be collected by weather forecast companies, to improve their predictions.
The garage door status would be an interesting information to share with neighbors.
If I'm on vacation a thousand miles away, my neighbor could receive an alert if my garage door is open,
so he could check if everything's OK.
On the other side, this is not the kind of data I want to share publicly, as burglars could use it know my habits.
About commands, they sure need to be protected. Especially the garage door, obviously.

# Home automation using MQTT

Well, such a project is no big deal. MQTT is simple, software tools and libraries are widely available.
To make my little project look good and brag about it, let's purchase a domain or subdomain name.
As well as a certificate, and secure the transport with TLS,
use login/passwords for restricted areas, or even create some client certificates if you want to be pretty serious about security.

> Next

And we're done. Or maybe not. If we were, I guess I won't have had the opportunity of doing such a talk today.

# DNS and TLS concerns

Yeah, our good old friends, DNS and TLS, on which we rely everyday for accessing our mails,
our social networks, our bank account.
Unfortunately, things didn't go as well as expected, especially those last months.
About DNS, its flaws are known for ages. It's a weak protocol, but the main concerns lately came from companies and governments
that abuse their power to perform censorship. We've seen that during the war in Syria, where people were tagging walls
with the 8.8.8.8 address of Google's DNS.
And I'm pretty sad that something that ought to be done only by "bad guys" has arrived in my own country these last weeks.

Next, TLS and the whole PKI.
OpenSSL has had its issues lately with Heartbleed, Poodle, but this is not where the real issue is.
There have been abuses, especially the SuperFish malware supplied in Lenovo's laptops.
Another things, more tricky, take their root in cryptography issues, which are way harder to figure out, for "normal" people like you and me.
I especially recommend reading the latest article about RSA key backdoor.

> Next

Anyway what's more important is that they rely on trust.

# Trust no one

And especially on a Hierarchical Trust model.

# Enter the Blockchain

So, what can we do about it ?
Well, the idea is to look at what crypto-currencies have achieved, and especially Bitcoin, and the concept of Blockchain.

> Next

Explaining *how* it works would be the subject of a talk on its own.
What's more interesting is to understand *why* it works.
It works mostly for the same reason why BitTorrent works.
There may be a few attacks performed by a few people or organizations, but the huge majority of people are silently using it,
and therefore ensure its integrity and security. People do that because they share the same defect, which is *greed*.
Because no one can resist the idea of receiving "free money".
And one more thing, let's imagine you're a "bad guy" with some computational power to spare.
It would probably be more efficient to use it for mining, than to try to corrupt the network.
That's why you can find Bitcoin mining farms everywhere in the world.

Which leads me to some kind of *natural law* which unfortunately is a bit sad to admit.

# TRUST < GREED

Yes, this is not a very nice view of mankind.

# Decentralization matters

Ok, you may wonder how it is related to the IoT.
Well surprisingly, some actors like IBM and Samsung shared some interest on that topic.

> Next

I did myself a proof-of-concept of something similar, or should I say,
IBM and Samsung did something similar to what I've done some months ago.
You can find all the details on my GitHub.

# How does it work?

Well, I must say that 90 percent of the job is done by the Twister platform.
Twister is a decentralized Twitter clone.
The Blockchain is used for handle registration.
Then, the remaining parts of the system belong to BitTorrent family of protocols.
A Distributed Hash Table mechanism like the one used in the Kademlia network, is used for replicated storage of user information.
The post themselves are stored in BitTorrent swarms.
