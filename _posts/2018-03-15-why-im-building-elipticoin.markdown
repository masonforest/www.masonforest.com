---
layout: post
title:  "Why I'm building Elipticoin"
date:   2018-03-15
categories: blockchain ethereum
---

I had my blockchain moment in October 2016. That moment when you realize that
blockchain technology is going to drastically change finance, business,
society—almost every facet of our lives. I bought as much Ether (the currency on
the Ethereum network) as I could afford and booked a flight to Ireland to
compete in a blockchain hackathon. I built [my first decentralized application
or dapp
](http://www.masonforest.com/blockchain/2016/11/21/orion-takes-second-place-at-dublin-blockchain-hackathon.html)
and was hooked. I quit my job. I didn’t have a plan, but I had an exciting idea:
to build an Ethereum node from scratch.

I had gotten into Elixir and was loving its functional elements. Elixir runs on
the [Erlang](https://www.erlang.org/) Virtual Machine. Erlang was built in the
80s by Ericsson, so it’s been seriously battle tested. Today it is used by
WhatsApp, Facebook (for chat), Amazon, T-Mobile, and Motorola, to name a few. It
offers massive scalability with high availability requirements. That sounded
like a perfect fit for an Ethereum node. I created the
[Exthereum](https://github.com/exthereum) repo and got to work.

About a month into it, I got an email from [Ayrat
Badykov](http://www.badykov.com/), who had the same idea and was also building
an Ethereum node in Elixir. To our surprise, we had both named our project
Exthereum! When Ayrat asked if I wanted to join forces, I eagerly accepted. It
worked out great: he’d been working on a [Merkle Patricia
Tree](https://github.com/ethereum/wiki/wiki/Patricia-Tree) implementation, and
I’d been working on implementing the [virtual
machine](https://themerkle.com/what-is-the-ethereum-virtual-machine/).

Ayrat found out that [Geoff Hayes](https://twitter.com/justhhgh) was also
working on the same idea and—surprise!—his project was also called Exthereum.
That made three engineers that had started the same project with the same name
in San Francisco, Moscow, and Martha’s Vineyard. Arat reached out to Geoff, we
merged in Geoff’s work, and we continued as a team.

Geoff was also working on [Compound](https://compound.finance/), a company that
will allow users to borrow and lend value on the blockchain. In August, Geoff
invited me to join their team, and I thought it was a great idea. I was excited
about Exthereum, but thought that building a real-world dapp on top of Ethereum
would give me a better understanding of what that was like as well. I went to
SF, we worked through the fall, and come December we had a working alpha!

Around that same time, the managing director of OmiseGo reached out to us and
said they were exploring the idea of using Exthereum in their Plasma
implementation. OmiseGo is one of the most well respected names in the space so
we were excited to chat with them. When we got on the call they told us that
Exthereum was also being considered for the [Ethereum Foundation Grants
Progam](https://blog.ethereum.org/2018/03/07/announcing-beneficiaries-ethereum-foundation-grants/).
They’d been in contact with Vitalik, and said he supported funding us as well. I
remember looking over my monitor with wide eyes at the other people around the
office, trying to keep a straight face and to stop myself from jumping around
the room. It was at that point I realized it was time for me to scale down my
time with Compound and focus primarily on Exthereum.

I began to plan my next steps. First and foremost, I wanted to get Ethereum off
of Proof of Work. The Ethereum network burns about as much energy as 1.5 million
US homes. This little “experiment” is fine but it’s irresponsible to be burning
that amount of energy in the long term. In using the network, we are directly
responsible for that waste. Fortunately, there is a potential solution that uses
very little energy: proof of stake. The Ethereum Foundation and others have done
great research in the space, and the plan is to eventually switch Ethereum from
proof of work to proof of stake.

The other major goal I had was to switch from the Ethereum Virtual Machine to
the WebAssembly Virtual Machine. Building the Compound alpha for the previous
few months had been more challenging than I had expected it to be. Blockchain
development is so hard because the tools are very new; the tools are new because
they have to work with the Ethereum Virtual Machine, which has only been around
for a couple years.

But programming for systems with limited resources at runtime is nothing new. If
we weren’t limited to Ethereum-specific tools and languages, we could build on
all the work that’s already been done and leverage existing communities, instead
of having to write everything from scratch. WebAssembly can be compiled by LLVM,
which means you could write smart contracts in C, C++, Kotlin, Rust, and many
others. This would also open up the tooling and communities of all of those
languages. Imagine how much easier this would make the experience of building
dapps.

Exthereum is about 80% complete. Geoff has devp2p hooked up, which can connect
to Geth and Parity nodes. I have a large majority of the Virtual machine tests
passing. Arat has a fully functioning Merkle Patricia Tree and RPL encoding and
decoding working as well. The remaining things to complete are:


  * Get ethhash the Ethereum Proof of Work algorithm hooked up (this hasn’t been
started)
  * Find and fix the remaining bugs in the Virtual Machine
  * Find and fix the remaining bugs in devp2p

Doing all of this will take a ton of time. As anyone in software
development knows, having something 80% done by no means means that you’re 80%
done in terms of time. Also, most of the work left to be done will eventually be
scrapped anyway. Once proof of stake is implemented, ethash will be obsolete,
and once the EVM is replaced with the WebAssembly VM, the EVM will be, too.
Devp2p is also going under a considerable refactor.

In late December I pulled my car into the Martha’s Vineyard Public Library
parking lot. I was staying at my parents house for the holidays and they don’t
have high speed internet so when I take video conference calls I have to take
them there. It was cold. When I opened my laptop I could see my breath on the
monitor.  I tried to get comfortable in the passenger seat of my 1990 blue Jeep
Cherokee. Geoff and I had a call scheduled with Wendell Davis from OmiseGo about
the Ethereum Foundation Grant. We were going to hear if this grant was a dinky
little nothing or something real. We all introduced ourselves and chatted for a
bit. Wendell is a key player in the crypto space. He was on the original email
list that Satoshi Nakamoto published his white paper to in 2008. Eventually,
Geoff asked outright. 

> “So how much are we talking about here?”

> ..... 

> “Pretty much anything you guys need”, Wendell told us.


Again it was hard to hold a
straight face 

> “If you need to hire 5 engineers for 2 years we can work with you
on that” he said.

That’s vague but definitely not a dinky little nothing. We
chatted for a bit more and then hung up. I called Geoff directly after that call
to recap. I told him that I was having second thoughts and considering other
options. He urged me to take the deal but said he supported whatever I decided
to do.

If we had taken the money from the Ethereum Dev Grants program we’d have a nice chunk
of money in the bank and probably have been able to get Exthereum to feature
parity with Parity and Geth. Positioning Exthereum as a first class client would
have opened a huge amount of doors for us many of which would be financially
lucrative as well. There’s the potential to make obscene amounts of money in the
blockchain space right now but there’s also something much more lucrative: The
potential to change the world.

I’d been thinking a lot about Exthereum and had realized it was actually going
to be more work finish it than it would to build my own blockchain from scratch.
There was also a massive potential to make a more sustainable, more developer
friendly blockchain. So that’s what I decided to do.

####  Introducing Elipticoin



The two questions most people ask when they hear about a blockchain project are
“where is the white paper?” and “will there be a token sale?” The answers are:
“there isn’t one” and “maybe.”

I always thought the the idea of a white paper had suspicious parallels to
religious scripture. I worry that when people believe in the idea behind a white
paper they become attached to it. And when they become attached to it they adopt
it as part of their identity. (Paul Graham wrote a great piece about [keeping
your identity small](http://www.paulgraham.com/identity.html).) I worry that this attachment will lead people to resist
change. Granted, some things need to to stay constant. Just like Bitcoin, there
will only ever be 21 million Elipticoins. But, like with all software,
adaptability is paramount. It’s better to build software that is adaptable than
to try to build something that’s perfect from the start. 

Time is also a factor. I’d much rather spend my time blogging about how
Elitpicoin works than setting up a LaTeX editor and figuring out how to center
my name of the cover page. I’ve written a brief overview of how the system will
work [here](https://github.com/elipticoin/elipticoin) and I plan to write more
as Elipticoin develops. 


As far as raising money goes: when building the networks that will control the
flow of value throughout the world, it’s important that we think about what it
is we consider valuable and how that value flows. Currently, the majority of
excess value pools at the top. What’s more important: Buying fancy cars for
already privileged, disproportionately white males, or buying food, shelter, and
medicine for thousands of people who can’t afford it? I’m a greedy human just
like everyone else, but our current value system just doesn’t make sense.
Engrained value systems won't change on their own but they _can_ change if
enough people want them to. With this in
mind, if I do do a token sale, I will donate 90% of the proceeds to [the
ACLU](https://www.aclu.org/), [Planned
Parenthood](https://www.plannedparenthood.org/) and [The International Rescue
Committee](https://www.rescue.org/). With that said, I don’t plan on raising
money until at least a year from now. I don’t want to raise money until
Elipticoin is fully functional. At a certain point money can be more of a
hindrance than an asset.

Finally, Bob Summerwill gave a great
[talk](https://www.youtube.com/watch?v=Yu2ReJUXIgc#t=15m25s) about ending the
tribalism in the blockchain community at EthCC recently. I know this post will
ruffle feathers and I’ll be attacked for posting it. People will tell me why
Elipticoin is a bad idea and why I’m going to fail. That’s just the way the
internet works these days.

I know building a blockchain from scratch is a moonshot and chances of success
are small. The technical part is the fun stuff, and I’ve had a blast working on
Elipticoin for the past couple months. I’m not so much looking forward to the
politics of it, but the promise that the blockchain holds is worth it for me. I
hope that I can remain as cordial as possible with those who disagree with me
and that we can work together whenever possible. If anyone is interested in
helping out, I look forward to hearing from you—there’s always plenty to do!

Also, I’ll be at the [MIT Bitcoin Expo](http://mitbitcoinexpo.org/) Friday
through Sunday. I’d love to chat if anyone’s around!

Looking forward to seeing where this goes!

-M
