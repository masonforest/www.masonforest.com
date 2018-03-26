---
layout: post
title:  "Why I'm building Ellipticoin Part II"
date:   2018-03-15
categories: blockchain ethereum
---

*Continued from [Why I’m building
Ellipticoin](http://www.masonforest.com/blockchain/ethereum/2018/03/15/why-im-building-ellipticoin.html)*

I’m back from the [MIT Bitcoin Expo](http://mitbitcoinexpo.org/) and I’m only
more convinced that Ellitpicoin needs to be built!

The most common question I got last weekend when I told people about Ellipticoin
was: “What sets your blockchain apart from Ethereum and other blockchains”

I’ve come to the conclusion that I want Elliptcoin to be the most developer
friendly third generation blockchain available. I got to talk to some of the top
dapp developers in the space and everyone I talked to agreed that smart contract
development is more painful than it has to be. Like I said in my last blog post
switching to the WebAssembly virtual machine will allow people to write smart
contracts in Rust, C, C++, Kotlin and any other languages that target
WebAssembly. Those languages have been around much longer than Viper and
Solidity and have much larger communities around them. After building out the
initial smart contracts for Ellipticoin in Rust I’ve realized how nice smart
contract development can be. Ethereum, EOS, Difinity and others also plan to
move WebAssembly virtual machines so developers will be able to build their
projects using the tools they like today and easily migrate between platforms in
the future.

When I told people that some would ask: “Okay, but why build your own blockchain
when Ethereum is switching to WebAssembly and Proof of Stake anyway?”

The short answer is: I think I can get there faster than they can. Ethereum does
have millions if not billions of dollars of funding and a massive team of some
of the best minds in the industry behind them but swapping the Virtual Machine and
Consensus algorithm out of a live network is immensely more difficult than
building something from scratch. Also Ethereum has a market cap of 50 billion
dollars so any change to the protocol must be taken with extreme care. I’ll be
able to move much more quickly. A WebAssembly virtual machine has [already been
built](https://github.com/paritytech/wasmi) and formally proven Proof of
Stake protocols have already been designed. All I have to do is put the pieces
together. I’m also in the process of researching [The Cosmos
SDK](https://github.com/cosmos/cosmos-sdk). The Cosmos SDK provides the
networking and consensus layers of a blockchain so all I’d need to build is the
application layer. I still need to figure out if it’s a good fit for
Ellipticoin. Either way, I expect to have something live by the end of the year.
 
Another question I got a few times about Part I of this blog post is: “Why
donate money to charity? Why not just let investors donate themselves?”


This is a good question and I’ve been thinking about it a lot. Last weekend’s
conference further confirmed to me that the blockchain revolution will be just
as much about as identity and politics as it will be technology. When someone
asked a panelist why we need interoperability if Ethereum can scale to support
all assets on a fully decentralized exchange he responded: [“have you met
Bitcoiners
before?”](https://www.youtube.com/watch?v=1wdPLsdHmvY&feature=youtu.be&t=6h19m53s).

People are loyal to groups they associate with. It’s clear from a evolutionary
biological perspective why we’ve come to be that way: being loyal to a group
that survives amongst other groups will increase the likelihood of the
individual’s survival as well. In the past people have been born into the groups
they associate with but recently people have been able to choose more and more
of the groups and tribes they belong to. People choose to be “Bitcoiners”
because they have a certain set of beliefs and values. Two of those values are
freedom and certainty. With Bitcoin you are free to spend your money anywhere
in the world and no person or government can stop you. There will also only ever
be 21 million bitcoins is existence so you can be certain your Bitcoins' value
won’t be lost to inflation. 

Sharing and giving is also a common trait of existing cultural groups
(governments, religious groups, etc). This makes sense from an evolutionary
biological perspective too. By joining a group and giving and sharing within
that group you’re anticipating that in the future the group will help you out
too if you need it. Wealth inequality is a serious problem in the world today
and if we don’t do anything about it it’s only going to get worse. To help curb
that trend I want sharing and giving to be part of the culture of Ellipticoin. I
intend for this to make people choose Ellipticoin over alternative Blockchains.

In [a study published by the Harvard Law School last
week](https://corpgov.law.harvard.edu/2018/03/23/corporations-and-the-culture-wars/)
researchers described how investors and other cooperate stake holders are being
asked to take a public stance on social issues. People, younger people
particularly, care about things like wealth inequality and climate change. They
understand that choosing which corporations to support has a significant impact
on the world. Choosing which blockchains to use to will have similar effects
which will make Ellipticoin more appealing to that group of people.

That said, I also don’t like controlling other people’s choices. If the
blockchain revolution is all about giving power back to the people why the heck
would I want to tell them what to do with their money? As a solution  to that
problem I’m planning on adding the concept of clans into the Ellipticoin
protocol. Clans will be identified a single word and a number (eg water-1). If
people don’t agree with the values of that clan they can fork and make their own
clan. This way if people would rather their money go elsewhere they can create
their own clan that’s set up differently. This way you can separate the cultural
pieces of blockchain while still allowing all clans to share the same technology
instead of having to repeatedly rewriting everything from scratch.

The concept of clans also solves some technical and governance problems as well.
First it solves the problem of name ownership. When a network forks you wouldn’t
have to fight over who gets “Ellipticoin” and who gets “Ellipticoin Cash” you
can just create a new clan. This also provides some protection against Proof of
Stake slipping into plutocracy. If the ruling majority that owns the majority of
the wealth isn't acting in the best interest of the users they can "revolt" and
create their own clan. The same goes for any corrupt governance. In designing
this system I want to figure out how to minimize centralization around anyone or
anything including myself. All humans are flawed and susceptible to corruption.
The best way to prevent against that is to deliver citizens and/or users the
best possible news and information so they can know when it’s time to elect new
leaders.

If you want to try out what I have so far you can install the [Ellipticoin
wallet](https://www.npmjs.com/package/ec-wallet), join the [Telegram
group](https://t.me/joinchat/F0_SEksYp5PhexXW6dQ-9A) and ask someone to send you
some testnet tokens!  I've deployed a [basic token
contract](https://github.com/ellipticoin/ellipticoin-base-contracts/blob/master/src/base_token.rs#L11)
and [human readable name
registry](https://github.com/ellipticoin/ellipticoin-base-contracts/blob/master/src/human_readable_name_registration.rs#L11)
written in Rust. Both have been compiled to WebAssembly and are being run in a
WebAssembly virtual machine by an [Ellipticoin Blacksmith
node](https://github.com/ellipticoin/ellipticoin-blacksmith-node). The next step
is setting up a way for users to deploy their own smart contracts and then I can
move on to implementing the consensus algorithm!

See you in Telegram!

-M
