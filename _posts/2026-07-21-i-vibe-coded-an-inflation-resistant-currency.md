---
layout: post
title:  "I Vibe Coded an Inflation-Resistant Currency"
date:   2026-07-21
categories: blockchain cryptocurrency bitcoin-dance
---

In 2017 I quit my job to pursue a career in cryptocurrency. I had high hopes. Nearly a decade later, crypto is still barely functional.

Last summer a friend helped me rebuild my dock. It took an entire afternoon just to get him set up with a Bitcoin wallet so I could send him some Bitcoin for his trouble. This summer, he has half as much "money" in that wallet as when I sent it.

In order for crypto to succeed it's going to need to pass what I call the **"send me five bucks test"**: if you have an iPhone, I should be able to send you five bucks without you downloading or installing anything. The transaction should settle in under a couple of seconds and cost a penny or two in fees, at most.

I vibe coded [bitcoin.dance](https://bitcoin.dance) to prove this is possible.

## What is Money

Money has three functions: a unit of account, a store of value, and a medium of exchange. Here's how bitcoin.dance handles each one.

### Unit of account

The unit of account in bitcoin.dance is pegged to the US dollar — but every account continuously accrues interest. This wasn't possible before cryptocurrency. If you have $100 in your pocket, there's no mechanism by which it becomes $100.01 by tomorrow. In bitcoin.dance, there is.

This is what makes the currency inflation resistant. As the dollar inflates, user balances grow to compensate. You still think and transact in dollars — the unit everyone already understands — but your money isn't quietly bleeding value while it sits there.

### Store of value

Under the hood, bitcoin.dance uses USDC as its store of value. USDC is a token, issued on various blockchains, that's backed by US dollar deposits in banks and managed by Circle.

To make that USDC earn interest, it's deposited into the [Steakhouse Prime USDC Morpho vault](https://app.morpho.org/base/vault/0xbeef0e0834849aCC03f0089F01f4F1Eeb06873C9/steakhouse-prime-usdc#overview). Morpho vaults are essentially banks on the blockchain: you deposit tokens, and you earn yield. That yield is what funds the continuously growing balances described above. And because it's your wallet, you can withdraw your USDC at any time.

### Medium of Exchange

Medium of exchange is the fuzziest of the three when translating to crypto. With traditional currency, the medium of exchange is the physical "stuff" you hand over — the bills, the coins, the swiped card. The closest crypto equivalent is the blockchain where transactions actually execute.

bitcoin.dance settles transactions on Base, a modern, low-cost blockchain. That's what makes the "send me five bucks test" passable: transfers confirm in seconds and cost fractions of a cent, instead of the slow, expensive settlement layer such as Bitcoin.

## The Fourth Function: Authentication

Cryptocurrencies have to serve a function that has no parallel in traditional currency: **authentication**. There needs to be a mechanism to verify that Alice is Alice and Mallory is Mallory, so Mallory can't spend Alice's tokens.

Blockchains solve this with private keys, but today most people store private keys in a hardware wallet or a browser extension — and both add exactly the kind of setup friction that fails the five bucks test. bitcoin.dance instead stores the user's seed phrase in their browser's password manager: infrastructure nearly everyone already has, already syncs across devices, and already trusts with their bank login. No new hardware, no new extension, no afternoon lost to setup.

## All currency is an options contract

Step back far enough and all currency is essentially an options contract. Before the dollar went off the gold standard, a dollar was a contract stating that the holder could trade it for an equivalent amount of gold at their local bank.

That contract required two layers of trust. First, faith that gold would hold its value — reasonable enough, since gold is scarce, pretty, and has been prized by humans for centuries. Second, trust that the bank would actually have the gold and hand it over when you showed up with the dollar.

When the dollar left the gold standard, it didn't lose 100% of its value. People keep trading dollars because they know other people will accept them for goods and services. The trust just moved from the vault to the network of people willing to take the paper.

## Unbundling money

Technology has a habit of bundling and unbundling goods and services. When TV and radio were the only ways to consume content, nearly everyone got the same bundle. As the internet grew, content unbundled — you chose which blogs and sites to read.

Cryptocurrency does the same thing to the functions of money. USDC, for example, uses the US dollar as its unit of account, US bank deposits as its store of value, and the Ethereum network as its medium of exchange. Three functions, three different providers.

bitcoin.dance bundles differently: USD is the unit of account, USDC deposited in an interest-bearing Morpho vault is the store of value, and a fast, cheap blockchain is the medium of exchange. These combine together to pass the five bucks test.

## Try it yourself

Go to [bitcoin.dance](https://bitcoin.dance), open the hamburger menu, and hit the faucet to issue yourself a hundred bucks (testnet only, sadly). You will see interest accrue slowly over time. Generate a link and send five bucks to yourself. Then send some to a friend. Send some to your grandma.
