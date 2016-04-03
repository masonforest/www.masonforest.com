---
layout: post
title:  "How Bitcoin actually works"
date:   2016-04-02
categories: bitcoin
---


It's odd thinking that cryptocurrency is going to be the most disruptive
technology we see in our lifetime but still realizing most people still think
it's a joke. Imagine you knew about the internet 10 years before and you had
to explain to people how transformative and influential it would be. You'd
sound pretty crazy. Over a billion dollars has been invested into Bitcoin
startups and the value of Bitcoin and all other cryptocurrencies in
circulation is over 8 billion dollars. This is going to happen. Currency is
just the beginning. The technology underlying Bitcoin is going change the way
business works. I don't expect non-techincal people to understand exactly how
it all works but I do think it would be valuable for people to know the
basics and why this break though is so important. For that reason I've tried
my best to explain how Bitcoin works below.

The goal of Bitcoin is to be able to transfer money from one person to
another without having to trust a middleman like a bank or a credit card
company in between. When we swipe our card at the grocery store we take it
for granted that our money is being transferred out of our account  and into
the grocery store's bank account. What's actually happening is when you swipe
your card the credit card authorizes the sale. Then later the credit card
company makes the actual transfer from its account to the grocery store's.
This has worked so far but there are a few issues with it.

  1. Credit card companies usually charge about 2% on these transactions.
That might not seem like a lot but it adds up.
  2. It makes it infeasible to send < $1.
  3. The Federal Reserve controls the creation of money. This
allows the US government to basically print money, devaluing the US dollar,
and give that money to big banks. They've done this so much that every
citizen in the US (including babies) is over $30,000 in dept to the Fed. Good
video about that [here](https://www.youtube.com/watch?v=Oe0fGXzKb1o).

Okay, so how do we build a system where babies don't get 30 Gs stolen from
them?

For a long time we've been able to digitally "sign" messages using computers.
The signature is a bunch of letters and numbers that you can run through a
program that will verify that the message could only have been sent by that
person.  So I could write the message "My name is Mason" and "sign" it using
this program. Other people could then run another program to check that I was
the one who signed the message and it wasn't somebody pretending to be me.
It's called [public key
cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) and it's
only used my the nerdiest of nerds. Great! Maybe we can use this to create
our new digital money.

Let's call our new money Boopcoin. Say Alice wants to send Bob one boopcoin.
She can write the message "Alice pays Bob one boopcoin" and send it to Bob.
Bob can verify that Alice and only Alice sent that message. Clearly this
system sucks. Alice can sign and send as many messages as she wants to as
many people as she wants. This is far from a working system but at least the
people receiving the messages can verify who sent them. This is a start and
we'll need it for the rest of the system to work.

Okay, now let's try to make it so people don't have unlimited money. First we
need assign a number to every transaction. So let's say "Bob pays Charlie one
boopcoin" is transaction #3. Next, whenever we sign a transaction we include
the number of the previous transaction. The message would now be "Bob pays
Charlie one boopcoin that he got from transaction #2" Charlie can now verify
that Bob sent him the message BUT he can also go back and look at Transaction
#2 and verify that Transaction #2 was sent to Bob. Transaction #2 says "Alice
payed Bob 1 boopcoin from transaction #1." You can follow this back all the
way to the beginning to where the first Boopcoin is created. We'll get into
how the first coin gets created later. Notice now, if Bob tries to pay
Charlie again with the same message: "Bob pays Charlie one Boopcoin that he
got from transaction #2." Charlie can say: hey man! You already payed me that
boopcoin! Bob can now only send One boopcoin for every boopcoin he receives.
We're getting closer!

We still have some issues. How do we make it so Bob can't "send" his boopcoin
to multiple people. From the last example, again imagine Bob sends the
message "Bob pays Charlie one boopcoin that he got from transaction #2" but
after that he also sends the message "Bob pays Doris one boopcoin that he got
from transaction #2." Both people think they got paid the same boopcoin and
that's no good. If there was some central place where all of the transactions
were recorded it would prevent this from happening. If that were the case
Doris could look and see that Bob already sent the coin he got from
transaction #2 to Charlie. We need some place we can keep a list of all the
transactions that have happened. That way people receive a message they  can
check the list and make sure that boopcoin has not already been sent to
someone else. This is the fundamental problem Bitcoin solves.

We could elect one person to be the "banker" per say that has the master list
of transactions. As we've seen before bankers aren't necessarily trust worthy
though. Maybe we could elect a group of people to be the bankers? This
doesn't work either because who controls the voting process? What's to stop
people from cheating and voting a bunch of times? The solution is called [proof of
work](https://en.bitcoin.it/wiki/Proof_of_work).

For proof of work to work first you need a math problem that would take a lot
of guessing to solve but it's very easy to check that you have the right
answer. With this, other people can quickly verify that you did lots of
guessing. This might make more sense with the real world example of how
Bitcoin works.

In Bitcoin a group of transactions is called a block. We can add a block of
transactions to the master list of transactions by "mining" that block.

Here's how that works: First, we'll need a way to convert the group of
transactions into one single number. One way to do that might be giving every
letter in the message to a number ie a=1 b=2 etc and then adding all the numbers together. Let
say are we convert 3 transactions we want to add to the list to a number and
we get 357 (in reality this number would be a lot bigger). Cool.

Now we start mining the block. The goal of mining is to guess the answer to a
math problem over and over again until you get the "right" answer. Before
hand everyone agrees on the equation and the "right" answer. Let's say the
right answer is 1 and the equation is:

"Multiply the number you got from step 1 by a random number and whatever is
in the 1's column is your answer"

Whoever find's the correct random number first wins!

Okay lets try some random numbers!

  * 357 * 21 = 79 => 9
  * 357 * 7 = 2142 => 2
  * 357 * 13 = 4641 => 1 we win!


We now send this out to everyone else trying to solve the puzzle. By solving
the puzzle it gives us the authority to add the transactions we were mining
to the master list.

One more slight detail if you're still following: The math problem is also
dependent on the answer to the previous math problem. So the math problem
might look more like

"Multiply the number you got from step 1 times the answer to the previous
math problem times some random number and whatever is in the 1's column is
you answer"

This ends up being a chain of math problems all pointing back to the previous
one. That's where we came up with the term block chain!

Okay, now how to we create Boopcoins in the first place?

How about we "pay" people for guessing the answers to the math problems.
Every time a miner mines a block we'll let them add one more transaction to
the block. So the miner, let's call him Moe, might add the transaction. "Moe
mined one boopcoin" Maybe that's transaction #99. He can now spend that
boopcoin like any other coin by signing the message "Moe pays Alice one
boopcoin he got from transaction #99".

We have currency!

The last step is making the math problem really really hard to solve. Currently it takes around 29,097,677,000,000,000,000 guesses before you mine a bitcoin. There are machines now which can do lots of math problems at once but that still takes lots of energy. At which point you're basically converting the energy you spend running those machines into Bitcoin.

Here's how things actually work in the Bitcoin protocol if you're interested:

Combining a block into one number - Bitcoin uses [merkle
trees](https://bitcoin.org/en/glossary/merkle-tree) to reduce a block of
transactions into one number.


The hard math problem - Bitcoin uses double
[SHA-256](https://en.wikipedia.org/wiki/SHA-2) as the hard math problem.
SHA256 outputs a number and if that number is below a certain
[difficulty](https://en.bitcoin.it/wiki/Difficulty) it's accepted as the
"right" answer.

Transaction ids - Transaction ids are just the SHA256 of the transaction
