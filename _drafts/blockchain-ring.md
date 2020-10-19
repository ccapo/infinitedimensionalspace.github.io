---
layout: post
title: Blockchain Ring POC
description: "Blockchain, Ring, POC"
modified: 2020-10-19
comments: true
tags: [blockchain, ring]
---

I had an idea while working with blockchain technology for the past five years, and I wondered if it were possible to create a blockchain that formed a closed loop.

The reason I wondered if such a object could be constructed comes down to two points:

1. The tip of the blockchain is the most vulnerable portion, but it were a closed loop then that would be less of an issue
2. A self-contained blockchain could be useful for ensuring the contents could not be modified

While the idea of an blockchain ring intrigued to me, the practicality of creating one as well as the potential use cases were less then forthcoming.

Not only could the problem that a self-contained blockchain ring would solve be handled using exising tools, but the little matter of the cryptographically secure hashing algorithms used by Bitcoin and other cryptocurrencies are designed to thwart pre-image attacks.

Pre-image resistence leaves the blockchain ring idea DOA, but rather then be dissuaded I tried to create a POC using non-cryptographic hashing functions or checksums.

I tested out four checksum methods:

1. Naive Checksum
2. XBee Checksum
3. Fletcher-16 Checksum
4. Fletcher-32 Checksum

I wrote a small program to iterate the above formula for different values of *r* and starting values of *x*, and I was able to reproduce the bifurication diagram that illustrates the emergence of chaotic behaviour as *r* increases.

{% gist ccapo/a3329f30b1cd1c45f9f4a83348d06d76#file-naive8-c %}

{% gist ccapo/a3329f30b1cd1c45f9f4a83348d06d76#file-xbee8-c %}

{% gist ccapo/a3329f30b1cd1c45f9f4a83348d06d76#file-fletcher16-c %}

{% gist ccapo/a3329f30b1cd1c45f9f4a83348d06d76#file-fletcher32-c %}
