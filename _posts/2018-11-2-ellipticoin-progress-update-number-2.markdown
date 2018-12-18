---
layout: post
title:  "Ellipticoin Progress Update #2"
date:   2018-11-2
categories: blockchain ethereum ellipticoin
---

This will be a shorter update since it’s only been two weeks since the last update. The big news is that [mana](https://github.com/mana-ethereum/mana) the Ethereum client I’ve been working on with PoA Network for the past few months will be getting support from two new organizations: [Compound](https://compound.finance/) and [Consensys](https://consensys.net/)! Here’s [a blog post](https://medium.com/poa-network/poa-network-compound-and-consensys-announce-collaboration-on-ethereum-client-written-in-elixir-b265d048402) about it from PoA Network. With the added resources and connections this will provide I’m confident we’ll be able to achieve our goal of building a first class client that can compete with Geth and Parity. In the next few weeks a few more developers will be added to the project. Currently my plan is to step back into an advisory type roll on that project and work on Ellipticoin full time!

#### Technical updates


1. Got the blacksmith node to [“watch” the staking contract](https://github.com/ellipticoin/ellipticoin-blacksmith-node/commit/479935c1bcb86b5a59e0f0de7d51050ce73c7aab).

    For the blacksmith node to function it’ll have to watch the staking contract on the parent chain (Ethereum). Every time block is mined on the parent chain we’ll call `winner` on the staking contract. If the node calling it is the winner they’ll process a block of transactions, broadcast that block to the network and call the staking contract to collect their reward. To get this to work I needed a way to be notified of new blocks being mined on Ethereum. I opened [a PR on Ethereumex](https://github.com/mana-ethereum/ethereumex/pull/45) to add WebSocket support but decided to close it because fully supporting WebSockets in Ethereumex would take a significant amount of time and there’s too much to do on Ellipticoin right now. My branch solves the problem I needed to solve though. The node can now successfully subscribe to a channel over a WebSocket to Infura and listen for new blocks. When a new block arrives it calls `winner` on the staking contract. The next step will be to forge a block and notify it’s peers if it’s the winner. Before doing that I needed to do more work on block forging.

2. Started [modeling block data](https://github.com/ellipticoin/ellipticoin-blacksmith-node/commit/48cd1f13087e9ce95a918e33bc280f8f429cb63b)

    To start blocks have the following attributes: 

    * parent: the parent block
    * number: the block number
    * total_burned: the total number of tokens (DAI) burned on this chain
    * changeset_hash: a hash of all the writes made by this block. (See the following section on the changeset hash)
    * block_hash: this is a hash of all the other values. This can act as a global identifier for blocks



    Using real models and a relational database to store block data has been much more intuitive than storing everything in an on disk in a key value store like Etheruem and Bitcoin do. I think it will also make the code more clear and make it easier to query data about the chain.


3. [Simplified Redis PubSub](https://github.com/ellipticoin/ellipticoin-blacksmith-node/commit/4a39f21c1d52daea4cf8cc71c5ddfbfe1f6e4eee#diff-e643f98fa645a486892b11cd9b9b4786)

    When transactions are posted to a Blacksmith node Elixir pushes them into a queue is Redis. This is the equivalent of the mempool in Bitcoin and Ethereum. When it’s time to mine a block Elixir has to tell Rust to start processing transactions. Originally I thought I would do that through a [nif](http://erlang.org/doc/tutorial/nif.html) but it turns out that nifs [disrupt the scheduling of other processes](http://erlang.org/pipermail/erlang-questions/2010-November/054699.html). Since transaction processing was going to run for two full seconds I wanted to find a better way. Enter [Redis PubSub](https://redis.io/topics/pubsub)! Now when it’s time to process some transactions we can call [Redis.publish(@channel, ["proccess_transactions", duration])](https://github.com/ellipticoin/ellipticoin-blacksmith-node/blob/master/lib/transaction_processor.ex#L42) from Elixir. On the other end Rust is [subscribed](https://github.com/ellipticoin/ellipticoin-blacksmith-node/blob/master/native/transaction_processor/src/main.rs#L46) to that channel and can [receive the message](https://github.com/ellipticoin/ellipticoin-blacksmith-node/blob/master/native/transaction_processor/src/main.rs#L50). It then processes transactions for the specified amount of time and then publishes back to the channel when it’s done. Elixir then picks up on this message and sends it to the appropriate process. Using Redis PubSub over nifs will keep BEAM from blocking allow for richer communication between Rust an Elixir



#### Protocol thoughts and updates

1. Networking

    I've been thinking a lot about how networking will work in Ellipticoin. Devp2p, Ethereum’s networking protocol, is probably the most complicated part of Ethereum. I want the networking code in Ellipticoin to be as simple as possible. To start it’s going to work a lot like a modern RESTful JSON api but with [CBOR](http://cbor.io/) instead of JSON. This way payloads will be smaller and deterministic so they can be signed. All messages will be broadcast directly to all other nodes. If this starts to be problematic we can switch to CBOR over libp2p.

2. Added the changeset hash

       Ethereum stores state in a Merkle Patricia tree. This means that every state update needs to walk the tree to a leaf node and update each node along the way. This adds a lot of complexity to the protocol  and ends up being a bottleneck in processing Ethereum transactions. Ellipticoin won’t store state in a Merkle tree or a Merkle-Patricia tree 😮. Instead, each time a write is made the value that was written and the key it was written to are appended to a string of bytes. When a block is forged a hash is taken of this string. Other nodes can then run the transactions themselves and verify that they get the same resulting hash. This means that instead of having to run the hashing algorithm multiple times for each individual state update you only have to run it once per block! This speeds things up and make implementation much simpler. The downside is you won’t be able to run Ellipticoin light clients. This shouldn’t be a problem though because if you don’t want to run a full node you can still verify the final changeset hash because it will be submitted to the parent chain (Ethereum).

3. Thinking about the signature chain

    When submitting a block to the chain a blacksmith will sign the following message (<> means concat):

    `block number <> changeset_hash <> last_signature_in_signature_chain`

    If a blacksmith signs two messages with the same `block_number` and `last_signature_in_signature_chain` with a different `changeset_hash` their stake will be slashed. This is the equivalent of [slashing](https://blog.ethereum.org/2014/01/15/slasher-a-punitive-proof-of-stake-algorithm/) in PoS algorithms and solves the nothing at stake problem.



Thanks for reading!

If you have any questions or comments on any of this swing by the [Ellipticoin Telegram room](http://t.me/ellipticoin) and say hello!
