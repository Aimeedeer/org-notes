+++
title = "Papers"
author = ["Aimee Z"]
description = "Papers I've read."
date = 2020-12-05
tags = ["paper"]
categories = ["reading"]
draft = false
[menu.main]
  weight = 2019
  identifier = "papers"
+++

[Aggregation of Gamma-Signatures and Applications to Bitcoin](https://eprint.iacr.org/2018/414.pdf)

Not a great paper. I don't think it has strong proof of its point,
and I am not sure if it's workable as their reasoning.

> Aggregate signature (AS) allows non-interactively condensing multiple individual signatures into a compact one.
Besides the faster verification, it is useful to reduce storage and bandwidth, and is especially attractive for blockchain and cryptocurrency.

> Currently, due to the 1M-byte limitation of block size, about 7 transactions
are conducted per second in the Bitcoin system. This leads to, in particular,
longer confirmation latency, relatively higher transaction fees, and easier target
of spam attacks.

> Bitcoin uses the EC-DSA signature scheme [37] over
the secp256k1 curve [22]. According to Bitcoin Stack Exchange, in a standard
“pay to public key hash” (P2PKH) transaction or a “pay to script hash” (P2SH)
transaction, the signatures occupy about 40% of transcript size.
In addition, an EC-DSA signature involves non-linear combination of ephemeral secret-key
and static secret-key, which is the source for relative inefficiency and for the
cumbersome in extensions to multi-signatures [8, 47], scriptless scripts [66], etc.
As a consequence, recently there is also renewed interests in deploying Schnorr’s
signature with Bitcoin in the future.

> Practical aggregate signature schemes were proposed [17, 7] in the plain
public-key model. They are derived based on the BLS short signature [18] in
groups with bilinear maps. There have been some discussions on deploying the
pairing-based AS schemes [17, 7] in the Bitcoin system [46], which are briefly
summarized below.
> – System complexity. Deploying pairing-based aggregate signature schemes requires the replacement of not only the EC-DSA algorithm but also the underlying elliptic curve. It makes a deployment in practice (such as Bitcoin)
much more invasive than simply shifting algorithms.
> – Bilinear group vs. general group. Intractability problems in groups with bilinear maps are weaker than the discrete logarithm problem in general EC
groups.
> – Verification speed. As an individual signature scheme, the verification of the
pairing-based BLS signature is significantly slower than that of EC-DSA.
Note that the miners still need to verify the correctness of individual BLS
signatures before aggregating them into a block. Some survey indicates that
on a concrete hardware it can verify 70,000 secp256k1 signatures per second,
while it could only verify about 8,000 BLS signatures per second [46].
>
> It is thus highly desirable to develop aggregate signatures, with the following
features simultaneously:
> – It can be built from general elliptic curves (without bilinear maps), in the
plain public-key model with fully asynchronous communications.
> – The underlying signature scheme has provable security, and moreover, is
more efficient and flexible than EC-DSA.