---
layout: post
title: "Security of Proof of Stake"
date: 2022-02-17 00:00:00 +0000
categories: blockchain
---

## About Nothing at Stake

I am not sure where the term “Nothing at Stake” was minted, but I can only guess that it came from the [Bitcoin community](https://bitcointalk.org/index.php?topic=897488.0). In Proof of Work blockchains, a fork is inevitable, but only for a short period of time. Alternatively, a longer chain is a chain that requires more work and therefore competing with it is hard, in the end, the longer chain will always win. It’s very organic and simple, wouldn’t you agree?

With the mindset of racing between forks, a [Proof of Stake](https://bitcointalk.org/index.php?topic=27787.0) blockchain looks insecure since there is no work involved, only stake, therefore validators lose nothing if they bet on both forks. This is the so-called “Nothing at Stake” attack.

This definition is based a wrong assumption. In Proof of Stake blockchains, a fork should never happen at the current height, so there shouldn’t be a “Nothing at Stake” attack.

Let’s dig into it a little deeper. Here are some assumptions:
1. In each height the proposer is deterministically known
2. More than ⅔ of the stake should be honest and sign the proposed block

To create a fork at the current height, a (bad) proposer has to publish two different proposals to the network. The second assumption states that ⅔ of the validators are honest and loyal to the network and that two proposals aren’t signed at once. As a result, only one proposal has the chance to be signed and accepted by others, while the second proposal will be rejected. Therefore, in Proof of Stake blockchains, the finality of it is an immediate action by the network. Beautiful, isn’t it?

In a nutshell, in Proof of Stake, if a fork happens at the current height, it is the result of a bad implementation or else the second assumption is not fulfilled. The second assumption for Proof of Stake blockchains is vital, if it is not fulfilled, the consensus mechanism will suffer.

### Slashing is wrong answer

When you don’t understand the question, you will give the wrong answer to it. The slashing mechanism is a wrong answer to a wrong question and brings much added complexity to the codebase. Slashing mechanisms also raise many questions, such as: “In which fork are you going to slash the stakes?” or, “Which fork are you going to choose as a main fork?”

## About Long Range Attack

If “Nothing at Stake” is unimportant, then Long Range Attacks are a very [serious attack](https://bitcointalk.org/index.php?topic=1382241.0) for Proof of Stake blockchains. Imagine that in a real Proof of Stake blockchain, with random validators, you can obtain (or even buy) the private keys from the early validators. If you have enough keys, you can reorganize a new fork from the early blocks and attack the blockchain. If you consider that there is no work here, just stake, then reorganizing the blockchain is easy.

### Algorand solution

In the [Algorand](https://www.algorand.com/) protocol users change their ephemeral participation keys used for every round. That is, after a user signs her message for round `r`, she deletes the ephemeral key used for signing, and fresh ephemeral keys will be used in future rounds. So even if in the future an adversary corrupts all committee members for a round `r`, as the users holding the supermajority of stakes were honest in round `r` and erased their ephemeral
keys, no two distinct valid blocks can be produced for the same round. [^1]

Algorand's solution for preventing Long Range attack has some side effects: It added complexity to the codebase and since all participant should provide cryptographic proof for ephemeral keys per round, it increased the network bandwidth.


### Zarb solution

One way to mitigate these attacks is to somehow bind the transaction into the main chain or fork. In this way, you could put something from the main chain, such as the previous block hash, into the transaction. This mechanism would be like stamping a transaction and would help to secure the blockchain against Long Range Attacks. Stamped transactions would be valid only on the main chain and therefore you wouldn’t be able to use them in any other long fork.

### Checkpoint solution

Some blockchains suggest to create checkpoints to prevent Long Range attack. This is a naive solution and doesn't solve the problem. First adversary can create the checkpoints also, and more importantly, how often a checkpoint should be created? In the other word, how long can be a Long Range attack!?

---

[^1]: [Algorand Blockchain Features Specification, Version 1.0](https://github.com/algorandfoundation/specs/blob/master/overview/Algorand_v1_spec-2.pdf)
