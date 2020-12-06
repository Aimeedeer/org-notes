+++
title = "Blockchain Voting"
author = ["Aimee Z"]
description = "Reading notes about blockchain voting."
date = 2020-11-16
tags = ["blockchain", "voting"]
categories = ["community"]
draft = false
[menu.main]
  weight = 2015
  identifier = "blockchain-voting"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Posts](#posts)
- [Papers](#papers)

</div>
<!--endtoc-->


## Posts {#posts}

[Blockchain and Trust](https://www.schneier.com/blog/archives/2019/02/blockchain%5Fand%5F.html), 2019
> When you analyze both blockchain and trust, you quickly realize that
there is much more hype than value. Blockchain solutions are
often much worse than what they replace.
>
> First, a caveat. By blockchain, I mean something very specific:
the data structures and protocols that make up a public blockchain.
These have three essential elements. The first is a distributed
(as in multiple copies) but centralized (as in there’s only one) ledger,
which is a way of recording what happened and in what order.
This ledger is public, meaning that anyone can read it, and immutable,
meaning that no one can change what happened in the past.
>
> The second element is the consensus algorithm, which is a way
to ensure all the copies of the ledger are the same. This is
generally called mining; a critical part of the system is that
anyone can participate. It is also distributed, meaning that
you don’t have to trust any particular node in the consensus network.
It can also be extremely expensive, both in data storage and
in the energy required to maintain it. Bitcoin has the most expensive
consensus algorithm the world has ever seen, by far.
>
> Finally, the third element is the currency. This is some sort of
digital token that has value and is publicly traded. Currency is
a necessary element of a blockchain to align the incentives of
everyone involved. Transactions involving these tokens are stored on the ledger.
>
> Most blockchain enthusiasts have a unnaturally narrow definition of trust.
They’re fond of catchphrases like “[in code we trust](https://www.nytimes.com/2017/12/18/opinion/bitcoin-boom-technology-trust.html),” “[in math we trust](https://www.amazon.com/Math-We-Trust-Bitcoin-Cryptocurrency-ebook/dp/B07C7TPXMD?tag=w050b-20),”
and “[in crypto we trust](https://cryptoclothing.org/product/crypto-shirt/).” This is trust as verification.
But verification isn’t the same as trust.

> Morals and reputation scale only to a certain population size.
Primitive systems were good enough for small communities,
but larger communities required delegation, and more formalism.
>
> The third is institutions. Institutions have rules and laws that
induce people to behave according to the group norm,
imposing sanctions on those who do not. In a sense,
laws formalize reputation. Finally, the fourth is security systems.
These are the wide varieties of security technologies we employ:
door locks and tall fences, alarm systems and guards,
forensics and audit systems, and so on.
>
> These four elements work together to enable trust.

> In many ways, trusting technology is harder than trusting people.
Would you rather trust a human legal system or the details of
some computer code you don’t have the expertise to audit?


## Papers {#papers}

[Going from Bad to Worse: From Internet Voting to Blockchain Voting](https://people.csail.mit.edu/rivest/pubs/PSNR20.pdf), November 6, 2020 (DRAFT)