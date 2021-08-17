---
layout: post
title:  "Moonshine is Moving to Polygon"
date:   2021-08-17
categories: blockchain ethereum moonshine
---

Moonshine Liquidity Rewards are moving to the Polygon Network at block #6359375 which will be mined around August 28th 2021. This will allow for 10% of total issuance to be distributed in the remainder of the  first era.


How To Migrate
==============
The migration process should take about 15-20 minutes depending on congestion on Ethereum.

To migrate you'll have to do the following:

1. Click remove liquidity, select the token you'd like to remove, enter 100% and click   
"Remove liquidity"

2. Click "Withdraw" and enter the amount of tokens you'd like to withdraw

3. "USD" on the existing platform is backed by CDAI. If you withdraw cDAI you'll need to withdraw it from Compound. To do that visit [https://app.compound.finance/](https://app.compound.finance/) and click DAI under Supply Markets, then click "Withdraw," "Max" and "Withdraw"

4. Visit [https://wallet.matic.network/](https://wallet.matic.network/) click "Move Funds from Ethereum to Polygon", enter the amount you'd like to move to Polygon and click "Transfer"

5. If you are moving DAI you'll need to convert it to USDC if you're moving renBTC you'll need to convert it to WBTC. You can use Moonshine on Polygon to do just that! Visit app.moonshine.exchange select the input and output tokens, click "Max" and  click "Convert".

6. Finally you'll need to add your tokens to the pool. Click "Pool" enter the amount of tokens you'd like to add and click "Add Liquidity" At this point you should be earning tokens again!

Why We're Migrating
===================
Migrating Moonshine to Polygon will let us offload decentralization and scaling concerns to the Polygon network so we can focus on building liquidity and creating the best possible user experience for traders and liquidity providers.

Future plans
============

After building up BTC and ETH liquidity we plan to add an on chain order book so active liquidity providers can maximize capital efficiency. The user experience for retail traders will be the same and trades will be routed through either the AMM or the order book depending on price. I've also looked into building an application specific Optimistic Rollup chain. This would allow for transactions to be executed in real time. Transactions costs on Polygon are sufficiently low but transactions still can take a few seconds to confirm.  The chain could be operated by multiple sequencers that are elected based on Moonshine stake or burn which ends up being a similar consensus mechanism as the prototype chain. The end goal of Moonshine is to offer realtime order fulfillment for a fraction of a penny a trade.
