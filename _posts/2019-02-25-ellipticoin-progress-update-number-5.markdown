---
layout: post
title:  "Ellipticoin Progress Update #5"
date:   2019-02-25
categories: blockchain ethereum ellipticoin
---

I'm back to Ellipticoin being a layer 1 solution again! After toiling with the
staking contract for about a month I realize it&apos;s actually going to be less work
to build a base layer blockchain than it will to build a layer 2 solution on top
of Ethereum. I think it&apos;s also a lot more interesting to create a layer 1 
solution as well. Here&apos;s how it&apos;s going work:

The consensus mechanism will be a hybrid proof of work/proof of burn system. At
any time miners can send tokens to the burn address. Miners will compete to
find the next block using the hashcash algorithm used in Bitcoin with one minor
adjustment: before miners would hash a block hash and a nonce and complete to
find a significantly low hash value:

Hash(Block Hash + Nonce)

(+ is the concat operator)

In Ellipticoin&apos;s hybrid model the miner can include burn transactions in the
hashed contents as well:


Hash(Block Hash + Nonce + Burned Transaction 1 + Burned transaction n)

Including burned transactions will have a multiplier effect on the chances of
that miner winning the block. So a block that&apos;s mined with difficulty 1  and
burned 10 coins will  “beat” a block that&apos;s mined with difficulty 2 and burned 2
coins. A constant will be chosen so that difficulty and burn value will be
weighted properly. The goal will be 0.01% of of burned value should be energy
and 99.9% should be burned tokens.


The idea here is that in the current Bitcoin network miners who spend more money
on energy can increase the number of hashes they generate and therefore increase
their chances of winning a block. In the Ellipticoin protocol miners will
purchase and burn tokens instead. This has all the same economic incentives as
the current system with some nice additional benefits. First, burning coins is
much more energy efficient than burning energy. Also, it levels the playing
field as the price of energy will probably vary more based on where you are in
the world than the price of tokens will. This also allows the protocol to
“recycle” the burned coins back into the system to pay for continued security.

Ellipticoin&apos;s monetary policy will be very similar to Bitcoin&apos;s. There will only
ever be 21 million Ellipticoin&apos;s. The supply curve will almost match that of
Bitcoin&apos;s except it&apos;ll start 10 years later. Once issuance slows the network can
continue to use burned coins to pay out miners at a constant rate for continued
security. That way the network will never have to rely on transaction fees alone
to fund itself.


Ellipticoin will have mostly of the same economic incentives as existing
protocols so very little new research will need to be done in order for it to be
built.

One additional detail I thought of was weighting the harvesting of “older”
burned transactions as more valuable. That would incentivize more people to burn
more tokens earlier. This is something I want to think more about.

I&apos;ll continue to keep you all posted on my progress.

Thanks for reading!

If you have any questions or comments swing by the [Ellipticoin Telegram
room](https://t.me/ellipticoin) and say hello!
