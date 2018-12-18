---
layout: post
title:  "What is blockchain technology and why does it matter?"
date:   2018-12-8
categories: blockchain ethereum ellipticoin
---
Wikipedia defines blockchain as:

  _A __blockchain__, originally __block chain__, is a growing list of records, called blocks, which are linked using cryptography.[1][6] Each block contains a cryptographic hash of the previous block, a timestamp, and transaction data (generally represented as a merkle tree root hash).
By design, a blockchain is resistant to modification of the data. It is "an open, distributed ledger that can record transactions between two parties efficiently and in a verifiable and permanent way"._

If that just looks like a bunch of technical gobbledygook that’s because it is. Most definitions of blockchain I see look similar to that. Currently blockchain is mostly just a buzzword to generate hype but it does have to potential to have a massive impact on the world. Like any technology, it’s not good or evil on it’s own. It’s going to depend on how humans use the technology. It’s important that as many people as possible have a least a high level understanding of what blockchain is so they can make their own decisions and help lead the technology in a positive direction. 

--

I’ll start by explaining what Bitcoin is and we can build on that. Bitcoin was the first blockchain and it’s goal was to act as digital cash.

To start imagine you have two people Alice and Bob. Alice and Bob both have ten US dollars. Instead of paying each other directly they want to keep track of their payments in a spreadsheet.

Alice has the spread sheet on her computer.

Alice buys Bob a beer. Beers cost $1 so she records it in the spreadsheet.

    * Bob owes Alice $1

Bob then buys Alice a taco. Tacos cost $3 dollars so Alice records that as well.

    * Bob owes Alice $1
    * Alice owes Bob $3

They can then total the payments and “cash out”. After totaling up the payment Alice owes Bob $2. She gives him $2 and they’re both happy.

The problem with this setup is that Alice has the spreadsheet on her computer. Bob has to trust that she won’t add any lines to the spreadsheet without his approval.

They could have given the spreadsheet to Bob but then Alice would have to trust Bob wouldn’t add any extra lines.


To solve this problem they can go get 6 of their friends. When either of them wants to add a line to the spreadsheet they tell all 6 people. Let’s say it’s “Bob owes Alice $2”. Each of the six people gets assigned a number and then they roll a dice to pick the leader of the group. The leader of the group then says the transaction out loud: “Bob owes Alice $2.” If everyone agrees they each add that line to their spreadsheets. If that person lies, say for example they say “Bob owes Alice $3”. They’re kicked out of the group. Alice and Bob find a friend to replace them and the process starts over again. As long as at least half of their friends are honest this process will work and they don’t have to trust any of their friends individually.

This is roughly how Bitcoin works with a few changes. First instead of choosing 6 friends anyone in the world can attempt to be selected as the leader and everyone is rolling dice. In Bitcoin these people are called miners. Rolling a dice is replaced running a program. The program doesn’t just print out a value 1-6 it can return anywhere between  0 and 11,579,208,923,731,619,542,357,098,500,868,790,785,326,998,466,564,0564,039,457,584,007,913,129,639,935. No joke, that’s 2^256, a really big number. The program also takes a tiny bit of energy to run. Miners run the program over and over again trying to get the lowest number. When a miner gets a low enough number they’re the winner and they can include some transactions in the “spreadsheet”. If they tell the truth everyone starts the the process again. If they lie they kick that miner out of the group and everyone starts again. Whichever miner wins the block also gets a reward for the block of transactions they mined.

This reward is paid in the Bitcoin the currency.

Bitcoin the currency is where a lot of people have doubts. How can a currency that isn’t backed by anything hold value? I’m still not completely sold on this concept either. The truth is it sort of works like a ponzi scheme. Investors see the massive profits that earlier investors made and buy in in hopes that the value will continue to increase. They are then incentivized to spread the usage of Bitcoin which makes more people hear about it. Those people then see the profits made by the previous round of investors and so on. There’s also a community and culture that has developed around the these currencies. People share memes on the internet about [“HODLing”](https://en.wikipedia.org/wiki/Hodl), “lambos” and “going to the moon”. People involved in this culture then become emotionally invested which develops power and value on it’s own. Economics is hard because people are [irrational and unpredictable](http://freakonomics.com/podcast/richard-thaler/). Whether the price of Bitcoin and Ether, the native currency of Ethereum, collapse, “go to the moon” or stay somewhere in-between there are still a large potential in the underlying technology.

I see Bitcoin and blockchain technology as two discoveries and two resulting technologies:

Discovery One: The blockchain
===========
This is the term that stuck but surprisingly this it’s the least interesting technically and the term wasn’t even used in [Satoshi’s white paper](https://bitcoin.org/bitcoin.pdf). This is just a way of verifying that all transactions are included in a chain.  It’s possible to take something called a hash of a group of transactions. Think of a hash like a fingerprint but if you change any transaction in any way the fingerprint changes as well. In the Bitcoin blockchain this fingerprint not only includes all of the transactions in that block it also includes the fingerprint of the previous block. That block’s fingerprint contains a fingerprint of the previous block all the way back to the first block. So any change to any transaction in the whole history will change the final fingerprint. In reality this system is more complex and uses something called [Merkle Trees](https://en.wikipedia.org/wiki/Merkle_tree), but the basic result is the same: any change to any transaction will change the final fingerprint or hash. This way, if you agree on the hash you can agree on all transactions in the whole chain.

Discovery Two: Authentication by Proof of Work
====================================

For any of these systems to work you need a way to count “votes”. In our example with Alice and Bob each of their six friends had one “vote” if they were the winner of the game. Initially you might think you could say that each miner gets one “vote”. This is hard though because what happens if one miner runs the program 1000 times, should they get 1000 votes? One of the key mechanisms that makes Bitcoin work is that running the mining software costs energy. You can run the mining software 1000 times on the same computer but it will cost 1000 times as much. This limits how many “votes” everyone gets. No single person can get unlimited votes because no single person has access to unlimited energy.

Resulting Technology One: Immutable Record
=================================
Over time it becomes more an more expensive for a Bitcoin block to be changed. This is useful if you want to prove something was true at a certain time. Say for example you have an earnings report that you want audited at a later time. You can take a hash for that report (remember from before that’s like taking the fingerprint) and put that hash in the Bitcoin blockchain. Then later on you can prove that earnings report hasn’t changed by showing someone the hash. [Factom](https://www.factom.com/) is a blockchain built specifically for this purpose. This is useful for some cases. If you don’t understand this one I’d say it’s the least important one so don’t worry. 
 
Resulting Technology Two: Digital Scarcity.
===========

Digital scarcity is the idea of having a limited amount of something that’s digital. How many copies of your email do you have? You’re right! Unlimited copies! You can copy an email as many times as you want. You can also forward an email to someone and keep a copy for yourself. This is fine for email but it doesn’t work for things like digital cash. Being able to copy digital cash would be sweet but would make it not very useful as a currency. Bitcoin the currency was one of the first things that was made digitally scarce. There will only ever be 21 million Bitcoins in existence. This is what theoretically makes them valuable. Ether, the base currency on the Ethereum network is also scarce. Ethereum allows developers to build their own assets on the network similar to how Apple lets developers build apps on the App Store. One of the most famous apps (Dapps, they call them) is CryptoKitties where one of the kitties sold for [$170K](https://thenextweb.com/hardfork/2018/09/05/most-expensive-cryptokitty/). Another use case is people are building something like digital shares of companies on the blockchain and selling them off at a huge profit. These are called ICOs and they collectively raised over [$13.7 billion dollars as of July this year](https://cointelegraph.com/news/pwc-report-finds-that-2018-ico-volume-is-already-double-that-of-previous-year). Ownership of things in the digital world is something that was not possible before. You could “own” things on farmville but Facebook always could take them back if they wanted to. Taking things back isn’t possible on the blockchain. Bitcoin might die, Ethereum might die, but the idea of true ownership of digital assets is not going to go away.

Resulting Technology Three: Shared Infrastructure
=======================================

This is mostly what big companies mean when they say “blockchain” which is ironic because these systems don’t actually require a real chain of blocks or a blockchain at all. Confusing I know!
Once of the most famous uses of blockchain technology by a big company is when [Walmart required
spinach suppliers to contribute to a blockchain](https://www.nytimes.com/2018/09/24/business/walmart-blockchain-lettuce.html) to keep track of contamination.

With digital scarcity we have a way we can “pay” a group of people to run a database. A database is basically a fancy word for a speadsheet: a place where you can store information. Since it’s run by a third party multiple people can all store data in the same database and no single person has to be trusted with it. In the future Walmart could have spinachece providers all submit information to a public blockchain and then it could verify that none of the information had been modified. But since IBM is running all of the software IBM has access to change the values in the database. This is not really a blockchain in any sense of the word. This is a shared database. I think what they’re doing is getting ready for when public blockchains ARE available. Or they’re interested in getting buzz or both. The point is being able to share data between organizations is going to be huge! When a lot of people use the word blockchain they just mean “sharing data.” This part of the blockchain I think think is most confusing because I think it’s unclear what was possible before, what’s possible now and how much of it is just to get the hype. In any case it has the potential to be transformative as well. 

Resulting Technology Three: Programmable Value (Smart Contracts)
======================================

If Bitcoin is a spreadsheet of transactions, Ethereum is a spreadsheet with formulas. You can program the formulas however you want and after the code is deployed to the Ethereum blockchain it will run a long as the Ethereum network itself runs.

#### Smart Contract Example 1 - Insurance:

A good example of a functioning Dapp on Ethereum is [Fizzy flight delay insurace](https://fizzy.axa/en-gb/).The rules of the contract are written into the code (that’s why it’s called a smart contract): First you enter your flight number and pay a premium. If you flight is delayed the contract will pay you out directly. If we go back to our spreadsheet analogy that would be like having a calculated field that added to your balance if the “flight delayed” column was checked. This is all programmed in a programming language called Solidity and programmers can look over the code to make sure it does exactly what it says it’ll do (just like how you’d have a lawyer look over a contract).

This is just a simple example but the implications of that are big! Normally for an insurance contract you’d have to trust that the company wouldn’t run away with your money. Since all the rules are written into the contract and the money never leaves the Ethereum blockchain you no longer have to trust the company! Even if fizzy wanted to they couldn’t withhold a payout if your flight was cancelled. Once the program is deployed and the contract is entered Fizzy doesn’t control the money. The contract does. Before this we always had to trust someone to follow through with financial contracts. Sure we can sue them them if they violate the terms of a written contract but that’s expensive. Once a contract is written they don’t have the ability to break it and therefore no longer need to be trusted.

#### Smart Contract Example 2 - Finance:
Once you have programmable value any financial instrument which used to be written on paper can now be written as a smart contract instead. [Compound finance](https://compound.finance/) has already built out the equivalent of capital markets on Ethereum. Users can send their tokens to Compound’s smart contract and contract will pay them interest. The contract will also loan those tokens to another group of users and charge them interest. The contract takes a tiny cut and pays Compound to keep developing the service. Since these contracts on on the blockchain Compound doesn’t have the ability to steal user’s funds or prevent the contract from paying out interest. Control is no longer in the hands of Compound the company but in the hands of the contract itself. This isn’t theoretical or promised. It’s live and working on the Ethereum network.

Maker DAI is another example of a live working smart contract. This contract is built to work like the Federal Reserve. They use a complex system of lending and locking of collateral to produce a stable asset, the DAI, which holds the same value as 1 USD. Currently DAI tokens are backed only by Ether but they’re working on adding other collateral as well. This contract is live and has been working since it launched. The price of DAI has remained stable even after the underlying collateral has lost 90% of it’s value!

I expect finance to be one of the most impacted by blockchain technology. 

#### Smart Contract Example 3 - Politics:
To get  an idea of how digital money and smart contracts will affect politics we can look at [this online fundraiser](https://secure.actblue.com/donate/donate-save-america2) for the 2018 US House races. The organizers found 10 candidates running for the US House of representatives where the races were close and media markets were cheap. You could donate money and they’d split the donations between the candidates. 
If this were a Dapp it could live track which media markets were the cheapest at the time you made your donation instead of having to choose the candidates upfront. You could even hook it up to pay fo the ads directly so you could actually watch which ads you were paying for. You could go even further and make the Dapp configurable based on policy. Want to see pot legalized? You could have it find the closest races where one candidate favors marijuana legalization and the other doesn’t. Scared you donation won’t be worth it? You could set up another Dapp that’s a marketplace for insurance on legislation. If something doesn’t pass, you get a payout. Or better yet [move elections themselves onto the blockchain](https://xkcd.com/2030/) and have candidates take the other side of those insurance contracts. If they don’t follow through on their campaign promises they automatically have to pay out. Would this be considered bribery? I’m not sure. This is all speculation but I use it as an example to show that having programmable money will have an effect on how the world works.


--

Some sell blockchain technology as a pancia: the idea being it will always favor what is good and fair. In the words of the Man in Black from the Princess Bride: “Life is pain, Highness. Anyone who says differently is selling something.” Currently a small group of people control the vast majority of the tokens which means they not only have immense wealth but immense control over how these networks work. Is that fair? They got in early, shouldn’t they be rewarded? If blockchain technology finds its way into politics should money have influence over group decision making? Is that even possible to prevent? I’m not writing this to argue one way or the other (although it’s probably clear what side of the debate I’m on) I’m writing to let people know the importance of this technology so they can think critically and make decisions of their own. With more voices in the blockchain space I’m confident it can in-fact have a net-positive impact on the world.

