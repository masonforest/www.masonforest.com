---
layout: post
title:  "Elllipticoin Prototype #2 Launch"
date:   2020-10-26
categories: blockchain ethereum ellipticoin
---

It's been a while since our last update and as always there is a lot to report!
We’ve been heads down for the past couple months working toward the launch of
our next prototype. The prototype is up as a testnet now and all we need to do
is flip the switch to turn it on! In this prototype we’ve added a [fixed
function automated market
maker](https://web.stanford.edu/~guillean/papers/constant_function_amms.pdf)
into the base layer protocol. What this means not only will you have instant
transfers for a penny a piece, you can have instant cheap trades of any token
on the Ellipticoin network as well. We’ll also be the first layer one network
to issue the majority of our tokens through a fair launch via liquidity mining.
In this post I'll describe how we came to these decisions and where we’re going
from here.


Bitcoin was revolutionary in that it proved digital scarcity could exist on a decentralized network. After Bitcoin, people realized blockchains could be useful for other things like [registering domain names](https://en.wikipedia.org/wiki/Namecoin) and [other tokens](https://blog.omni.foundation/2013/11/29/a-brief-history-of-mastercoin/). In 2013 Vitalik Buterin published his white paper on Ethereum. Ethereum is a programmable blockchain. This enabled people to launch their own tokens and decentralized applications on top of Ethereum instead of having to create a new blockchain for every application. Since then people have been building all sorts of interesting decentralized applications on Ethereum. Recently there has been a surge in financial applications built on Ethereum which has lead to what's being called decentralized finance or DeFi. DeFi applications have over $11 billion of value locked in Ethereum dApps alone.

In this prototype we're building some of these financial instruments directly into the base layer protocol. This is better for a few reasons. First, users benefit from shared liquidity over liquidity silos. If there are two decentralized exchanges running on Ethereum one may have better prices than the other. The same goes for rates on lending and other applications. To solve this issue aggregators have been built to find the best price among these silos. Instead of having silos and aggregators Ellipticoin will have a single pool for each primitive at the base layer.

This also limits the attack surface of the system. Today, [$24 million was drained](https://thedailygwei.substack.com/p/a-rotten-harvest-the-daily-gwei-103) from a popular yield farming Dapp which took advantage of an exploit involving multiple smart contracts. If there is just one of each type of contract there's a lot fewer ways to exploit them by composing them together.

Built-in DeFi primitives also ensure uniformity across assets and primitives. ERC20 tokens can, for example, have a lot of [unpredictable behavior](https://github.com/xwvvvvwx/weird-erc20). On the Ellipticoin network all tokens are controlled by the [same contract](https://github.com/ellipticoin/ellipticoind/blob/master/src/system_contracts/token/mod.rs). Having a single automated market maker is also beneficial as you can assume all assets on Ellipticoin will be available to it. This means you could build a point of sale system on top of Ellipticoin where patrons pay in one currency and the vendor receives another: true economic abstraction.

Also when interacting with DeFi primitives transactions can be processed with machine level code which is at least an order of magnitude faster than current smart contract platforms. This also eliminates the need for gas in interaction with those primitives. Adding or removing liquidity on Uniswap is variable and can cost up to $50 USD! Adding or removing liquidity on Elliptcioin will cost a fixed fee of $0.01 USD.



[Ellipticoin's issuance schedule](https://github.com/ellipticoin/ellipticoind/blob/master/src/system_contracts/ellipticoin/issuance.rs#L19) uses the exact formula as Bitcoin's with some modified constants. 1.28 ELC will be minted every block for the first era and halved every subsequent era for a total of 8 eras. Eras last about one year each which means issuance will be complete by about 2028. At that point Ellipticoin's supply will be capped at 21 million coins: the same supply cap as Bitcoin.


With this issuance rate and a $0.50 ELC price the Elliptcoin network will be issuing about $10K worth of tokens a day at launch. Those tokens will be divided evenly among those who provide Bitcoin and Ether liquidity on the network. With more Bitcoin and Ether on the network better prices will emerge. As more and more Bitcoin and Ether get moved onto the network the value of the network grows which in turn will drive up the price of Ellitipcoin. This in turn makes a stronger incentive to bring Bitcoin and Ether to the protocol to earn more rewards and a powerful feedback loop will emerge.


It can't be emphasized enough that what we're launching is a __prototype__. The network may go down and your tokens may be locked in the bridge temporarily. It's less likely but also possible that funds will be lost all together. Don't purchase more tokens then you're happy to lose. With all of that said if any ELC tokens are stolen we will revert network state back to before the bug occurred so once you have your tokens they will be safe. We wanted to release something as soon as possible so we could get feedback and verify that everything works as expected. Here are some things we plan to improve in the near to medium term future:

* The bridge: Currently the bridge contract has a single singer and is run by the Ellipticoin team. In the short term we'd like to convert that to a multi-sig wallet and in the medium term we'd like to replace it with a MPC bridge like Keep or Ren. Note you can still buy ELC over the counter from other users without having to use the bridge. The bridge is just a convenience for moving on and off of the network. Also, we're strongly incentivized against running off with the money in the bridge. We all see much more upside in Ellipticoin's success.

* Private key management: Currently private keys of the Elliptcoin wallet are stored in the browser. One of the first things we plan to work on after the launch of this prototype is adding the ability to sign transactions using the [FIDO standard](https://fidoalliance.org/). This will enable people to keep their Ellipticoin and other assets on the Elliptioin safe in any hardware key which supports the FIDO standard.


We'd much rather release something then wait until we had the perfect product so we can ensure we're building the right thing. Our ability to iterate will give us a significant leg up over other blockchain projects. Once we've proven our assumptions we plan to harden the network until we get to a point where state is never reverted and all network upgrades have zero downtime.
   

We'll be releasing this next prototype sometime in the next 2 weeks. Join our Telegram to follow along on when exactly that will be! We're looking forward to launch and would love for you to come along for the ride with us!
