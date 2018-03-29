---
layout: post
title:  "Scaling Ellipticoin to thousands of transactions per second and beyond"
date:   2018-03-15
categories: blockchain ethereum ellipticoin
---

On February 16th  2018 the Ethereum Community fund was [announced](https://techcrunch.com/2018/02/16/ethereum-community-fund/), over $100
million dollars to be given as grants to fund development of blockchains that
scale. The race for scalable blockchain infrastructure has begun. I originally
started Ellipticoin because I didn’t like how much energy Proof of Work burns
and I wanted to use existing tools to build smart contracts and Dapps. But I
realize building a blockchain that scales is also desperately needed in the
space. The Ellipticoin network isn’t live yet but I do have enough built to
start benchmarking the Virtual Machine. This post is about how I got the
Ellipticoin Virtual Machine to scale from 10 transactions per second (slower
than the current iteration of the Ethereum Network) to 4 thousand transactions
per second.


The first thing I did was I set up a [simple benchmark
test](https://github.com/ellipticoin/ellipticoin-blacksmith-node/blob/master/lib/mix/tasks/benchmark.ex#L7-L15)
in the [Ellipticoin Blacksmith
Node](https://github.com/ellipticoin/ellipticoin-blacksmith-node). I used
[Benchee](https://github.com/PragTob/benchee) which runs code repeatedly for a
set interval of time and outputs statistics on how long each run took. The
benchmark is pretty simple. First it loads up the “sender” with `1000000000`
tokens. Then benchee runs the transfer function repeatedly for 1 second. Once
the time is up Benchee prints out some statistics about how long each transfer
took. For good measure I printed out the balance of the recipient at the end
just to make sure they got all their tokens.

The [token
contract](https://github.com/ellipticoin/ellipticoin-base-contracts/blob/master/src/base_token.rs)
I ran is written in Rust and compiled to WebAssembly. It does everything you’d
expect a basic token contract to do. First it checks if the sender has enough
tokens to send. If they do it subtracts the amount from the sender's balance and
adds it to the recipient's balance. If the sender doesn't have enough tokens it
returns an error.

When I ran then benchmark before any optimization each iteration took on average
97.7 milliseconds which comes out to 10.24 iterations per second on average.
Currently the Ethereum network processes about 15 transactions per second so I
still had a ways to go.


    ellipticoin-blacksmith-node master % mix benchmark

    Name                   ips       average     deviation  median         99th %

    base_token_transfer   10.24     97.70 ms     ±1.65%    98.02 ms      100.13 ms


    Receiver's balance after benchmark 10


The code running inside the VM is pretty simple. It does one comparison between
integers (checking the senders balance) and updates two values in the state
database (backed by [Rocksdb](http://rocksdb.org/)). A lot of this time was spent reading the the
contract code and initializing the virtual machine. Running these operations
natively would be much quicker. So that’s what I did.

If we know the inner logic of a contract and know what incoming calls to that
contract look like we can intercept the calls before the VM is started and run
the logic natively.

Erlang and therefore Elixir have a fast and powerful feature called [binary
matching](https://hexdocs.pm/elixir/Kernel.SpecialForms.html#%3C%3C%3E%3E/1-binary-bitstring-matching).
This was perfect for matching against only the transactions I wanted and at the
same time extracting all information relevant to that transaction.

[The
code](https://github.com/ellipticoin/ellipticoin-blacksmith-node/blob/master/lib/vm.ex#L45-L59)
looks a little funny but what it’s doing is pretty simple. Basically it’s
matching against the transaction data and if the transaction contains a certain
string of bytes then we know it’s a call to the “transfer” function. If it is, in fact, a call to the transfer function, then run the logic of the transfer function natively, otherwise, we will need to run the transaction in the external VM.


That gave a significant improvement. Transactions ran in 1.38 milliseconds on
average which give us 724 runs per second.


    Name                   ips          average   deviation   median      99th %
    base_token_transfer   724.65        1.38 ms   ±55.42%     1.24 ms     6.41 ms

Pretty good!

We were still reading and writing to disk in each iteration of the transfer
function though. This could be improved. It would be much faster if we could be
reading and writing to memory instead. So that’s what I did.

It was actually pretty easy to switch from storing state on disk to storing it
in memory as well. All I had to do was swap the read and write functions from
Rocksdb to [Redis](https://redis.io/). Now you might be saying: “Mason, you can’t store blockchain
state in Redis it won’t have strong persistence guarantees.” You’d be right that
Rocksdb has much stronger guarantees that you won’t lose data in case of a power
outage. But if a blockchain is spread across hundreds or thousands of machines
you can rely on other nodes to recover from catastrophic failure. Storing data
in memory is also a concerning for centralization. Memory is much more expensive
than disk space and it’s possible that the blockchain size gets so large that
you wouldn’t be able to run a full node on a consumer laptop.  This is a
tradeoff I’m willing to make. Currently you can rent a `r4.2xlarge` from AWS for
under $400/mo which has 61 gigabytes of RAM. Stakers could also charge higher
[rent
fees](https://www.ethnews.com/ethereum-developers-talk-rent-fees-for-mainnet-smart-contracts)
to offset that cost. I do want Ellipticoin to be sufficiently decentralized but
I’m willing to make certain decentralization tradeoffs in favor for performance.
Currently I think the decentralization tradeoff is worth it for storing state in
memory.

So how fast is?

Pretty fast:

    Name                    ips        average   deviation   median    99th %
    base_token_transfer   4.82 K      207.31 μs   ±8.74%     203 μs   285.17 μs


207.31 μs per transaction gives us 4.82 K  transaction per second which is
getting toward where we need to be for a scalable blockchain. If you'd like to
download and run these benchmarks yourself you can clone the [blacksmith
node repo](https://github.com/ellipticoin/ellipticoin-blacksmith-node) and run
`mix benchmark`.


I think the main bottleneck is at this point is Elixir’s
[GenServer](https://elixir-lang.org/getting-started/mix-otp/genserver.html)
which is currently being called every time a transcation is run. Theoretically I
could rewrite the Blacksmith node in Rust  at which point our bottleneck would
probably be Redis. 🚀😳

Note that these transaction speeds are only for optimistic native functions that
will have to be built into the nodes. I’d imagine to start you could build in
native functionality for all ERC20 tokens. Other native contracts could be added
as well. If the original smart contracts are written in Rust this shouldn’t be
too hard though. If the node is  written in Rust it could even share the same code!

I still have a long way to go on getting the Ellipticoin network up and running
and more research to do on scaling, but I’m happy to be sharing my findings. I
think the idea of optimistic native functions are an easy way to get a big
performance boost on commonly used transaction types at little cost. I’m still
not sure storing blockchain state in memory is the best way to go, but the
performance gains definitely make it worth considering. I’ve also been thinking
about having a “hot chain” that’s stored in memory that could be used for
trading and what not and a “cold chain” that’s backed by hard drives that can be
used as a store of value. If you have any feedback on these ideas or anything
else come say hi in the [Ellipticoin
Telegram Room](https://t.me/joinchat/F0_SEksYp5PhexXW6dQ-9A)!

Back to work…

-M
