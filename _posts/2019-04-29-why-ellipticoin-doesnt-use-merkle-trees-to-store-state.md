---
layout: post
title:  "Why Ellipticoin doesn’t use Merkle trees to store state"
date:   2019-04-29
categories: blockchain ethereum ellipticoin
---
Ellipticoin won’t use Merkle trees to store state. That may be blasphemy to the
blockchain purest but the goal of Ellipticoin is not to be the most pure.  The
goal is to make the best tradeoffs necessary to create a blockchain that can
support global scale that’s sufficiently decentralized both technically and
socially.


The primary reason for making this decision is to optimize transaction
processing speed. Changing values at every node of a walk of a tree is going to
be slower than changing a single value. While operations on Merkle trees can be
optimized they are never going to be faster than using a straight key-value
store.

Another benefit of dropping Merkle trees is it makes the implementation of the
chain a lot simpler! A considerable amount of time went into researching and
building out an implementation of Merkle Patricia trees when we were building
[Mana](https://github.com/mana-ethereum/mana).  Simpler software runs faster, is
more secure and is easier to implement. This will make it easier to create
multiple implementations of Ellipticoin! If it’s easier for more people to
create and maintain more implementations of Ellipticoin the project will be more
socially decentralized. 

The most significant drawback of his tradeoff is that Ellipticoin won’t be able
to support light clients. If it was possible to support light clients without
making significant other tradeoffs that would make sense but that’s not the
case.

Regardless of the blockchain if you want the strongest guarantees about security
and decentralization you should run a full node. One of Ellipticoin’s goals is
to make running a full node more feasible. If running a full node is easy there
isn’t as big a need to run a light client.

If you don’t want to run a full node you can still run a zero-client.
Zero-clients such as Metamask don’t require Merkle trees to verify transactions.
Zero-clients are actually much more commonly used today than light clients. A
medium term goal of Ellipticoin is to further decentralize zero-client
infrastructure as well. Instead of relying on a single service provider there
will be a network of edge nodes that accept transactions.

I’m working on implementing state transitions without Merkle trees now. If I
find that it’d be easier or superior in some way to use Merkle trees they can
always be added back.
