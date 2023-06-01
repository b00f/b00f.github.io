---
layout: post
title: "What makes a blockchain secure?"
date: 2022-03-17 00:00:00 +0000
categories: blockchain
---

Consider a blockchain project with thousands of lines of code as a black box.
There may be critical bugs or issues hidden within it.
Each of them can act as a potential bomb, and if someone finds and triggers them, they can cause serious damage.
But how does a blockchain secure itself against these potential bombs?

The answer is "Decentralization".
Decentralization makes a blockchain secure, even if it has potential bugs or issues.

If a blockchain is not decentralized, it doesn’t have value, and therefore, no one will care about it.
Even if a hacker bothers to find the bomb, it doesn’t matter.
No one gets hurt by exploding a bomb in the desert.

But what about a decentralized blockchain? What happens if someone finds one of the bombs within the blockchain? There are two real-world scenarios that can happen here:

1. A [white hat](<https://en.wikipedia.org/wiki/White_hat_(computer_security)>) hacker finds the issue first, reports it to the community, and tries to fix it.
Like disposing the bomb before exploding.
The [Bitcoin Transaction Malleability](https://www.coindesk.com/markets/2014/02/12/what-the-bitcoin-bug-means-a-guide-to-transaction-malleability/) is a good example of how the community can find, report, and fix a potential issue before it causes any serious damage.

2. A [black hat](<https://en.wikipedia.org/wiki/Black_hat_(computer_security)>)  hacker finds the issue first and exploits it for personal gain.
This damage can be destructive.
But even in this case, if a blockchain is truly decentralized, the community can recover the blockchain from the damage.
I can't imagine a scenario worse than the [DAO attack](https://www.coindesk.com/markets/2020/09/20/did-ethereum-learn-anything-from-the-55m-dao-attack/) in Ethereum.
In the DAO attack, more than 3.64 million or about 5% of the total supply was hacked.
But the community decided to undo this attack by forking the blockchain.
It was controversial and [funny](https://www.youtube.com/watch?v=_O5fdMFKEC0) but it worked!

In conclusion, **decentralization makes a blockchain secure, not the algorithm**, but the algorithm is important to make a blockchain decentralized.
A poorly implemented or overly complex blockchain can't be decentralized.
It is a chicken and egg story.
