---
layout: post
title: "BLS Threshold Signature using Shamir's Secret Sharing"
date: 2025-07-11 00:00:00 +0000
categories: cryptography
---

In this post, I’m going to explain how we can use **Shamir's Secret Sharing** for a **Threshold Signature Scheme** using the **BLS Signature** scheme.

## Threshold Signature

A [Threshold Signature](https://medium.com/keepnetwork/threshold-signatures-ff2c2b98d9c7)
allows a group of participants to collectively sign a message such that
any subset of at least $k$ out of $n$ participants can generate a valid signature, while fewer than $k$ cannot.
This is extremely useful in distributed systems, where we want fault tolerance and decentralization in
signing operations without having to trust a single key holder.

## Shamir's Secret Sharing

Adi Shamir, in his seminal 1979 paper *"How to Share a Secret"*, introduced a method to split a secret into parts using **Lagrange interpolation** over a finite field.
This technique is known as **Shamir's Secret Sharing (SSS)** and
remains one of the most widely used methods for secure secret splitting.

In a nutshell, a secret can be embedded in a random polynomial:

$$
f(x) = a_0 + a_1x + a_2x^2 + \cdots + a_{k-1}x^{k-1}
$$

Here, $ a_0 $ is the secret, and the rest of the coefficients $a_1, a_2, \dots, a_{k-1}$ are chosen randomly.

We then evaluate this polynomial at $n$ different non-zero values $ x_1, x_2, \dots, x_n $ to generate $n$ shares: $ (x_i, f(x_i)) $.
These shares are distributed to participants.

To recover the secret $ a_0 = f(0) $, we only need $k$ shares. Using **Lagrange interpolation**, we can reconstruct the polynomial and retrieve the secret:

$$
f(0) = \sum_{j=0}^{k-1} y_j \prod_{\begin{smallmatrix} m=0 \\ m \ne j \end{smallmatrix}}^{k-1} \frac{x_m}{x_m - x_j}
$$


