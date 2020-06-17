---
layout: post
title:  "Ellipticoin Launch and Post Mortem"
date:   2020-06-17
categories: blockchain ethereum ellipticoin
---

Ellipticoin launched! The network ran for a total 4 hours 48 minutes and 55
seconds at which point a hacker managed to stall the network. I wrote a
post-mortem below but first a bit more about the launch!

We had [22 trades and
$4,142](https://uniswap.info/pair/0x73b647a974cF248c20Ef00359C2F2f86545CBa9F) in
trade volume go through the community bridge and more and more people are
hearing about Ellipticoin organically. Our goal was to make it to $10k in volume
this phase but $4k is plenty to prove out people want what we're building and
that we should continue to iterate.

Post Mortem
===

Some day Ellipticoin will be known as the most secure blockchain on earth. Right
now that's not possible or realistic. Normally before blockchain networks launch
they pay for an audit which is basically a proof read but for  code and instead
of searching for spelling errors auditors search for bugs. Blockchain audits can
cost $100k or more! Since we're bootstrapping Ellipticoin we can't afford an
audit. Instead we're launching as is, warning people of the risks and fixing
bugs as they come up. When bugs do come up we roll back state to the place where
the bug occurred, fix the bug and launch the network from there.

We knowingly went this route and it didn't take long for a bug to be exploited.
The exploit that caused the network to stall was creative and I tip my hat to the hacker that exploited
it. I hope soon for the Ellipticoin DAO to have funds to properly compensate
white hat hackers.

The hacker made use of two issues with Ellipticoin and managed to stall the
network. One was what's called a [replay
attack](https://en.wikipedia.org/wiki/Replay_attack) and the other was how we
choose block winners.

In block 2876 the hacker sent a "start_mining" transaction signed by one of our
miners. How were they able to generate a signature without the private key? They
didn't actually. They just copied a transaction that had already been run on a
previous iteration of the network. The solution to this is to add a `network_id`
field to transactions so they can't be replayed. It looks like Ethereum had [a
similar issue](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-155.md) at
launch which has since been fixed.

The "start_mining" transaction added that miner to the list of available miners.
When that miner was selected the winner all the other miners waited for that
miner to produce the next block. Since that miner wasn't really online it never
produced a block and the network stalled. If a miner fails to produce a block
after a certain amount of time we should slash their stake and let the next
miner mine the block.

Of all of the things that could have gone wrong this is relatively not that bad.
No tokens were stolen and everybody who bought will have their tokens when we
relaunch. As we continue to iterate we'll continue to harden the network and
make it more secure.


Going forward
===

We're going to take the next few months to fix the bugs that were uncovered in
this phase and build an automated market maker on top of Ellipticoin. We're also
going to build a bridge on [Ren](https://renproject.io/) so we can move all ERC20
tokens over to Ellipticoin. This will mean at the launch of phase II you'll be
able to trade and transfer all ERC20s and Bitcoin instantly and cheaply all
while maintaining custody of your coins. That's when things get interesting.
Keep an eye out for the launch of Phase II in late summer early fall 2020.

In the meantime we’re going to disable the wallet at  wallet.ellipticoin.org so
we can focus on building phase II. We are however  also going to keep phase I of
the network running in the background so we can continue to iterate on the user
experience of onboarding and continue to find and squash bugs. If you’re
interested in early access and helping us test our onboarding experience message
us in [Telegram](https://t.me/ellipticoin) and we can help get you set up!

