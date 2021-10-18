---
layout: post
title:  "Moonshine Exchange Ships to Mainnet"
date:   2021-10-18
categories: blockchain ethereum moonshine
---

Moonshine exchange is live!  Right now  you can farm MSX tokens on Polygon and trade 5 ERC20 tokens on Moonshine Chain. Our first round of experimentation pulled in $110K in liquidity without any outbound press or marketing. 

As we launch this next prototype I think it’s important to think about how we got here, what Moonshine Exchange is and where it’s going.


How We Got Here
===

Building the initial prototype of Compound's smart contracts with Solidity and Ethereum dev tooling felt like I was working with my hands tied behind my back. I would waste hours tracking down undescriptive  error messages and researching oddities of Ethereum that were not relevant to the problem I was trying to solve.  When building out the Moonshine AMM in native Rust I finally felt like I was solving the problem at hand. I’m happy to say that itch has been scratched!

Originally Moonshine was meant to be a smart contract platform so I could share this development experience with others. I quickly learned that smart contract development is not currently the primary use-case of blockchains. The primary use-case of blockchains right now is actually speculating on tokens and earning yield.

Moonshine's new vision takes that into account.

What is Moonshine
=================

#### Moonshine is a digital asset exchange.

Currently the digital assets are only a handful of ERC20 tokens originating on Ethereum. In the future we could add additional assets including assets from other chains, other assets that earn yield and NFTs.

#### Transactions will be processed in real time a negligible cost.

At some point in the future realtime transaction processing and negligible costs will no longer be a competitive advantage for blockchains. Currently not many people use blockchains natively so slow and expensive transactions are acceptable. If people are making $1M on a trade they’re willing to pay $80 to make the transaction. If crypto is going to go mainstream that won’t be the case forever. If people have the option to trade the same assets on another venue with faster settlement times and lower costs they’ll eventually move there. At the that point the competitive advantage will be liquidity and user experience which is why we're focusing on that now.

#### USD will be the unit of account


 USD is the current dominant unit of account that most people use when trading digital assets and otherwise. USD is a superior unit of account to any network token which fluctuates based on speculation. Economics is  squishy subject but I can say personally: if I knew the Ether price or the Bitcoin price were going to stay the same for the foreseeable future I would probably trade my tokens for an interest bearing fiat backed stable coin. Currently USDC is the dominant cross chain USD backed stable coin. Moonshine reserves are currently held in Compound USDC but if there becomes a need to switch (say for example USDC is banned in the US without KYC) the DAO could swap all of the underlying CUSDC for another token, Compound DAI say.


Where Moonshine is Going
===

Moonshine has always been about solving problems pragmatically. Currently, exchanges and lending platforms are essentilly unusable for anyone who's trading less than $1000. Moonshine fixes this. Building an application chain allowed us to focus and optimize for the problem we're trying to solve, digital asset exchange, instead of having to build out an entire smart contract platform. A smaller problem space allows Moonshine to be simpler which allows for greater throughput and speed.


Currently Moonshine is a layer two app chain running on Ethereum because that's what's needed most urgently right now. It's unclear whether the future is cross chain or if digital assets will primarily exist on Ethereum. Moonshine will survive either of those futures. If ETH2 ships next year as planned transaction prices will come down and Moonshine could move to the dominant layer two. If the future is multi-chain we could add those chains as peer chains. In the mean-time we'll continue to build the best experience possible for users and build liquidity of ETH and BTC.


As always please come by [the Telgram room](http://t.me/ellipticoin) if you've got any feedback or you'd like to chat more!
