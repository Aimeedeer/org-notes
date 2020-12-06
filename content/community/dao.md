+++
title = "DAO"
author = ["Aimee Z"]
description = "Reading notes about the community DAO."
date = 2020-11-16
tags = ["blockchain", "dao"]
categories = ["community"]
draft = false
[menu.main]
  weight = 2016
  identifier = "dao"
+++

[Secret Sharing DAOs: The Other Crypto 2.0](https://blog.ethereum.org/2014/12/26/secret-sharing-daos-crypto-2-0/), 2014

[The General Theory of Decentralized Applications, Dapps](https://github.com/DavidJohnstonCEO/DecentralizedApplications)
> The concept of a Dapp is so powerful and elegant, because it does not
include these traditional corporate techniques. The ownership of
the Dapp’s tokens is all that is required for the holder to use the system.
It’s that simple. The value of the tokens is determined by
how much people value the application. All the incentives,
all the monetization, all the ways to raise support are
built into this beautifully simple structure.
Dapps are not required to recreate the functions that used to be necessary
in centralized corporations in order to balance the power of shareholders
and offer returns for investors and employees.

> Initial tokens are distributed
>
> If the Dapp is using the fund-raising mechanism, a wallet software becomes
available to the stakeholders of the Dapp, so that they can exchange
the tokens of the DA. In the case of Mastercoin, an Exodus fund-raising address
and a wallet script were publicly released.
>
> If the Dapp is using the development mechanism, a bounty system is
put in place that allows the suggestion of tasks to be performed,
the tracking of the people who are working on those tasks and
the criteria by which bounties can be awarded.

[Bootstrapping A Decentralized Autonomous Corporation: Part I](https://bitcoinmagazine.com/articles/bootstrapping-a-decentralized-autonomous-corporation-part-i-1379644274) | 2013
> As Let’s Talk Bitcoin’s Daniel Larmier pointed out in his own exploration on this concept,
in a sense Bitcoin itself can be thought of as a very early prototype of exactly such a thing.
Bitcoin has 21 million shares, and these shares are owned by what can be considered
Bitcoin’s shareholders. It has employees, and it has a protocol for paying them: 25 BTC
to one random member of the workforce roughly every ten minutes. It even has
its own marketing department, to a large extent made up of the shareholders themselves.
However, it is also very limited. It knows almost nothing about the world except for
the current time, it has no way of changing any aspect of its function aside from
the difficulty, and it does not actually do anything per se; it simply exists,
and leaves it up to the world to recognize it. The question is: can we do better?

> The first challenge is obvious: how would such a corporation actually make
any decisions? It’s easy to write code that, at least given predictable environments,
takes a given input and calculates a desired action to take. But who is going to
run the code? If the code simply exists as a computer program on some particular machine,
what is stopping the owner of that machine from shutting the whole thing down,
or even modifying its code to make it send all of its money to himself? To this problem,
there is only one effective answer: distributed computing.

> Here, rather, we need the kind of distributed computing that we see in Bitcoin:
a set of rules that decentrally self-validates its own computation.
In Bitcoin, this is accomplished by a simple majority vote: if you are not helping to
compute the blockchain with the majority network power, your blocks will get discarded
and you will get no block reward. The theory is that no single attacker will
have enough computer power to subvert this mechanism, so the only viable strategy
is essentially to “go with the flow” and act honestly to help support the network
and receive one’s block reward. So can we simply apply this mechanism to
decentralized computation? That is, can we simply ask every computer in the network to
evaluate a program, and then reward only those whose answer matches the majority vote?

> Bitcoin is a special case because Bitcoin is simple: it is just a currency,
carrying no property or private data of its own. A virtual corporation,
on the other hand, would likely need to store the private key to its Bitcoin wallet –
a piece of data which should be available in its entirety to no one, not to everyone
in the way that Bitcoin transactions are. But, of course, the private key must
still be usable. Thus, what we need is some system of signing transactions,
and even generating Bitcoin addresses, that can be computed in a decentralized way.
Fortunately, Bitcoin allows us to do exactly that.

[
DAOs, DACs, DAs and More: An Incomplete Terminology Guide](https://blog.ethereum.org/2014/05/06/daos-dacs-das-and-more-an-incomplete-terminology-guide/) | 2014
> as Bitshares’ Daniel Larimer points out, “everyone thinks a DAC is just a way of
IPOing your centralized company.” The intent of this article will be to delve into
some of these concepts, and see if we can come up with at least the beginnings of
a coherent understanding of what all of these things actually are.

> Note that there is one gray area here: contracts which are finite on one side,
but infinite on the other side. For example, if I want to hedge the value of
my digital assets, I might want to create a contract where anyone can
freely enter and leave. Hence, the other side of the contract, the parties
that are speculating on the asset at 2x leverage, has an unbounded number of parties,
but my side of the contract does not. Here, I propose the following divide:
if the side with a bounded number of parties is the side that intends to receive
a specific service (ie. is a consumer), then it is a smart contract; however,
if the side with a bounded number of parties is just in it for profit
(ie. is a producer), then it is not.

> That is, there is a concept of shares in a DAC which are purchaseable
and tradeable in some fashion, and those shares potentially entitle
their holders to continual receipts based on the DAC’s success.
A DAO is non-profit; though you can make money in a DAO, the way to do
that is by participating in its ecosystem and not by providing investment
into the DAO itself. Obviously, this distinction is a murky one;
all DAOs contain internal capital that can be owned, and the value of
that internal capital can easily go up as the DAO becomes more powerful/popular,
so a large portion of DAOs are inevitably going to be DAC-like to some extent.
>
> Thus, the distinction is more of a fluid one and hinges on emphasis:
to what extent are dividends the main point, and to what extent is it about
earning tokens by participation? Also, to what extent does the concept of
a “share” exist as opposed to simple virtual property? For example,
a membership on a nonprofit board is not really a share, because membership
frequently gets granted and confiscated at will, something which would be
unacceptable for something classified as investable property, and a bitcoin is not
a share because a bitcoin does not entitle you to any claim on profits or
decision-making ability inside the system, whereas a share in a corporation
definitely is a share. In the end, perhaps the distinction might ultimately be
the surprisingly obscure point of whether or not the profit mechanism
and the consensus mechanism are the same thing.

> Additionally, there is also the question of how all of these things should be built.
An AI, for example, should likely exist as a network of private servers,
each one running often proprietary local code, whereas a DO should be fully
open source and blockchain-based. Between those two extremes, there is
a large number of different paradigms to pursue. How much of the intelligence
should be in the core code? Should genetic algorithms be used for updating code,
or should it be futarchy or some voting or vetting mechanism based on individuals?
Should membership be corporate-style, with sellable and transferable shares,
or nonprofit-style, where members can vote other members in and out?
Should blockchains be proof of work, proof of stake, or reputation-based?
Should DAOs try to maintain balances in other currencies,
or should they only reward behavior by issuing their own internal token?
These are all hard problems and we have only just begun scratching the surface of them.

[Turn an Internet Community into a DAO in 3 Steps](https://hackernoon.com/turn-an-internet-community-into-a-dao-in-3-steps-8k1b3w5y)
> Simply put, DAO is a perfect structure to organize collective activities in the community,
especially when most collaborations in the community are conducted distributedly,
multi-disciplinarily, in random occurrence, and without a trust basis.
DAO leverages Smart Contract on the blockchain to automatically implement contract terms
to solve the trust issues. Also, contribution-based incentives can be allocated to activate the community.
>
> However, DAO's structure is not perfect and has some limitations for large scale applications.
>
> First, DAO relies too much on the smart contract, while the machine language is not suitable
for conveying complex business logic. It's almost impossible for a general internet community
to become an expert to compile all its collaboration needs into the contract.
The stake is too high! It is also costly and low in efficiency if every operation step
needs to run smart contracts on the blockchain to reach the global consensus.
>
> Second, most of DAO's governance protocols mainly focus on the voting algorithm,
while for the internet communities, voting would only be a small part of
all the collective actions. Actually, the community is more caring about how to
make the collaborations happen among all the community members.
>
> Third, as there is a lack of an effective management system to regulate
the collaborations, the DAO has become only a shell structure without any real value
creation activities in it. So, it's not hard to understand that the DAO's
governance token holders have low intent to vote as they do not care about
what is going on in the community, and they are only pursuing arbitrage opportunities.

> The Wiki-based ComCo Management Framework contains one's collaboration history,
which can be traced back as the credential check.

\*\*