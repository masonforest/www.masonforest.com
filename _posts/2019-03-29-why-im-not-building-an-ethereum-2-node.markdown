---
layout: post
title:  "Why I'm not building an Ethereum 2.0 node"
date:   2019-03-29
categories: blockchain ethereum ellipticoin
---
Last week I went to [Unchained
live](https://unchainedpodcast.com/vitalik-buterin-on-whether-or-not-ethereum-is-blowing-it/)
in New York city. After the show I met [Dan
Robinson](https://twitter.com/danrobinson]) who had released the [rainbow
network white paper](https://rainbownet.work/RainbowNetwork.pdf) just hours
before. I explained to him at a high level of how Ellipticoin works. He didn't
find any immediate holes but one of his first questions was: "why aren't you
building an Ethereum 2.0 node instead?" I didn't have a clear answer for him so
I wanted to write out an answer here incase I'm asked the question again or
incase anyone is interested.

First off, I don't like the flamewars that go on in the crypto community. I
don't want this to be contributing to that. However Vitalik did say in the
interview that he would like Ethereans to criticize each other respectfully so
that's what I'll attempt to do here.

When building software you start out with a set of assumptions. Sometimes the
assumptions are about the real world and some are technical. One of the biggest
technical assumptions being made about Ethereum 2.0 is that sharding will be
feasible to implement in the next 2-5 years. I have opinions about whether
that's the case but more importantly this assumption hasn't been tested. There
is no working software to prove that this is possible. I understand that this is
a hard problem and it will take long time to solve but you should test your
biggest assumptions as soon as possible. Currently we're relying on the word of
Vitalik and others to tell us that these assumptions will turn out to be true. I
have no doubt that those people believe that but doubt that the assumptions are
actually true. Vitalik is a wildly smart guy and his contributions to the space
have been astounding. Still, in building software for the past 15 or so years
I've realized the default is for software projects to go over time and budget or
to fail. I'd be much more confident in the project if I could see working
software instead of having to rely on the word of others.

I see a couple of counter arguments to this. One is that they're building
Ethereum 2.0 in phases. The issue I have with this is that the biggest
assumptions are being tested in the final phases. Phase 0 is basically testing
the assumption of whether we can build a peer to peer network that randomly
selects proposers and validators. I think while not trivial this is possible.
Phase 1 seems harder to implement and Phase 2 harder still. The problem with
this is we won't know whether the whole system is viable until the final phase
is complete. We're still relying on people's word. What I don't want to happen
is for 3 years to pass and us to find that sharding isn't feasible. It doesn't
matter how much we've built if we don't have working software.

There's also the argument that phase 0 and phase 1 are valuable on their own.
That's still an assumption. I can be sure that that's the case when I can run
software on the network. Until then I'm just relying on what someone else said.

Being a software engineer makes you pessimistic about the success of software
projects. That doesn't mean great software can't be built. Most engineers will
agree though that building a piece of working software and iterating on that is
much more likely to be successful than planning a design upfront. That's not how
Ethereum 2.0 is being built.

I'm also worried about the centralization of wealth and power over the network.
Often we think of the technical decentralization but I think it's important to
think about decentralization of power and wealth as well.

If a small group of people hold the vast majority of the tokens in the system it
will be more centralized than if that group is larger. I'm not sure of the exact
distribution of ETH but I know that there are some very large whales in the
space. This may be unavoidable but still, the more centralized a network is the
less interested I am on working on it.

I think it's also important to think about power and control over the node and
mining software. If we have 10000 different miners running software but a small
group of people and entities are controlling how software and network works I
think we have failed. It's no secret that ETH 2.0 is going to be immensely
complex. Complexity has drawbacks but one that I don't think that isn't often
discussed is the centralization that it can cause. Consider a small group of
people make a decision about a network that a minority doesn't like. If the node
implementation of the network is simple the minority can fork the network
easily. If it's complex it will be harder to maintain the codebase and therefore
harder to create a fork that continues to work with all of the existing tools in
the ecosystem.

There's also quite a bit of centralization of power around one company:
Consensys. First off, I respect what Consensys has done for the space. They've
probably done more good for the industry than any other company thus far. That
said, they are a company and therefore they have to make money. It seems
suspicious that they've partnered with [banks, big pharma, fossil fuel
companies](https://entethalliance.org/membership/) and an [oppressive
regime](https://en.wikipedia.org/wiki/Human_rights_in_Dubai). It's possible that
they are doing these things with the best intentions but it I have my doubts. If
they aren't I worry those already in power will gain too much control over the
Ethereum ecosystem. Again, I don't mean to flame on Consensys I'm just stating
the facts. If anyone wants to discuss this more I'd be happy to.

Ethereum 2.0 is promising a lot. If at some point in the next 5 years we're
running an network sharded across thousands of consumer grade laptops which can
run eWASM contracts I'll be thoroughly impressed. Still, I don't have enough
confidence in that vision to devote a serious amount of time to it. In the mean
time I'll continue my work on [Ellipticoin](http://www.ellipticoin.org/).

Again if anyone would like to discuss any of this more I'd love to.

Thanks for reading,

-Mason
