---
layout: post
title:  "Byzantine Agreement Proof of Stake"
date:   2019-02-02 00:00:00 +0800
categories: blockchain
---

*In this article, I try to discuss about the practical issues of Proof of Stake consensus model. I assume that you know the concepts of consensus engine in the blockchain and difference between the Proof of Work and Proof of Stake consensus models. In case you want to know more about blockchain fundamental parts read here: [Understanding Blockchain Fundamentals](https://medium.com/loom-network/understanding-blockchain-fundamentals-part-1-byzantine-fault-tolerance-245f46fe8419)*

## A paradox

Proof of Stake is awesome but it is not practical. Unlike Proof of Work, which is based on competition between miners, Proof of Stake is based on cooperation between validators. Validators work together to validate a block, like a team. A good player will be incentivized and a bad actor will be punished and fired from the team. However, the question is how big will this team or validator set be?

For a team, size matters. You can’t simply ask anyone to join your team! Therefore, in PoS (Proof of Stake) we will face this issue during implementing how big a validator set can be? If your validator set gets big, the voting time will be loner and also the set will be more vulnerable in network partition attack. For example, some calculation in [Tendermint](https://tendermint.com/) shows that with more than 100 validators, it becomes less efficient.

The block size can also be a big issue when the number of validators increase. Validators in PoS put their signature in the end of each block to announce their vote to other. As long as more than 2/3 of validators sign a block, the block will be committed into the blockchain. If each signature has 64 bytes length, for 100 validator, the size of a block will be more than 10KB, for 1000 validator it will be more than 100KB. This is a huge overhead in designing a PoS blockchain. However you can use BLS signature aggregation to aggregate all the signatures into a single one during commit time, but still you need to pass whole signatures before committing the block among the validators.

Here is the paradox: a blockchain will be more secure and decentralized if more validators participated on it, but all implementations of PoS has limitation for the size of the validator set. You need more validators but you can’t have it.

### Delegated Proof of Stake (dPoS)
![Delegated Proof of Stake](/public/images/delegated_proof_of_stake.jpg)

In a nutshell, in Delegated Proof of Stake (dPoS), people vote for a group of ‘delegates’ and these delegates validate transactions and produce blocks.

But how can voters trust a delegate? If a delegate is acting badly, what will happen? You can stake your token in favor of good actors. But you still need a minimum trust on this platform. We know in the blockchain, the main consent is not trusting anyone. This is another paradox in dPoS.

In the EOS blockchain, there is [constitution](https://github.com/EOSIO/eos/blob/5068823fbc8a8f7d29733309c0496438c339f7dc/constitution.md) for the people participating in the blockchain. This constitution is full of “shall” and “shall not”. In the long run, this blockchain will be ruled by a powerful organization and won’t be decentralized anymore.

### Byzantine Agreement Proof of Stake (baPoS)
![Byzantine Agreement Proof of Stake](/public/images/byzantine_agreement_proof_of_stake.jpg)

The solution we are offering in [GALLACTIC Blockchain](https://github.com/gallactic/gallactic/) is by creating a **dynamic set** of validators. Validators can be changed randomly. Anyone can easily become a validator by staking some tokens. In each round, every validator starts running a Verifiable Random Function (VRF) in order to self-choose themselves. The VRF is absolutely random and the result can be verified photographically. Based on their stake and their chance, a validator can be on the set for the next run. Once a validator enters the set, the oldest validator in the set exits. So we can always guarantee that validators are in the set for a certain amount of time. In the set, validators have equal power and it helps to make a fully democratic set for the blockchain.

For more information, check out [here](https://www.slideshare.net/MostafaSedaghatjoo/byzantine-agreement-proof-of-stake).
