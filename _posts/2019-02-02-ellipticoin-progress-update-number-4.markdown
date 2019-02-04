---
layout: post
title:  "Ellipticoin Progress Update #4"
date:   2019-02-04
categories: blockchain ethereum ellipticoin
---
A lot has happened in the past few weeks!

Last Monday I attended [Grincon](https://grincon.us/). Grin is a privacy coin
that's an implementation of the
[MimbleWimble](https://www.mycryptopedia.com/mimblewimble-explained/) protocol.
It's got one minute block times and didn't have a premine or founders reward. I
really respect that and I think it aligns incentives and sets the project up for
long term success. When discussing the roadmap they made a point to point out
the things they don't know or weren't sure about. They could do this because
they didn't have as much financial incentive to inflate expectations. They even
went so far as to make the block reward at constant 60 GRIN forever instead of
giving Grin a capped the supply like Bitcoin has. This gives less of an incentive
to early adopters to hold coins but favors long term adoption instead. This I'm
less sure about. Currently their lead and seemly only full time developer is
being funded though donations which they asked multiple times for at the
conference. I respect what they're doing but I'm not sure if it'll be
sustainable. I do really value working software and real world adoption though
and the tech is definitely the coolest thing I've seen since Ethereum. But, I think
there is some middle ground between telling a complete lie and raising hundreds
of millions of dollars vs being 100% honest and having to rely solely on
donations to fund development.  Regardless I love experimentation. Try it out!
If it works then it works. If not go back and try again.

I saw some people I knew at the conference and met a ton more really smart
interesting people as well. It was nice to hear positive validation of what I'm
working on: "Oh and if it doesn't work out the things you'll learn will be
valuable at your next job." is something one of the people there said to me.
Usually when I tell people I'm working on building my own blockchain they look
at me like I'm trying to build my own rocket ship to go to the moon. Another
quote that stuck with me regarding the current state of the world: "You know how
people say the day is darkest before the dawn? The reverse it true as well." We
laughed and processed to stuff our faces with the finger food piled on the table
in the lobby.

Hearing about all these cool technologies like MimbleWimble and
[CukooCycle](https://github.com/tromp/cuckoo/blob/master/img/cuckatoo_cycle.jpg)
got me thinking a lot about different ways in which Ellipticoin could be set up.
One idea I had was to create a hybrid proof of work/proof of burn version of
hashcash. This could work by having miners burn some coins and then after some
amount of time (24 hours say) they could “mine” those burned values. You'd do
this in the same way hashcash works in Bitcoin where you concatenate the block
hash with a nonce but instead you concatenate the block hash, the nonce,
and the burned transaction. This then makes mining much more expensive
because the when you're mining you have to pay for the for the energy to run
your mining rig but also you have to pay for the currency you're burning. You
could then put constant multipliers on the amount of energy burned vs the amount
of currency burned as desired. Burning currency is better for the environment
and it also levels the playing field a bit because you don't favor people who
have access to cheaper energy. It's hard to bootstrap a currency off of proof of
burn though because you have a chicken or the egg problem.  What made me think
of this is that Grin is using one mining algorithm (Cuckatoo31+) and will
migrate over to another one (Cuckaroo29) over time using sliding constants. That
sounded like a great idea. Using a similar sliding algorithm you could bootstrap
the network with proof of work and slowly transition to proof of burn.  I could
also use their idea of no premine and no founder reward and launch this network
without having to do a token sale! If I put a fixed cap on supply of the
currency I think that would be enough to incentivize people to mine it. Another
more envelope pushing idea I had was to have the burned tokens be deposited into
a DAO on the platform. There could then be some sort of on-chain governance
structure that decided where to spend those funds such as on development of the
protocol itself.

I'm still thinking the first iteration of Ellitpcoin will be a scaling solution
on top of Ethereum because that's where it has the potential to get the most
usage. It will, however, be possible to use most of the components of that
system to build a standalone blockchain, like the one I described above, as
well. I'm also a big believer in the [Stellar Consensus
Protocol](https://www.stellar.org/papers/stellar-consensus-protocol.pdf) which
could be used to build yet another network. I could use the same components to
build a Polkadot parachain. I could build Proof of Authority networks. Each of
these networks would require a way to gossip blockchain data, a way to
communicate with and process smart contracts and api for users to interact
with the system. I want to build the simplest implementation of those components
and iterate from there. If there's one thing I've learned about software
development in the past 15 or so years it's that iterating on working software
leads to better results than planning how the software will work upfront. Our
assumptions about the world are very often incorrect. It's not until we try
things that we can tell if they will really work or not in reality.


One such component that needs to be built is a way for users to interact with
the Virtual Machine. Last week I released a functioning version of exactly that:
[WasmRPC](https://github.com/wasm-rpc/wasm-rpc-spec)! Here's the updated
[Ellipticoin Base Token
contract](https://github.com/ellipticoin/ellipticoin-base-contracts/blob/master/base_token/src/base_token.rs)
which now implements a WasmRPC interface. Since WebAssembly only has integer and
float types I needed a way to represent more complex type like strings and
maps/objects in binary format so I could pass complex arguments to a
WebAssembly program and have complex values returned from it. Instead of writing
my own standard for the binary format I used the existing standard
[CBOR](http://cbor.io/). Basically WasmRPC says data should be communicated over
CBOR and describes how errors should be handled and provides some optional
`imports` as well.

It's a similar to the [CommonWA](https://github.com/CommonWA/cwa-spec) standard
which the [Perlin network](https://www.perlin.net/) uses. I'm really excited for
the Perlin network launch which is scheduled for late spring/early summer. I
feel like they have very similar goals to Ellipticoin and they've been coming
out with some great open source software like
[life](https://github.com/perlin-network/life) and
[noise](https://github.com/perlin-network/noise). I discovered CommonWA after
putting together WasmRPC. Each spec shares some functionality but there are
different functionalities as well. CommonWa defines all resources as URLs for
example. This might not make sense for Ellipticoin since Ellipticoin won't
natively be interacting with external resources. I would love to find common
functionality and come to agreement  on certain parts of the spec though
(potentially around error codes for example).

Standards like these will be good for users of the platforms because they will
allow them to run their smart contracts on multiple platforms with minimal or no
modifications. I value freedom and I think it will be important in the future to
be able to move between blockchains so you can us the one that best aligns with
your values and best fits your needs.

--

The day after Grincon I went back to the city and worked out of the
[Compound](https://compound.finance/) office for the day. It was great to catch
up and hear about all they've accomplished over the past few months. They've
already got millions of dollars worth of value flowing through their protocol
and it's one of the most used Dapps in the world.

It was also great to get feedback on Ellipticoin from Geoff and the team since
they are so deeply involved the space.
--
These next few weeks I plan to get transaction processing working and get a
testnet up again.

As always I'm always excited to chat more about all of this. If you have any
feedback or would like chat more drop in the [Telegram
channel](https://t.me/ellipticoin) and say hello!

Until next time,

-M
