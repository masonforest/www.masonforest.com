---
layout: post
title:  "How to Deploy a Custom Token to the Ethereum Blockchain using Ethel"
date:   2016-09-05 00:00:00
categories: ethereum
---

[Ethel](https://github.com/masonforest/ethel) is a framework I'm working on
for writing smart contracts in solidity along with a testing framework in
golang. In this tutorial I'm going to walk you though deploying a custom token
to the [Ethereum](https://ethereum.org/) blockchain.

First we'll need install the Mac OSX build tool [Homebrew](http://brew.sh/):

    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

and then install golang:

    brew install go

Now we can install `ethel`:

    go get github.com/masonforest/ethel/...

And initialize `ethel`. This will create us an account we can use to deploy our contract from.

    ethel init

It'll ask a few questions. I'd recommend encrypting your live key and not encrypting your testnet key but it's up to you. Once it's done it'll print out your new address and testnet address.

At this point you'll need to [install geth](https://github.com/ethereum/go-ethereum/wiki/Getting-Geth) and download the blockchain. Once geth is installed run:

    geth --fast --testnet --rpc

While it's syncing head over to [the ether.camp testnet explorer](https://test.ether.camp/) and click “Get Free Ether” up in the top left. Copy and paste your testnet address that was displayed when you ran `ethel init` and click “Get Ether”.

Once the testnet blockchain has synced (it might take a few hours) run:

    ethel balance --testnet


If you're impatient you can use my public testnet node for the remainder of the tutorial:

    ethel balance --testnet --host=`http://ethereum-testnet.masonforest.com:8545/`

Once you see a valid balance you're ready to deploy! First lets create a new project and `cd` into it:

    ethel new my_token
    cd my_token

this should create the following file structure:


    ├── contracts
    │   ├── token.sol
    │   └── token_test.go
    └── libraries
        └── token.sol


`Token.sol` is a simple ethereum token contract of written in [solidarity]()
You can test your contract by running:

    ethel test

If everything passes (it should), go ahead and deploy your contract:

    ethel deploy --testnet

    -> Contract pending deploy: 0x6b143788cbe2fbfdaee4fb7bb8101abe40eb2ef8
    -> Transaction waiting to be mined:0x981a0723899230a84bda8a15a3bc90d3873d2f82481d04c919dee97041635fac
    -> Deployed Token.
    -> Balance: 78 Ether

Copy the "Contract pending deploy" and paste it into the search bar on [http://testnet.etherscan.io/](http://testnet.etherscan.io/). You should see your contract deployed to the blockchain!


Once you've deployed you contract to the testnet blockchain you can try deploying it to the live network! To do this you'll have to run `geth` and `ethel deploy` without the `--testnet` flag. You'll also need to fund your account with Either. The latest version of `Mist` has a Coinbase integration which allows you to buy Ether with a credit card right inside the app. You can also buy Ether from [Coinbase](https://www.coinbase.com/buy-ether?locale=en) or [ShapeShift](https://shapeshift.io/).

Ethel is still in the early prototype phase. If you're looking for something more stable check out one of these similar projects:

* [Populus](https://github.com/pipermerriam/populus)
* [Truffle](https://github.com/ConsenSys/truffle)
* [Embark](https://github.com/iurimatias/embark-framework)

If you have any questions don't hesitate to reach out.

Thanks for reading!

-Mason




