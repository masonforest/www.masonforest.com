---
layout: post
title:  "Ellipticoin Progress Update #1"
date:   2018-10-21
categories: blockchain ethereum ellipticoin
---
_Progress  on [mana](https://github.com/poanetwork/mana) was good last week week. German took over working on getting mainnnet to sync so I've had more time to work on Ellipticoin. Here are some updates and thoughts:_


Overall Direction
=============

Since I've written last [new estimates](https://www.reddit.com/r/ethereum/comments/9p70pz/ethereum_20_timeline/) have come out on on when ETH 2.0 will be released. Most people agree that it's going to take longer than a year before the network will go live. Being in software I expect it will take significantly longer than that. Software estimation [is hard](https://robots.thoughtbot.com/why-fixed-bids-are-bad-for-clients-too) and [it always has been](https://www.amazon.com/Mythical-Man-Month-Software-Engineering-Anniversary/dp/0201835959).

Originally I was planning to build a network that was a competitor to Ethereum.
Now I want to build an interim solution for developers who want to continue to
develop Dapps with improved developer and user experiences but don't want to wait
until Eth 2.0 is complete. Then once Ethereum 2.0 is released, since both
Ellipticoin and Ethereum 2.0 run the WebAssembly VM, people can migrate back to
Ethereum if they like or continue to use Ellipticoin. That's the power of
standards and interoperability!

I've been working with [POA Network](https://poa.network/) on the [mana](https://github.com/poanetwork/mana) Ethereum client for the last five months and I've learned a ton about how Ethereum works.

There are some trade-offs and optimizations that can be made to ETH 1.0 to make
a network that scales without having to solve hard technical problems such as
sharding and proof of stake.

I think I can build Ellipticoin faster than everyone else can build Ethereum 2.0 because I'm not actually building much new software. Like Ethereum 2.0 I'm
going to use an existing [WebAssembly](https://github.com/paritytech/wasmi)
interpreter. For the consensus mechanism I’m going to use [Proof of
Burn](http://www.masonforest.com/blockchain/ethereum/ellipticoin/2018/05/29/the-ellipticoin-proof-of-burn-algorithm.html).
This should be much easier to implement than Proof of Stake because the
economics are almost identical Proof of Work’s. I also don't plan to implement
sharding. It's mostly just putting the pieces together.


The goal, which I think is achievable, is to build a blockchain similar to Ethereum with the following traits:

* Runs the WebAssembly VM. This will allow Dapp developers to build smart contracts in fully featured languages like Rust. This comes with the added benefit of the rich Rust community and ecosystem.

* Processes 150 transactions or more per second. Ideally I think we can do a lot better than that but even a ~10x increase would be a significant improvement.

* 5 second block time. Ideally it’d get down to 3 but this will take some real world testing to figure out what’s possible.

* A bridge that uses cross chain atomic swaps so you can move tokens back and forth between Ethereum and Ellipticoin

* A stable coin (DAI) as the base currency


My goal is to build a system where you can move 100 DAI over to the Ellipticoin network, transfer it back and forth 100 times and then move it back in under a minute. Also I want to be able to deploy a simple smart contract to the network written in Rust.

It’s taken a team of 3-5 about 6 months to build a ETH 1.0 client that can successfully process the ETH 1.0 blocks (more news on this coming from POA soon!). Most of that time was actually spent reverse engineering the VM and figuring out edge cases in tests.

I’m confident I can launch Ellipticoin before ETH 2.0 is live. 


Technical Update
================

* Updated how WebAssembly code is run

Before transactions were run through an [Elixir nif](http://erlang.org/doc/tutorial/nif.html). This worked but it meant each transaction that was processed had the overhead of calling through to that nif function. The way it works now is transactions are pushed by Elixir onto a queue in Redis. The [transaction processor](https://github.com/ellipticoin/ellipticoin-blacksmith-node/blob/master/native/transaction_processor/src/main.rs#L10) then runs the code through the [wasmi WebAssembly interpreter](https://github.com/paritytech/wasmi) and pushes the results back into Redis. The transaction processor is written in Rust so it’s _fast_.

You can see a basic WebAssembly program with state running [here](https://github.com/ellipticoin/ellipticoin-blacksmith-node/blob/master/test/vm_test.exs#L18). The test calls `increment`  on `Counter` twice and then verifies that `count` is equal to `2`

Here’s [an example](https://github.com/ellipticoin/ellipticoin-blacksmith-node/blob/master/test/integration/base_token_test.exs#L21) of a transaction running through the full HTTP stack. 

* Wrote deployment scripts

[The scripts](https://github.com/ellipticoin/ellipticoin-staking-contract/tree/master/scripts) deploy the staking contract to the Ethereum Network (Rinkeby for now), mint [fake DAI](https://rinkeby.etherscan.io/token/0xA1FB77a212419bfE1B58E906DC39993823b424EC) on Rinkeby and deposit it into the staking contract. Writing these scripts took longer than I expected as development usually does.



Design changes
==============

* Remove the bridge concept in favor of cross chain atomic swaps

I talked to [Brad](https://github.com/bradleystachurski) a lot about my plans to
build a bridge where users wanting to enter the network "buy" the exists of
users wanting to exit. He pointed out that that will require liquidity of people
wanting to enter the network. Instead I’m going to use [cross chain atomic
swaps](https://hackernoon.com/atomic-swaps-simply-explained-how-to-swap-cryptocurrencies-without-a-middleman-6cd29680c32e)
to move tokens from the Ethereum network to the Ellipticoin network and back.
These also require liquidity but cross chain atomic swaps are already a proven
technology. I want to innovate in as few places as possible so I can focus on
scaling and developer experience.

* Moved block data into Postgres.

Originally I was storing block data (`transaction_hash`, `parent_block` etc) in Redis along with contract state. This ended up being difficult to query. For example I wanted to query the latest 3 blocks that were mined. This is possible in Redis but not what it was built for. Since block data is only updated once per block it doesn’t have the same performance requirements as contract state. Moving block data into a postgres database will make querying data much easier. Smart contract state will still remain in memory so execution can remain fast. 

* Moved smart contract code out of memory.

I chatted with [Geoff](https://github.com/hayesgm) last week and we agreed that if you’re going to use heap allocations in Rust the resulting WASM binaries are relatively large. The [simple token contract](https://github.com/ellipticoin/ellipticoin-base-contracts/blob/master/base_token/src/base_token.rs) I wrote results in a ~27 KB wasm file. [Parity's token example](https://github.com/paritytech/pwasm-token-example) ends up being ~39KB. Without heap allocations you can get the resulting binaries down to [less then 3 KB](https://kripken.github.io/blog/binaryen/2018/04/18/rust-emscripten.html). I think there are ways to optimize binary size but for now I’m planning to assume contract binaries will be at or around 100 KB.

This is significant and if Ellipticoin is going to store all of its state in memory this could get expensive. So, instead of storing contract code in memory I’m moving it back to the disk.  When transactions come in their code can be read from disk and pushed into a queue in memory. This can be done in parallel and ahead of time since, once it’s set, smart contract code is read-only (ie you don’t need to deal with race conditions). This way only contract state will be stored in memory. Contract state will be much smaller. Each entry in the address -> balance map is only 40B (32-byte address + 8-byte balance) for example. 

If a transaction requires another contract’s code to function that can be specified when submitting that transaction to the network.

For now I’m going to store contract code in Postgres. When it comes time to scale up moving contract code to a key value store in RocksDB or similar should be trivial.

Other Updates
=============

* I on-boarded [Brad](https://github.com/bradleystachurski).

I paired with Brad last Wednesday and we got the staking contract running
locally on his laptop. It’s so cool to have another developer helping me on the
project! Brad has a background in finance so he’s been able to give good
feedback on the economics of Ellipticoin. He’s been working in the blockchain
space for past year or so and plans to do so for the foreseeable future. If anyone
else is interested in contributing please join [the Telegram
room](https://t.me/joinchat/F0_SEksYp5PhexXW6dQ-9A) and we can go from there!

Things I've been thinking about
===============================

* Economic considerations

  * Brad brought up the point that if you burn enough DAI you could potentially send it into an upward spiral. We figured you could also liquidate the DAI in the staking contract for ETH and burn the ETH. This would theoretically reduce the supply of ETH which would drive up the price which would make the parent network more secure. I have also considered setting the burn address to a charity or a charity DAO such as [giveth](https://giveth.io/).

   * David, the creator of SIA Coin gave me some good feedback on the economics of proof of burn. He said that since energy is a resource that’s available everywhere it’s hard to gain control over it. If the staking contract burns tokens it’s possible that a malicious actor could hoard tokens to gain control over the network. I think if you hoard DAI more DAI are created. Also, if this doesn’t work we could make Ellipticoin token agnostic and have the smart contract liquidate tokens for ETH though a decentralized exchange. I don’t think we’ll need to do this for v1 but it’s good to keep thinking about.

  * Slimcoin [corrected me on twitter](https://twitter.com/Slimcoin_club/status/1004841670884708352). The project is alive and well and adding features! Sorry for mistake :)


*  DDOS attacks/Sybil Resistance
    * [Will](https://www.linkedin.com/in/willmeister/) gave some good feedback, pointing out that if the winning blacksmith node of each round is public that node is vulnerable to DDOS attacks. We figured that the protocol could be updated so that in each staking round the winning node determines the next winner and messages the winner directly. This can be expanded by not only notifying the next winner but the next few potential winners. If the first winner doesn’t mine a block after a certain period of time then the next winner can and so on. I also have considered a system where sending any message to a node costs a small fee. This would make DDOS attacks prohibitively expensive. There are definitely low tech solutions to this problem that can be used early on and it can iterated on as time goes on.



---

Design and development of Ellipticoin is far from perfect or done. I very much believe in iterative development and planning as a project progresses. If you have any feedback on any of these updates or ideas I’d love to hear from you! Come on by the [Ellipicoin chat](https://t.me/joinchat/F0_SEksYp5PhexXW6dQ-9A) and say hello!


_Thanks to [Will Meister](https://twitter.com/will_meister) for giving me feedback on earlier drafts of this post._
