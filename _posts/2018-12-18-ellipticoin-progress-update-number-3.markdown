---
layout: post
title:  "Ellipticoin Progress Update #3"
date:   2018-12-18
categories: blockchain ethereum ellipticoin
---
Since the crypto markets have been down recently my contract working on [Mana](https://github.com/mana-ethereum/mana) has ended. The good news is that means that I’m now focusing on Ellipticoin full time! I’ve spent the past few weeks building away. After Thanksgiving I spent the week in Boston and worked with my friends at [LevelK](https://www.levelk.io/) and my friends ar [Neon Labs](http://neonlabs.co/) who are in the processes of joining [Keep](https://keep.network/)). I also went to a crypto happy hour where I got to meet [Corbin Pon](https://www.linkedin.com/in/corbinpon) of [Keep](https://keep.network/) and [Nick Emmons](https://www.linkedin.com/in/nickemmons), the lead blockchain engineer at John Hancock and many others. It was great to be able to chat directly with the people building decentralized applications so I could hear their pain points and use that that feedback to shape what Ellipticoin will be.

Here’s some updates from the past few weeks:

### Updated elevator pitch


Over time Ellipticoin has been evolving. Here’s the latest paragraph explaining what Ellipticoin is:

Ellipticoin is a developer friendly, scalable, Ethereum side-chain. It runs the WebAssembly Virtual Machine uses the DAI stablecoin as the it’s base token. Blacksmith nodes are authenticated via proof of burn and forge transactions to keep the network secure. Proof of burn has similar economic incentives to proof of work except instead of burning energy they burn tokens on the parent chain.


### A Rehashing of How the Bridge Will Work

For the network to be useful in it’s early days there will need to be a way to move tokens from the parent chain to the child chain and back. Tokens can be minted on the child network at any time by sending them to the bridge contract where they will be locked. When sending tokens to the bridge contract users will specify their address on the child network. Each time a child block is confirmed on the parent chain the blacksmith node will check for new transactions in the bridge contract. It will then take those transactions and “mint” those tokens on child chain. When the user is ready to exit they will send their token that was minted on the child chain to an exit contract and include their address on the parent chain. Users on the parent chain can then “buy” those exits by calling another function on the bridge contract on the parent network. When the blacksmith node mines blocks it will also process exit purchases. When exits are purchased the blacksmith will unlock tokens in the exit contract on the caild network and send them to the appropriate address. If there aren’t any buyers on the parent chain the user on the child chain can drop the price of their exit and try again. You then have a market for entries and exits.  There’s a couple trade offs here. First, you’ll need to build a market of entries and exits. I’m assuming that blacksmith nodes will make enough on transaction fees to bootstrap this liquidity. Second, it’s the users job to check that they’re purchasing a valid exit. If someone purchases an exit on an invalid chain they won’t be able to get their money back. This should be able to be solved by letting software determine what is a good and a bad exit. In making these tradeoffs we get two nice benefits. Firstly, we won’t need to wait for a challenge period like in a traditional plasma chain. This means that exists could be as fast as 1 block on the parent network provided there’s liquidity. The other, more important benefit is that the child chain can run it’s own virtual machine without having to have a complex mechanism for checking validity of exists on the parent chain. This is much simpler and cheaper to run on the parent chain.

### Feedback From Steve

I reconnected with and old friend [Steve Marx](https://twitter.com/smarx) who gave me some incredibly valuable feedback on Ellipticoin. First, he discovered [a bug](https://github.com/ellipticoin/ellipticoin-base-contracts/commit/b7c38c2fd73329d5d8701fd1feef5d920cf291f5) in Ellipticoin token contract within minute or two of looking at it. He also, more importantly, discovered a flaw in Ellipticoin’s leader election algorithm! He pointed out that [ECDSA’s signature algorithm](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm#Signature_generation_algorithm) is not deterministic. When calculating a signature the signer must choose some random k value that will affect the final signature. This could be exploited by the attacker continually choosing k values which result in them winning the following round of staking. The good news is that RSA signatures _are_ deterministic. I’ve found an [RSA library written in Solidity](https://github.com/adriamb/SolRsaVerify). All I need is add the ability for blacksmith nodes to register their RSA public keys and swap ESDSA signature verification with RSA signature verification. 

Steve also pointed out that I have a bug in my [`set`](https://en.wikipedia.org/wiki/Set_(abstract_data_type)) implementation. I need to add a check to make sure addresses are [only added](https://github.com/ellipticoin/ellipticoin-staking-contract/blob/master/contracts/Depositable.sol#L22) if they didn’t previously have a balance. Finally, and I’m sure he’d find more if he did a deep dive, Steve pointed out that the loop to determine block winners could consume a dangerous amount of gas. Also, it will be more expensive to be a blacksmith that joins later than it will to be one that joins earlier. I don’t think that any of these bugs are show stoppers. It’ll take some time to squash them and perfect the system but I’m still confident Ellipticoin can be successful.

I owe Steve big time for taking a look at this for me! I can’t thank him enough!

While it’s important to address these bugs it also re-enforces my belief that there’s room for improvement on smart contract development tools. For example the `set` bug: If I were to rewrite this smart contract in Rust I could use the builtin [set type](https://doc.rust-lang.org/std/collections/struct.HashSet.html) and I wouldn’t need to implement that data type myself. And for the expensive loop bug: miners are paid [over a billion dollars a year](https://cryptoslate.com/report-nearly-2-5-billion-paid-annually-to-ethereum-miners-eth-issuance-woes-continue/). It seems reasonable that we could arrange them in such a way that programmers could write smart contracts without having to optimize them as much as we do today. Programming is all about tradeoffs. Sometimes optimizations can be traded for simplicity. If we could fully take advantage of the underlying hardware of the nodes it would allow for simpler software to be written with less bugs.


### Simple Block Explorer Launched

I’ve launched a simple [Block Explorer](https://block-explorer.ellipticoin.org/)!

What’s actually going on here?

I have 3 live nodes on the network: Davenport, Joyce, and Fritz (They’re all named after [famous blacksmiths](http://theconsummatedabbler.com/2016/06/25-of-the-worlds-most-famous-blacksmiths/)).

The staking contract is [deployed](https://rinkeby.etherscan.io/address/0x756d0ABF6235AB135126fe772CDaE195C3DECc0e) the the Rinkeby testnet. Each of the nodes has their own private key and is subscribed to listen for new blocks. When a new block comes in the node determines whether it’s the winner. If it’s the winner it runs the transactions in its transaction queue and which generates a hash of state changes. It then broadcasts the block to all other nodes, submits that hash to the the staking contract, and signs the signature chain. The value of that signature determines the next winner and the processes repeats. Each node has a CBOR over REST API and a Websockets API. The block explorer is a React app that hits the REST over CBOR API and then connects to the Websocket API to listen for new blocks. Currently each block on the Ellipticoin network corresponds to one block on Rinkeyby. Eventually the goal is to have multiple elections happen between blocks.


### Some Quick Thoughts On Governance

I listened to [a great debate](https://www.zeroknowledge.fm/52) between Gavin Wood and Vlad Zamfir on blockchain governance mechanisms. It got me thinking a lot about how I think governance should work in Ellipticoin. I want to write a lot more about my thoughts on governance but I thought I’d start by giving a high level overview of my thoughts here.

I think it’s important to recognize that there are different pieces to a blockchain and they can potentially be governed differently. First off there’s technical governance. From a technical perspective there’s not much difference between specifying how a virtual machine should work vs specifying how an HTML rendering engine should work. I think there are existing projects and technical standards that are maintained by groups of people that we can learn from.  One of the goals of Ellipticoin is to rely on existing components making the spec itself as thin as possible. For example, instead of specifying an Ellipticoin specific virtual machine, I’m using the WebAssembly VM. This is the same reason I chose [CBOR](http://cbor.io/) as a binary encoding format instead of creating my own and the same reason I’m using libp2p over a custom networking protocol. All of those pieces have existing governance processes. Of the remaining technical components that need to be governed the client developers can work together in a traditional spec process. If there are serious disagreements they can resort to a fork.

The second thing that needs to be governed is monetary policy. With the initial iteration of Ellipticoin I want to outsource that as well to Maker and the DAI stable coin. DAI tokens have maintained their value through a 70% drop in Ether price which backs the tokens. Once multi-collateral DAI is launched I’ll feel even more confident that it can maintain its value through volatility of the underlying collateral. I imagine using a stable token as the base token will be controversial: why use a network where you can’t benefit from it’s adoption over one where you can? There are plenty of places to make money in the blockchain space other than in speculation. You can consult. You can run nodes. You can build a business. The incentive to use a network should be that it’s useful not that you can make a profit if other people use it. That network will not only need to be more useful, it will need to be substantially more useful, enough to make people use it even though there isn’t a financial reward associated with using it. I’m confident that this can be done. Having a stable token is the best user experience for users of the network as well. Users won’t need to take into account fluctuations in price when calculating gas costs or prices of goods and services.  These decisions fall back on philosophical arguments that people have been having for centuries. I don’t think people change their mind much on these topics. If you disagree I’d love to hop on a video call and discuss. Also, since the code is liberally licenced anyone is welcome to fork the codebase and launch a network with their own incentive mechanisms!


Finally forks and the name need to be managed. When Bitcoin and Ethereum forked their communities decided which fork got the name and which fork had to add a suffix. Each Ellipticoin network will have it’s own network id. Therefore anyone can run create a fork at any time assigning a new network id. Want to build a proof of stake version of Ellitpcoin? Go for it! Want to hardspoon another network? Sure! If the name is associated with the underlying software instead of a fork “ownership” of the name becomes less meaningful. Also, since the base token is a stable coin forks should be less contentious. If users disagree with a fork they can move their tokens back to the parent chain or to another child chain fairly easily.

### Potential Name Change

Since Ellipticoin is now going to be an Ethereum sidechain instead of it’s own chain I’m considering renaming it. I’m thinking maybe Plasma - something? Plasma Prime? Plasma WASM? If you have any ideas come share them in the [Ellitpicoin Telegram room](https://t.me/ellipticoin)

Thanks for reading!

-Mason


