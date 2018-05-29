---
layout: post
title:  "The Ellipticoin Proof of Burn Algorithm"
date:   2018-05-29
categories: blockchain ethereum ellipticoin
---

As of this writing Proof of Work has been effective at keeping the Bitcoin
blockchain secure for almost a decade. It’s clear that the economics and
incentives are set up in a way that is effective at securing a multibillion
dollar payment network. One major drawback of the Proof of Work is that it burns
a lot of energy. At the time of this writing [the Bitcoin network burns more
energy than all of
Hungary](https://www.theguardian.com/technology/2018/jan/17/bitcoin-electricity-usage-huge-climate-cryptocurrency).
In the Ellipticoin Proof of Burn Consensus algorithm I aim to keep all as much
of the incentive structure and economics of Proof of Work in-tact while swapping
out the burning of an analog resource: energy with a digital one: in this case,
ERC20 standard tokens on the Ethereum Network.


### Validators


Validators play a very similar role to miners in Proof of Work chains. Just
like in Proof of Work chains validators take transactions from the network and
combine them into blocks. A hash is taken of the resulting state after all
transactions in a block are applied and that hash is submitted to the network. The difference is that in each round
of mining in a Proof of Work chain each miner burns some amount of energy but
in each round of validating a validator burns some amount of tokens. To participate in validation validators
must deposit tokens to a staking contract before hand. Each round some small portion
of tokens will be burned from each account (this is the equivalent of burning
energy in a proof of work contract) and the block reward will be dispensed to
the winner. Block rewards are made up of the total of all transaction fees in
the block.

### Randomness through Signature Chains

One of the core issues with burning a digital asset instead of an analog one is
you need a reliable source or randomness to select a block leader in each round
of consensus. Proof of work algorithms  garner randomness from the miners
appending random nonces to block hashes and then hashing the resulting value. If
the resulting hash value has more leading zeros than the associated difficulty
that miner “wins” the block. No miner knows which nonce will produce the
“winning” hash ahead of time so they must calculate a lot of hashes to determine
a winning value. The more hashing power a miner controls the more chances they
have of winning.  Ellipticoin generates it’s randomness from a chain of
signatures who’s first value is a predetermined random seed. To bootstrap the
network the source of a random seed value must be agreed upon. For example the
state lottery numbers at a future date or the block hash of the Ethereum
blockchain at a certain height. When that values is revealed (ie the lottery
numbers are chosen or the Ethereum block is mined) the winner of the first block
is determined. The winner is selected using Ouroboro’s [Follow the Satoshi
algorithm](https://cardanodocs.com/cardano/proof-of-stake/#follow-the-satoshi)
algorithm. In short, a single deposited coin is selected based on the random
value and the owner of that coin is the winner of that block. This means that
the more coins a validator has deposited into the the staking contract the more
chances they have of winning. To collect their reward validators must sign the
random value. This signature can be verified by the staking contract and it will
produce a new random value. This random value is then used to determined the
next winner who will be required to sign that value and so on. If a validator
fails to sign the random value and collect their validator reward that validator
has a portion of their stake slashed and another validator is chosen at random.


### Ellipticoin Specific Properties

Random seed: The random seed value of the Ellipticoin network will be the block hash of the Ethereum block at a determined height.

Token: The initial staking token will be the DAI stable coin. When DAI
stablecoins are burned new tokens will be created until their price balances out
at $1 USD. The DAI can also be used as the native currency on the Ellipticoin
network. Stable coins have a variety of advantages for over tokens that fluctuate
in value.


Token Bridge: Any token will be able to transferred from the parent chain
(Ethereum main-net) to the child chain (Ellipticoin main-net) via a bridge
modeled after the Proof of Authority token bridge. User’s will have the same
private keys on both networks and be able to move tokens between chains by
calling a function on the staking contract.

### Known Attack Vectors

Rigging the election: If an attacker knows they’re the next block winner before
submitting it to the blockchain they know the next random value. They could
potentially deposit just the right amount of tokens to be elected in the next
staking round. This is only an issue if the attacker can control who can deposit
funds to the staking contract. If someone else deposits or withdrawals tokens
from the contract the next winner will change and the attacker won’t be able to
rig the election. This could be made even more secure by required deposits to be
deposited for a significant amount of time before being eligible for staking
rewards. This would require the attacker to maintain control over the network
for the entire warm up period to carry out an attack.

51% attack:  An attacker can stake a large amount of tokens, wait to be elected
and sensor transactions just like in a Proof of Stake chain. Each time a
validator withholds a block their stake will be slashed which would end up being
very expensive. For greater security you could build a system which slashes
stake exponentially as more blocks are withheld. If transactions are being
censored from the Ellipticoin network they could be submitted to the parent
network. That way attackers would need to 51% both the Ellipticoin network and
the Ethereum network to carry out an attack.

### FAQ

#### What are the benefits of using Ellipticoin over Ethereum natively.

The Ellipticoin network runs the WebAssembly Virtual Machine which means developers can write smart contracts is a variety of languages including Rust, C and C++. The Ellipticoin network can also process thousands of transaction per second.


#### Why not just implement Plasma?

I've been following the research the Plasma people have been doing and I love
the work they’re doing. That said I think it’s important to take multiple
different approaches to solve second level scaling. The benefit of second level
scaling solutions is many of them can be tested simultaneously and share
innovations. I look forward to sharing any ideas and learnings that I come
across and seeing both solutions develop. 🚀

#### Has proof of burn been implemented before? Why did it fail?

[Slimcoin](http://slimco.in/) seems to be the only semi-functional proof of burn
blockchain. One of the reasons it’s failed it they’re burning assets on their
own chain. I don’t know of any attempts to implement Proof of Burn on top of the
Ethereum network.


### Demo

I’ve implemented a proof of concept of the staking contract [here](https://github.com/ellipticoin/ellipticoin-staking-contract).

And there’s a live demo of it running here: [https://staking-demo.ellipticoin.org/](https://staking-demo.ellipticoin.org/)

Thanks to the [Proof of Authority](https://poa.network/) team for supporting the [Sokol test network](https://sokol-explorer.poa.network/) where it’s deployed!

I've also deployed a proof of concept of the Ellipticoin network along with a
[Getting started Guide](http://ellipticoin.readthedocs.io/en/latest/).


