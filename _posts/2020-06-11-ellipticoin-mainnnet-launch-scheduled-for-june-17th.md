---
layout: post
title:  "Ellipticoin Mainnet to Launch June 17th"
date:   2020-06-11
categories: blockchain ethereum ellipticoin
---

Tom and I have been heads down for the past couple weeks putting the finishing
touches on what we're calling Ellipticoin Prototype Phase I.

In this phase we'll be launching with the minimal set of functionality so we can
get working software into the hands of users as quickly as possible. Users will
be able to trade Ellipticoin with Ether via Uniswap and a community-run token
bridge. Ethereum holders can also unlock Ellipticoins via the "Trade" tab of the
[Ellipticon wallet](https://wallet.ellipticoin.org/). In prototype Phase 1 tokens will unlock at ratio of one
hundred to one. This is to incentivize early adoption similar to how Bitcoin's
halvenings are set up. This phase will be capped at 10,000 ETH/1 million EC.
100,000 Ellipticoin and 25 ETH will be added to the Uniswap liquidity pool on
June 17th putting the initial token price at $0.06 USD per token. Later phases
will have progressively smaller multipliers as we gain traction. Eventually
unlocking will be phased out all together and supply will be capped at 21
million coins. Since burned tokens are recycled back into the system the system
doesn't need to rely on inflation to pay for security. Bitcoin and Ethereum
showed that digital assets and smart contracts are possible but we're confident
Ellipticoin will be significantly more scalable, user friendly an energy
efficient.

Of course there's dozens of other so-called "Ethereum Killers" making those
promises, what makes Ellipticoin different? Most importantly: our funding
mechanism. I’m not aware of any other projects that are bootstrapping without a
pre-mine or founders reward. For a layer 1 solution to be successful the sale of
tokens needs to be open to the public like the Ethereum and Bitcoin token sales were. The fact is that ownership of a
blockchain isn't evenly distributed and it never will be. Power naturally
centralizes but the illusion of ownerlessness has to be maintained. It’s not
possible to maintain that illusion through traditional fundraising mechanism
which all of the other Eth killers are utilizing.

Is this the easiest path? Certainly not but it’s necessary to keep the project
sufficiently decentralized. My plan for profiting off of this personally is
facilitate the community in creating an Ellipticoin DAO which is funded by miner
"taxes". That DAO can then hire me to continue to work on the protocol. Tom
plans to profit off of running a miner. In the last 30 days Ethereum miners have
been paid over $16 million dollars in mining fees. There’s plenty of money to be
made without raising money privately.

Another question we’ve been getting is how we achieve scalability and zero
emissions on consensus. In short: by making trade offs. Other projects will
attempt to hide their trade offs. They’ll  tell you there are no downsides to
their designs. In reality this isn’t ever the case. The first trade off we
decided to to make was to remove the DDOS protections of traditional proof of
work consensus. That allowed us to elect block winners ahead of time which means
they can process transactions in real time. For users this gives the experience
of instant transactions that they’re used to in Web 2.0. Miners will have to
protect themselves from DDOS attacks through traditional means which is a solved
problem. Another tradeoff we are making is we are optimizing for miners to use
cloud instances instead of running hardware themselves. While running your own
hardware does sounds like more fun you can process a lot more transactions per
node with powerful cloud servers. There is the argument that expensive cloud
servers are cost prohibitive to some people but we think this a worthwhile trade
off. Miners on ETH2 will already have to buy $8k worth of ETH at current prices
to run a staking node. Why not have them spend that money on a cloud server
instead? I don’t expect users of the network care whether the network is running
on cloud servers or commodity hardware.

We're also approaching development differently than Eth 2 and other
teams.
[Waterfall development](https://en.wikipedia.org/wiki/Waterfall_model) has been known to lead to missed deadlines and buggy
sofware for years. Instead of writing a specification up front we're buildin
we're using a more modern apprach and building a prototype and iterating based
on user feedback.

Instead of writing our own binary encoding format and networking library we're
using existing standards like [CBOR](https://tools.ietf.org/html/rfc7049) and [HTTP](https://tools.ietf.org/html/rfc2616). This allows us to ship working
sofware quicker than other teams.

We also leverge existing software like [Postgres](https://www.postgresql.org/)
our database and [Rust](https://www.rust-lang.org/) for our smart contract
programming language. Spending less time reimplmenting things from scratch gives
us more time to focus on building the best possible network.

Ethereum does have network effects. This is what we will working around in
Prototype phases II and III. In prototype phase II we’ll be setting up a
decentralized bridge from Ellipticoin to Ethereum using RenVM. In prototype
phase III we’ll be setting up a decentralized bridge from all ERC20 tokens on
Ethereum to Ellipitcoin. Also in this phase we’ll set up a simple automated
market maker similar to Uniswap version 1 so people can trade these tokens. The
first real use case will allow for people to buy sell and trade all ERC20 tokens
instantly. This is not only a much nicer user experience but will solve for the
front running issues seen on Uniswap. Imagine being able to convert your ETH to
Bitcoin instantly and then sending it to someone else who can instantly convert
it to a [Gold sythentic](https://blog.synthetix.io/chainlink-decentralizes-first-wave-of-synthetix-price-feeds/) for storage. That will all be possible in prototype phase III. And
that’s just the beginning! After the prototype phases we plan to enable full smart
contract support and add a DAO that’s funded by miner taxes which will take
ownership of the network.

What set us apart from other chains is that we ship working software. You can
already trade ETH for Ellipticoins via the [Ellipticoin wallet](https://wallet.ellipticoin.org/). As is noted
on the website these balances won’t be persisted and is just available for
testing purposes. On June 17th we’ll flip the switch and kick things off for
real!

Yesterday I was charged $29 in transaction fees and had to wait 12 minutes for
[a single
transaction](https://etherscan.io/tx/0x517572e6a1d23fec12b9c6d2b76d3e617ce504e2b420f5831933555fa3753cd9)
to confirm when adding liquidity to Uniswap. A single Bitcoin transaction
requires the same energy it would take to power 18 US households for a day.
We’re confident we can do better. Join us!

As always you can join us in the Telegram chat room if you’ve got any questions
or comments on any of this! See you there!
