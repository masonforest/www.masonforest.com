---
layout: post
title:  "How to deploy an ERC20 token in 20 minutes"
date:   2017-11-13
categories: blockchain ethereum
---

Ethereum tokens are all the rage these days. These tokens can represent units of
value in the real world: [Gold](https://digix.global/),
[Whoppers](https://www.cnbc.com/2017/08/28/burger-king-russia-cryptocurrency-whoppercoin.html),
[Kittens](https://www.cryptokitties.co/) and even something similar to [shares
of a
company](https://www.investopedia.com/terms/i/initial-coin-offering-ico.asp).
People have raised over [$2 billion in token sales](https://www.forbes.com/sites/chancebarnett/2017/09/23/inside-the-meteoric-rise-of-icos/#57ac46d95670) thus far. These tokens have
been standardized in the
[ERC20](https://theethereum.wiki/w/index.php/ERC20_Token_Standard) standard so they can be easily traded between wallets. In this
tutorial I'm going to walk you through deploying your own ERC20 token to the
live Ethereum network.

Here's what you'll need to get started:
--------------------------------------

* A basic text editor. ([Atom](https://atom.io/) is good. I like
  [Vim](http://www.vim.org/))
* A basic understanding of the command line and a terminal emulator. The built
  in mac app "Terminal" should be fine. I like [iTerm2](https://www.iterm2.com/)
* The Chrome Web Browser
* [Node.js 8](https://nodejs.org/)
* A name for your token. Mine is going to be called HamburgerCoin

The first thing you'll need to do is install [MetaMask](https://metamask.io/).
Go to the Metamask website and click on "Get Chrome Extention".

Metamask will allow you to make transactions on Ethereum through Chrome. They
rely on [Infura](https://infura.io/) who run public Ethereum nodes so you don't
have to. If you're feeling adventurous you could download and install
[Mist](https://github.com/ethereum/mist/releases) instead. When you run Mist
you're running your own Ethereum node. If you run your own node you'll have to
sync the network to your computer which can take a while. That's
technically safer because you don't have to trust Infura with your
transactions. Infura could censor your transactions by just ignoring them but they wouldn't be able
to steal your money. Since installing Metamask is quicker and simpler than
running Mist I'm going to assume we're using Metamask for the rest of the
tutorial.

Next you'll need to install [truffle](http://truffleframework.com/):

     $ npm install -g truffle

Now create a new directory for your new coin, CD into it and initialize your
truffle project.

    $ mkdir hamburger-coin
    $ cd hamburger-coin
    $ truffle init

Congrats your truffle project is now set up!

Now let's create our coin. First we'll need to install the
[OpenZepplin](https://github.com/OpenZeppelin) framework. The OpenZepplin
Framework contains a bunch of prebuilt contracts including the ERC20 token
contracts that we want to deploy.

(Just keep pressing return to use the defaults)

    $ npm init
    package name: (hamburger-coin)
    version: (1.0.0)
    description:
    entry point: (truffle.js)
    test command:
    git repository:
    keywords:
    author:
    license: (ISC)
    About to write to /Users/masonf/src/hamburger-coin/package.json:

    {
      "name": "hamburger-coin",
      "version": "1.0.0",
      "description": "",
      "main": "truffle.js",
      "directories": {
        "test": "test"
      },
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "author": "",
      "license": "ISC"
    }


    Is this ok? (yes) yes
    $ npm install zeppelin-solidity

Now we can create our token contract. Open up `contracts/HamburgerCoin.sol` and
add the following:


    pragma solidity ^0.4.18;
    import "zeppelin-solidity/contracts/token/StandardToken.sol";

    contract HamburgerCoin is StandardToken {
      string public name = "HamburgerCoin"; 
      string public symbol = "HBC";
      uint public decimals = 2;
      uint public INITIAL_SUPPLY = 10000 * (10 ** decimals);

      function HamburgerCoin() public {
        totalSupply = INITIAL_SUPPLY;
        balances[msg.sender] = INITIAL_SUPPLY;
      }
    }

The OpenZepplin `StandardToken` is a standard ERC20 token.
If you're interested take a look at the [source
code](https://github.com/OpenZeppelin/zeppelin-solidity/tree/master/contracts/token) to see how it works!

It's actually not too complicated. The contract has a map of [addresses to
balances](https://github.com/OpenZeppelin/zeppelin-solidity/blob/master/contracts/token/BasicToken.sol#L15).
It also has a list of [allowed
transfers](https://github.com/OpenZeppelin/zeppelin-solidity/blob/master/contracts/token/StandardToken.sol#L17).
Think of these like checks. You can write a check but the money doesn't
transfered
until it's cashed.

If someone wants to transfer some funds you call
[approve](https://github.com/OpenZeppelin/zeppelin-solidity/blob/master/contracts/token/StandardToken.sol#L48) on the contract with the amount of tokens you want to send. This is like writing a check.

Then you can call
[transferFrom](https://github.com/OpenZeppelin/zeppelin-solidity/blob/master/contracts/token/StandardToken.sol#L26) which will actually run the transfer.

We could have written these contracts all ourselves but it's better to rely on
well tested community software at this point. Writing an ERC20 token from scratch is for a different blog post.

Try running compile to make sure our contract compiles:

    $ truffle compile
    Compiling ./contracts/HamburgerCoin.sol...
    Compiling zeppelin-solidity/contracts/math/SafeMath.sol...
    Compiling zeppelin-solidity/contracts/ownership/Ownable.sol...
    Compiling zeppelin-solidity/contracts/token/BasicToken.sol...
    Compiling zeppelin-solidity/contracts/token/ERC20.sol...
    Compiling zeppelin-solidity/contracts/token/ERC20Basic.sol...
    Compiling zeppelin-solidity/contracts/token/MintableToken.sol...
    Compiling zeppelin-solidity/contracts/token/StandardToken.sol...
    Writing artifacts to ./build/contracts
Next you'll need to add a migration file which will tell truffle how to deploy
your contract.

Next we need to add a truffle
[Migration](http://truffleframework.com/docs/getting_started/migrations).

Create migrations/2_deploy_hamburgercoin.js and add the following:

    var HamburgerCoin = artifacts.require("./HamburgerCoin.sol");

    module.exports = function(deployer) {
      deployer.deploy(HamburgerCoin);
    };

Now let's configure truffle to be able to deploy
using the Infura public nodes. If we're going to deploy to a public node we'll
need to the private keys to our wallet. We could just include those keys in our
source code but then if anyone got access to the source code they'd be able to
steal all of our Hamburger Coins! 😮 To prevent this we'll use the
[dotenv](https://github.com/motdotla/dotenv) node module.

Let's install all of the modules we'll need for deploying to Infura.

    npm install --save-dev dotenv truffle-wallet-provider ethereumjs-wallet

Now edit `truffle.js` and add the following:

    require('dotenv').config();
    const Web3 = require("web3");
    const web3 = new Web3();
    const WalletProvider = require("truffle-wallet-provider");
    const Wallet = require('ethereumjs-wallet');

    var mainNetPrivateKey = new Buffer(process.env["MAINNET_PRIVATE_KEY"], "hex")
    var mainNetWallet = Wallet.fromPrivateKey(mainNetPrivateKey);
    var mainNetProvider = new WalletProvider(mainNetWallet, "https://mainnet.infura.io/");

    var ropstenPrivateKey = new Buffer(process.env["ROPSTEN_PRIVATE_KEY"], "hex")
    var ropstenWallet = Wallet.fromPrivateKey(ropstenPrivateKey);
    var ropstenProvider = new WalletProvider(ropstenWallet, "https://ropsten.infura.io/");


    module.exports = {
      networks: {
        development: {
          host: "localhost",
          port: 8545,
          network_id: "*" // Match any network id
        },
        ropsten: {
          provider: ropstenProvider,
          // You can get the current gasLimit by running
          // truffle deploy --network rinkeby
          // truffle(rinkeby)> web3.eth.getBlock("pending", (error, result) =>
          //   console.log(result.gasLimit))
          gas: 4600000,
          gasPrice: web3.toWei("20", "gwei"),
          network_id: "3",
        },
        mainnet: {
          provider: mainNetProvider,
          gas: 4600000,
          gasPrice: web3.toWei("20", "gwei"),
          network_id: "1",
        }
      }
    };

Next we'll need to get our private keys from Metamask:

    1. Click on the fox icon on the top right of your Chrome window.
    2. Click on the ellipses that are to the right of "Account 1"
    3. Click "Export Private Key"
    4. Confirm you password
    5. Click the text to copy your private key to you clipboard.

Next open your up ".env" and paste in your private key like so (Your private
keys should be the same on both Ropsten and Mainnet):

    ROPSTEN_PRIVATE_KEY="123YourPrivateKeyHere"
    MAINNET_PRIVATE_KEY="123YourPrivateKeyHere"

Next let's deploy to the Ropsten Ethereum Test network.

The Ethereum test networks are networks which you can use to test your contracts.
There's also [Kovan](https://kovan-testnet.github.io/website/) and
[Rinkeby](https://www.rinkeby.io/). I chose Ropsten for this tutorial because
it's the easiest to get Ropsten ETH at the moment. All are fairly
similar and you can use
whichever testnet you like but for the remainder of the tutorial I'll assume
you're using ropsten. Visit
[https://faucet.metamask.io/](https://faucet.metamask.io/) to request some test ETH. Once you get
some ETH from the faucet you should be ready to deploy!

    $ truffle deploy --network ropsten
    Compiling ./contracts/HamburgerCoin.sol...
    Compiling ./contracts/Migrations.sol...
    Compiling zeppelin-solidity/contracts/math/SafeMath.sol...
    Compiling zeppelin-solidity/contracts/token/BasicToken.sol...
    Compiling zeppelin-solidity/contracts/token/ERC20.sol...
    Compiling zeppelin-solidity/contracts/token/ERC20Basic.sol...
    Compiling zeppelin-solidity/contracts/token/StandardToken.sol...
    Writing artifacts to ./build/contracts

    Using network 'ropsten'.

    Running migration: 1_initial_migration.js
      Deploying Migrations...
      ... 0xc2bbe6bf5a7c7c7312c43d65de4c18c51c4d620d5bf51481ea530411dcebc499
      Migrations: 0xd827b6f93fcb50631edc4cf8e293159f0c056538
    Saving successful migration to network...
      ... 0xe6f92402e6ca0b1d615a310751568219f66b9d78b80a37c6d92ca59af26cf475
    Saving artifacts...
    Running migration: 2_deploy_contracts.js
      Deploying HamburgerCoin...
      ... 0x02c4d47526772dc524851fc2180b338a6b037500ab298fa2f405f01abdee21c4
      HamburgerCoin: 0x973b1a5c753a2d5d3924dfb66028b975e7ccca51
    Saving artifacts...

The line above "Saving aritfacts" will have be the new address of your contract!

Copy and paste the address into the [Ropsten Etherscan
search box](https://ropsten.etherscan.io/) and you should see your newly
deployed contract!

You should now be able spend your tokens with any ERC20 compatible wallet
like [Mist](https://github.com/ethereum/mist) or
[MyEtherWallet](https://www.myetherwallet.com/).

For this tutorial I'll walk you though using a wallet I built called
[Etherface](http://etherface.io/).

First add your token to Etherface:

    1. Visit http://etherface.io/.
    2. Make sure you've selected "Ropsten" as the network in Metamask
    3. Click "Tokens"
    4. Click the "Plus" icon in the top right
    5. Enter the contract address from above

If you have a friend who wants some Hamburger tokens you can send them now! If
not here's how you can test out transferring them between two of your own
accounts:

    1. Click the "Switch Accounts" button in metamask (it's on the top right)
       and change your account to "Account 2"
    2. Click the ellipsis next to "Account 2" and select "Copy Address to
       clipboard"
    3. Switch back to "Account 1" *This is important, if you don't the
       transaction will fail*
    4. Click "Send" under your balance in Etherface
    5. Paste in the address of "Account 2"
    6. Enter an amount to send
    7. The Metamask confirmation box should pop up. Click "Submit"
    8. Wait ~15-30 seconds
    9. You balance should be reduced. "Account 2" now has some Hamburger coins!




Now you're ready to  deploying to mainnet!

    $ truffle deploy --network mainnet

You should be able to add your token to Etherface as before and start sending
people your newly minted tokens!
