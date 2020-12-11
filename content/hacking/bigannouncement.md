+++
title = "Project: The Big Announcement"
author = ["Aimee Z"]
description = "A tiny smart contract."
date = 2020-12-07
tags = ["substrate", "ethereum", "smartcontract"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2002
  identifier = "project-the-big-announcement"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [TBA's smart contract on Substrate](#tba-s-smart-contract-on-substrate)
    - [TODO](#todo)
- [TBA on Ethereum](#tba-on-ethereum)

</div>
<!--endtoc-->


## TBA's smart contract on Substrate {#tba-s-smart-contract-on-substrate}

Ah oh...

```shell
$ cargo contract new bigannouncement-substrate
ERROR: Contract names cannot contain hyphens
```

New name: tbaSubstrate ;)

```shell
$ cargo contract new tbaSubstrate
      Created contract tbaSubstrate
```

But Rust seems doesn't like it:

```shell
warning: crate `tbaSubstrate` should have a snake case name
  |
  = note: `#[warn(non_snake_case)]` on by default
  = help: convert the identifier to snake case: `tba_substrate`
```

My mood is just like this post: [Frustrated? It's not you, it's Rust](https://fasterthanli.me/articles/frustrated-its-not-you-its-rust)


### TODO {#todo}

-   [ ] Compare the caller's price and set/ornot the new message(hash)
-   [ ] Save or read from some ink API for the last message(hash string) and paid money (bignumber type?)
-   [ ] Withdraw() function


## TBA on Ethereum {#tba-on-ethereum}

-   Source code: [bigannouncement.eth](https://github.com/Aimeedeer/bigannouncement)
    -   [Solidity contract](https://github.com/Aimeedeer/bigannouncement/blob/master/contracts/BigAnnouncement.sol)
-   [Hacklog](https://github.com/Aimeedeer/bigannouncement/blob/master/doc/hacklog.md)