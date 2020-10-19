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

```
$ ./naive8
{"id":1,"data":"Lorem ipsum dolor sit amet, consectetur adipiscing elit.","prevHash":"u","hash":"�","nonce":"!"}
{"id":2,"data":"Interdum et malesuada fames ac ante ipsum primis in faucibus.","prevHash":"�","hash":"�","nonce":"!"}
{"id":3,"data":"Nunc pharetra elit magna, eu accumsan lorem convallis et.","prevHash":"�","hash":"�","nonce":"!"}
{"id":4,"data":"Ut eu elit eget orci tincidunt mattis eu id eros.","prevHash":"�","hash":"�","nonce":"!"}
{"id":5,"data":"Proin condimentum neque nec dui tempor vehicula.","prevHash":"�","hash":"u","nonce":"v"}
Hash Collision Found!
```

{% gist ccapo/a3329f30b1cd1c45f9f4a83348d06d76#file-xbee8-c %}

```
$ ./xbee8
{"id":1,"data":"Lorem ipsum dolor sit amet, consectetur adipiscing elit.","prevHash":"�","hash":"�","nonce":"!"}
{"id":2,"data":"Interdum et malesuada fames ac ante ipsum primis in faucibus.","prevHash":"�","hash":"I","nonce":"!"}
{"id":3,"data":"Nunc pharetra elit magna, eu accumsan lorem convallis et.","prevHash":"I","hash":"�","nonce":"!"}
{"id":4,"data":"Ut eu elit eget orci tincidunt mattis eu id eros.","prevHash":"�","hash":"r","nonce":"!"}
{"id":5,"data":"Proin condimentum neque nec dui tempor vehicula.","prevHash":"r","hash":"�","nonce":"s"}
Hash Collision Found!
```

{% gist ccapo/a3329f30b1cd1c45f9f4a83348d06d76#file-fletcher16-c %}

```
$ ./fletcher16
{"id":1,"data":"Lorem ipsum dolor sit amet, consectetur adipiscing elit.","prevHash":"}","hash":"h","nonce":"!!"}
{"id":2,"data":"Interdum et malesuada fames ac ante ipsum primis in faucibus.","prevHash":"h","hash":"#}","nonce":"!!"}
{"id":3,"data":"Nunc pharetra elit magna, eu accumsan lorem convallis et.","prevHash":"#}","hash":"i","nonce":"!!"}
{"id":4,"data":"Ut eu elit eget orci tincidunt mattis eu id eros.","prevHash":"i","hash":"P�","nonce":"!!"}
{"id":5,"data":"Proin condimentum neque nec dui tempor vehicula.","prevHash":"P�","hash":"}","nonce":"p"}
Hash Collision Found!
```

{% gist ccapo/a3329f30b1cd1c45f9f4a83348d06d76#file-fletcher32-c %}

```
$ ./fletcher32
{"id":1,"data":"Lorem ipsum dolor sit amet, consectetur adipiscing elit.","prevHash":"o��P","hash":"","nonce":"!!!!"}
{"id":2,"data":"Interdum et malesuada fames ac ante ipsum primis in faucibus.","prevHash":"","hash":"���","nonce":"!!!!"}
{"id":3,"data":"Nunc pharetra elit magna, eu accumsan lorem convallis et.","prevHash":"���","hash":"��O�","nonce":"!!!!"}
{"id":4,"data":"Ut eu elit eget orci tincidunt mattis eu id eros.","prevHash":"��O�","hash":"ĳ�","nonce":"!!!!"}
{"id":5,"data":"Proin condimentum neque nec dui tempor vehicula.","prevHash":"ĳ�","hash":"o��P","nonce":"���{"}
Hash Collision Found!
```
