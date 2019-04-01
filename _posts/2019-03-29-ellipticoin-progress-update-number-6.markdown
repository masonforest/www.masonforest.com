---
layout: post
title:  "Ellipticoin Progress Update #6"
date:   2019-03-29
categories: blockchain ethereum ellipticoin
---

Last week I went to [Unchained Live in New York
city](https://unchainedpodcast.com/vitalik-buterin-on-whether-or-not-ethereum-is-blowing-it/).
It was interesting to hear Vitalik's thoughts on the current state of Ethereum and Laura
asked tough and thought provoking questions as always. After the show I met [Dan
Robinson](https://twitter.com/danrobinson/) who had released the [rainbow network paper](https://rainbownet.work/RainbowNetwork.pdf) hours before. I explained to
him why I'm building Ellipticoin and not an Ethereum 2.0 implementation. I
decided to write more about that [in a separate post here](/blockchain/ethereum/ellipticoin/2019/03/29/why-im-not-building-an-ethereum-2-node.html). I got to talk to a
lot of of other people about Ellipticoin as well and it got me excited to come home and get back to
work!

Probably the most exciting news this week is that I've got a simple testnet up and
running!

There are three miners running on DigitalOcean servers with a  simple [block
explorer setup here](https://block-explorer.ellipticoin.org/).

The miners are all running a proof of work algorithm I thought up called
hashfactor. Here's [a
gist](https://gist.github.com/masonforest/e674ee749a01391f4ce7a35aa7bbf286)
describing how it works.

I started implementing hashcash in Rust but it seemed more complicated than it
needed to be. Hashfactor functions similar to hashcash although it's simpler to
implement and the the proof of work value is an integer instead of a hash with
leading zeros. It's also simpler to calculate how many hashes are required on
average to produce a valid value.

I also switched Ellipticoin's peer to peer networking layer from
[libp2p](https://libp2p.io/) to [Perlin network's](https://www.perlin.net/)
[noise protocol](https://github.com/perlin-network/noise). I was having trouble
getting libp2p to work properly in tests and couldn't figure out how to set up
kademlia easily. This could be be because the rust implementation is still
young. In any case, Noise seems to be simpler and easier to use a the moment. I
set up noise with kademlia and echd in about [100 lines of go code](https://github.com/ellipticoin/ellipticoin-node/blob/master/native/noise/noise.go). Switching
p2p protocols was fairly painless. Network adapters in ellipticoin define two
methods `broadcast(message)` and `receive(message)`  which do what you'd expect
them to do where `message` is a raw binary message.

I'm following Perlin closely and I'm fairly active on their Discord channel.
They're building a smart contract platform that runs WebAssembly as well
although they're using [Snowflake to Avalanche
consensus](https://ipfs.io/ipfs/QmUy4jh5mGNZvLkjies1RWM4YuvJh5o2FYopNPVYwrRVGV).
The software they've open sourced so far has been very high quality. I plan to
write an implementation of Ellipticoin in Golang at some point and use their
[Life Virtual Machine](https://github.com/perlin-network/life) as well. Their WebAssembly
interface isn't released yet but I've played around with their [smart contract
framework](https://github.com/perlin-network/smart-contract-rs/tree/smart-contract-refactor).
Currently our interfaces are a bit different but my goal is to aim for
interoperability whenever possible. I'd love for Ellipticoin smart contracts to
be run on the Perlin network as well. I believe in the future there will be many
blockchains used for different purposes and tradeoffs. Interoperability allows
users to move to the best platform instead of being stuck using an incumbent
technology.

Finally I added [a roadmap of Ellipticoin's consensus mechanism](https://ellipticoin.readthedocs.io/en/latest/roadmap/).

The next step is to get transaction and smart contract processing working again
and at that point I'll have everything I need to go live!

As always I'd _love_ to hear anyone's feedback on any of this. Swing by the [Telegram chat room](https://t.me/ellipticoin) and say hello!

Until next time,

-Mason
