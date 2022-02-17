---
layout: post
title: "Security of Proof of Stake"
date: 2022-02-17 00:00:00 +0000
categories: blockchain
---

## About Nothing at stake

I am not sure where the term _"Nothing at Stake"_ is minted, but I can guess it came from the [Bitcoin community](https://bitcointalk.org/index.php?topic=897488.0). In Proof of Work blockchains, fork is inevitable, but only for a short period of time. Finally the longer chain will win.
A longer chain is a chain that has more work and therefore competing with it is hard. It's very organic and simple, isn't it?

With the mindset of race between forks, a [Proof of Stake](https://bitcointalk.org/index.php?topic=27787.0) blockchain looks insecure, because there is no work here, only stake, and validators lose nothing if they bet on both forks. This is the so-called "nothing at stake" attack.

Sometimes you need to implement an idea to understand it well. In Proof of Stake blockchain fork should never happen at the current height. So there is no "Nothing at Stake" attack.

Let's dig it more. Here is some assumptions:
1. In each height the proposer is deterministically known
2. More than ⅔ of the stake should be honest and sign the proposed block

To create a fork at the current height, a (bad) proposer has to publish two different proposals to the network and create a fork.
Due to the second assumption, 2/3 of validators are honest and loyal to the network and they don’t sign two proposals at the same time. Therefore only one proposal has the chance to be signed and accepted by others, the second proposal will be rejected. So in proof of stake blockchain the finality is an immediate action by the network. Beautiful, isn’t it?

In a nutshell, in Proof of Stake if a fork happens at the current height, it is the result of a bad implementation or the second assumption is not fulfilled.
The second assumption for Proof of stake blockchain is vital, otherwise the consensus mechanism will be suffered.

### About slashing

When you don’t understand the question, you will give the wrong answer to it. Slashing mechanism is a wrong answer to a wrong question and just brings complexity to the codebase. Slashing mechanisms raise many questions, like: In which fork are you going to slash the stakes? And which fork are you going to choose as a main fork?

## About long range Attack

If "Nothing at Stake" is not important, "Long Range" attack is very [serious attack](https://bitcointalk.org/index.php?topic=1382241.0) for Proof of Stake blockchains. Imagine in a real proof of Stake blockchain with random validators you can obtain (or even buy) the private keys from the early validators. If you have enough keys, you can reorganize a new fork from the early blocks and attack the blockchain. Consider that there is no work here, and just stake. Therefore reorganizing the blockchain is easy.

Unfortunately there is no way to immune a proof of stake blockchain against Long Range attack. The only way to mitigate this attack is to somehow bind the transaction into the main chain or fork. In this case you can put something from the main chain (like previous block hash) into the transaction. This mechanism is pretty like stamping a transaction and helping to secure the blockchain against Long Range attacks. Stamped transactions are valid only on the main chain and therefore you can’t use them in any other long fork.


