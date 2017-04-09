---
layout: post
title:  "How to Build a Twillio Oracle on the Ethereum Blockchain"
date:   2017-04-01
categories: blockchain
---

At it's core Ethereum is a decentralized world computer. Like any computer it
needs inputs and outputs. One way to get data into our out of the blockchain
is something called an Oracle. Oracles are centralized services which other
smart contracts can interact with. One example is a Ethereum price Oracle. A
person could create price oracle and assert that the price of Ethereum is
$50/Ether. Other smart contracts could then act based on that value (possibly
a crowdfunding tool could refund everyone if the price dropped too low). An
issue with Oracles is that you're relying on one party or group to be honest.
There's nothing to stop the Oracle from saying that the price is $25. Ideally
we'll need Oracles less and less. For example, once an on chain trading
solution emerges we'll no longer need a price Oracle. Smart contracts will be
able reference the trading data directly. In the mean time as long as you trust them,  Oracles are a good stop
gap that allow smart contracts to interact with the real world in all sorts of
ways. In this tutorial I'll walk through setting up a [Twillio](https://www.twilio.com/) Oracle which
will allow smart contracts to send text messages.

Prerequites:

 - Mac OSX or a Unix based envionment and basic knowelege of the command line
 - [Yarn](https://github.com/yarnpkg/yarn)
 - [truffle](https://github.com/trufflesuite/truffle)
 - [geth](https://github.com/ethereum/go-ethereum/wiki/geth)
 - [testrpc]( https://github.com/ethereumjs/testrpc)


First we'll need to set up the smart contract that will recieve requests for text message deliveries. First initialize the project:

    $ mkdir twillio-oracle
    $ cd twillio-oracle
    $ truffle init

And remove the example contracts

    $ rm test/\* contracts/ConvertLib.sol contracts/MetaCoin.sol

Let's use [TDD](https://en.wikipedia.org/wiki/Test-driven_development) and start by creating a failing test:


    // test/twillioOracle.js

    var TwillioOracle = artifacts.require("./TwillioOracle.sol");

    contract('TwillioOracle', function(accounts) {
      it("logs when message request events are recieved", function(done) {
        TwillioOracle.deployed().then(function(twillio) {
          var events = twillio.Message().watch(function(error, result) {
            assert.equal(result.args.to, "+1555-555-5555");
            assert.equal(result.args.body, "Test");
            events.stopWatching();
            done();
          });

          twillio.createMessage("+1555-555-5555", "Test");
        })
      });
    });

For this Oracle well be using [Ethereum Events](http://solidity.readthedocs.io/en/develop/contracts.html#events) to trigger text messages. In our contract we'll have a function called `createEvent` which creates an Ethereum event. We'll also have a seperate server running watching for events. When an event is triggered, we'll send a text!

Before testing well need to update the migration file:

    # migrations/2_deploy_contracts.js
    var TwillioOracle = artifacts.require("./TwillioOracle.sol");

    module.exports = function(deployer) {
      deployer.deploy(TwillioOracle);
    };

In a seperate terminal window run:

    testrpc

[TestRPC](https://github.com/ethereumjs/testrpc) is a fake Ethereum server
used for testing and developing smart contracts without needing to pay money
for real ether or waiting for the blockchain to sync.

Now you can run:

    truffle test

You should get:

    Could not find artifacts for ./TwillioOracle.sol from any sources

Let's create a basic contract

    # contracts/TwillioOracle.sol
    pragma solidity ^0.4.4;

    contract TwillioOracle {
    }

And run:

    truffle test

Now you should get:

    Uncaught TypeError: twillio.Message is not a function

Next add our `createMessage` function

    # contracts/TwillioOracle.sol

    pragma solidity ^0.4.4;

    contract TwillioOracle {
        event Message(string to, string body);

        function createMessage(string to, string body) {
            Message(to, body);
        }
    }

And your tests should pass!

    Contract: TwillioOracle
      ✓ logs when message request events are recieved


    1 passing (58ms)

Let's pick this contract apart a bit. First we define the `Message` event. A
message event will have 2 properties `to` (the phone number to send the
message to) and `body` (the message body of the text).



 
Now you're ready to deploy.

Kill `testrpc` and start `geth` instead:

    geth --rpc

If you want to run both at the same time you can update your
[truffle.js](http://truffleframework.com/docs/advanced/configuration#networks) accordingly.


You'll need to start by unlocking your account:

    $ geth attach
    >  personal.unlockAccount(eth.accounts[0], "password")

Also, until [this gas limit bug](https://github.com/trufflesuite/truffle/issues/271) is resolved you'll need to set gas=300000 in your truffle configuration file.


    # truffle.js

    module.exports = {
      networks: {
        development: {
          host: "localhost",
          port: 8545,
          gas: 300000,
          network_id: "*" // Match any network id
        }
      }
    };

Then you can migrate:

    truffle migrate


We now have a contract deployed to mainnet!

Finally we'll need to add a seperate server for watching for events and sending texts:

Here's the that:

    // ./watcher.js
    var Twilio = require('twilio');
    var twillio = new Twilio.RestClient(process.env.TWILLIO_SID, process.env.TWILLIO_AUTH_TOKEN);
    var Web3 = require('web3');
    var web3 = new Web3(new Web3.providers.HttpProvider(process.env.ETHEREUM_RPC_URL));
    var fs = require('fs');

    fs.readFile('build/contracts/TwillioOracle.json', (error, json) => {
      var json = JSON.parse(json);
      var contract = web3
        .eth
        .contract(json.abi)
        .at(process.env.CONTRACT_ADDRESS);

      contract.Message().watch(function(error, event) {
        twillio.messages.create({
          body: event.args.body,
          to: event.args.to,
          from: process.env.TWILLIO_FROM_NUMBER,
        }, function(err, message) {
          console.log("Sent:" + message.sid);
        });
      });
    });

First we read the
[ABI](https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI) from the
contract build directory. We use the ABI to tell
[web3](https://github.com/ethereum/web3.js/) how to interact with the
contract. We then watch for `Message` events and trigger Twillio to send messages when those events occur.

You can run the watcher server like so, getting `TWILLIO_SID`, `TWILLIO_AUTH_TOKEN` and `TWILLIO_FROM_NUMBER` from the [Twillio Console](https://www.twilio.com/console) and the `CONTRACT_ADDRESS` from the results of running `truffle migrate`:

    TWILLIO_SID=XXX TWILLIO_AUTH_TOKEN=XXX CONTRACT_ADDRESS=0xxxxxxx TWILLIO_FROM_NUMBER=XXX  ETHEREUM_RPC_URL=http://localhost:8545/ node watcher.js

You now have a fully functioning Oracle!

When you call `sendMessage` it triggers an Ethereum event. Our Oracle is
watching for that event and when it's triggered it delivers the SMS for us!

Next you'll probably want to make the `sendMessage` function payable so people
can't send unlimited texts though you Oracle. I'll leave that as an excerise
for the reader.


Happy Oracle Building!
