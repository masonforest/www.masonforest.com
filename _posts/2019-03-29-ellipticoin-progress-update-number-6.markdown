---
layout: post
title:  "Ellipticoin Progress Update #6"
date:   2019-03-29
categories: blockchain ethereum ellipticoin
---

There's a lot to report this issue!


### Testnet Live
Probably most exciting: I’ve got a simple testnet up and running!

I’ve got three separate miners running on DigitalOcean and simple [block explorer setup here](https://block-explorer.ellipticoin.org/).

### New proof of work algorithm
The miners are all running a proof of work algorithm called hashfactor. This is probably the next most exciting piece of news: I created a new simpler proof of work algorithm called hashfactor! Here’s [a gist](https://gist.github.com/masonforest/e674ee749a01391f4ce7a35aa7bbf286) describing how it works.

It functions similar to hashcash although the result is an integer instead of a hash with leading zeros.

It was much easier to implement than hashcash and it’s simpler to calculate how many hashes are required on average to get a valid proof of work value.

### Switch from libp2p to Noise
I switched Ellipticoin's peer to peer networking layer from [libp2p](https://libp2p.io/) to [Perlin
network's](https://www.perlin.net/) [noise
protocol](https://github.com/perlin-network/noise). I was having trouble getting
libp2p to work properly in tests and couldn’t figure out how to set up kademlia.
This could be be because the rust implementation is still young. In any case,
Noise seems so be simpler and easier to use a the moment. I could set up and
noise with kademlia and echd in about 100 lines of go code. Switching p2p
protocols was fairly painless. Network adapters in ellipticoin define two
methods `broadcast(message)` and `receive(message)` where message is a binary
message which do what you’d expect them to do. If in the future libp2p becomes
easier to interface with I could switch back fairly easily as well.


### Unchained live trip
Last week I went to [Unchained Live in New York city](https://unchainedpodcast.com/vitalik-buterin-on-whether-or-not-ethereum-is-blowing-it/). Laura asked tough and interesting questions as always. After the show I met Dan Robinson who had released the rainbow network paper hours before. I explained to him why I’m building Ellipticoin and not and Ethereum 2.0 implementation. I decided to write more about that [in a separate post here](). 

