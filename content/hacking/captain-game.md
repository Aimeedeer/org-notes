+++
title = "Captain Game"
author = ["Aimee Z"]
description = "A captain game smart contract"
date = 2020-12-27
tags = ["rust", "smartcontract"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2002
  identifier = "captain-game"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [My hacklog](#my-hacklog)
    - [2021-01-01](#2021-01-01)
    - [2020-12-31](#2020-12-31)
    - [2020-12-29](#2020-12-29)
    - [2020-12-27](#2020-12-27)

</div>
<!--endtoc-->

TODO:

-   [ ] Game contract 0.05


## My hacklog {#my-hacklog}


### 2021-01-01 {#2021-01-01}

In ink, `clone` can't be derived for nested `HashMap` type.
We use `BTreeMap` instead.

```rust
use ink_storage::collections::HashMap;

pub struct Game {
    game_accounts: HashMap<AccountId, GameAccount>,
}

pub struct GameAccount {
    level: u32,
    level_programs: BTreeMap<u32, AccountId>,
}
```


### 2020-12-31 {#2020-12-31}

We can't use `String` directly.

```shell
   Compiling game v0.1.0 (/private/var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T/cargo-contract_GdJJAl)
error[E0412]: cannot find type `String` in this scope
  --> /Users/aimeez/github/contract-game/src/game/lib.rs:49:51
   |
49 |     pub fn create_game_account(&mut self, role_name: String) -> Result<(), Error> {
   |                                                      ^^^^^^ not found in this scope

error: aborting due to previous error
```

We need `alloc` here.

```rust
extern crate alloc;

#[ink::contract]
mod game {
    use ink_storage::collections::HashMap;
    use alloc::{string::String, format};
    ...
}
```


### 2020-12-29 {#2020-12-29}

Added `HashMap`, and it looks complicated.

```rust
use ink_storage::collections::HashMap;

#[ink(storage)]
pub struct Game {
    game_accounts: HashMap<AccountId, GameAccount>,
}

#[derive(Debug, scale::Encode, scale::Decode, ink_storage_derive::PackedLayout, ink_storage_derive::SpreadLayout)]
#[cfg_attr(feature = "std", derive(scale_info::TypeInfo))]
pub struct GameAccount {
    game_account_id: [u32; 8],
    level: u32,
}
```


### 2020-12-27 {#2020-12-27}

Use `ink!` to create a contract template in our project (currently private).

```shell
$ cargo contract check --manifest-path src/game/Cargo.toml
```