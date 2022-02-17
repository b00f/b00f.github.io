---
layout: post
title: "About security of Proof of Stake"
date: 2022-02-17 00:00:00 +0000
categories: blockchain
---

## About Nothing at Stack

I am not sure where the term "Nothing at stake" is minted, but I can guess it came from the Bitcoin community.
In Proof of Work blockchains, fork is inevitable, but only for a short period of time. Finally the longer chain will win.
A longer chain is a chain that has more work and therefore competing with it is hard. It's very organic and simple, isn't it?
With this mindset of race between forks, a [Proof of Stake](https://bitcointalk.org/index.php?topic=27787.0) blockchain is unsecure, because there is no work here, only stake, and a validator loses nothing if they bet on both forks. Looks unsecure, right?

Sometimes you need to implement an idea to understand it well. In Proof of Stake blockchain fork should never happen at the current height, if it happens it is the result of bad implementation. So there is no "Nothing-At-Stake" attack.

Let's dig it more. Here is some assumptions:
1- In each height the proposer is deterministically known
2- More than ⅔ of the stake should be honest and sign the proposed block

To create a fork at the current height, a (bad) proposer has to publish two different proposals to the network and create a fork.
Due to the second assumption, 2/3 of validators are honest and loyal to the network and they don’t sign two proposals at the same time. Therefore only one proposal has the chance to be signed and accepted by others, the second proposal will be rejected. So in proof of stake blockchain the finality is an immediate action by the network. Beautiful, isn’t it?

The second assumption for Proof of stake blockchain is vital, otherwise the consensus mechanism will be suffered.

### About Slashing

When you don’t understand the question, you will give the wrong answer to it. Slashing mechanism is a wrong answer to an invalid question and just brings complexity to the codebase. In which fork are you going to slash the stacks? And which fork are you going to choose as a main fork?

## About Long Range Attack

If nothing-at-stake is not important, Long Range attack is very [serious attack](https://bitcointalk.org/index.php?topic=1382241.0) for Proof of Stake blockchains. Imagine in a real proof of Stake blockchain with random validators you can obtain (or even buy) the private keys from the early validators. If you have enough keys, you can reorganize a new fork from the early blocks and attack the blockchain. Consider that there is no work here, and just stake. Therefore reorganizing the blockchain is easy.

Unfortunately there is no way to immune a proof of stake blockchain against Long Range attack. The only way to mitigate this attack is to somehow bind the transaction into the main chain or fork. In this case you can put something from the main chain (like previous block hash) into the transaction. This mechanism is pretty like stamping a transaction and helping to secure the blockchain against long range attacks. Stamped transactions are valid only on the main chain and therefore you can’t use them in any other long fork.
