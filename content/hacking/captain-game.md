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
    - [2021-01-16](#2021-01-16)
    - [2021-01-14](#2021-01-14)
    - [2021-01-07](#2021-01-07)
    - [2021-01-03](#2021-01-03)
    - [2021-01-01](#2021-01-01)
    - [2020-12-31](#2020-12-31)
    - [2020-12-29](#2020-12-29)
    - [2020-12-27](#2020-12-27)

</div>
<!--endtoc-->


## My hacklog {#my-hacklog}


### 2021-01-16 {#2021-01-16}

Create isolated environment for testing.
[Source code](https://github.com/Aimeedeer/game-test).


### 2021-01-14 {#2021-01-14}

Debug `contract trapped`

```shell
2021-01-14 16:34:36.014  DEBUG tokio-runtime-worker runtime:DispatchError
2021-01-14 16:34:36.014  DEBUG tokio-runtime-worker runtime:8
2021-01-14 16:34:36.014  DEBUG tokio-runtime-worker runtime:17
2021-01-14 16:34:36.014  DEBUG tokio-runtime-worker runtime:ContractTrapped
2021-01-14 16:34:36.014  DEBUG tokio-runtime-worker runtime:PostInfo:
2021-01-14 16:34:36.014  DEBUG tokio-runtime-worker runtime:actual_weight=
2021-01-14 16:34:36.014  DEBUG tokio-runtime-worker runtime:7172485790
2021-01-14 16:34:36.014  DEBUG tokio-runtime-worker runtime:pays_fee=
2021-01-14 16:34:36.014  DEBUG tokio-runtime-worker runtime:Yes
```

Found `DispatchError` from Substrate code:

```rust
/// Turn this GasMeter into a DispatchResult that contains the actually used gas.
pub fn into_dispatch_result<R, E>(self, result: Result<R, E>) -> DispatchResultWithPostInfo
where
      E: Into<ExecError>,
{
      let post_info = PostDispatchInfo {
              actual_weight: Some(self.gas_spent()),
              pays_fee: Default::default(),
      };

      result
              .map(|_| post_info)
              .map_err(|e| DispatchErrorWithPostInfo { post_info, error: e.into().error })
}
```


### 2021-01-07 {#2021-01-07}

Test code:

```rust
#[ink(message, payable)]
pub fn run_level_test(&mut self) -> bool {
    let program_id = "4cfac7f74c6233449b5e54ba070231dd94c71b89505482cd910000656258d3ed";

    ink_env::debug_println(&format!("hash {:?}", program_id));

    let program_id = hex::decode(program_id).unwrap();
    ink_env::debug_println(&format!("decode {:?}", program_id));

    let program_id = AccountId::try_from(&program_id[..]).unwrap();
    ink_env::debug_println(&format!("AccountId {:?}", program_id));

    true
}
```

Run Canvas and print `debug` in console:

```shell
$ canvas --dev --tmp -lerror,runtime=debug

2021-01-07 20:47:37.878  DEBUG event.loop0 runtime:hash "4cfac7f74c6233449b5e54ba070231dd94c71b89505482cd910000656258d3ed"
2021-01-07 20:47:37.880  DEBUG event.loop0 runtime:decode [76, 250, 199, 247, 76, 98, 51, 68, 155, 94, 84, 186, 7, 2, 49, 221, 148, 199, 27, 137, 80, 84, 130, 205, 145, 0, 0, 101, 98, 88, 211, 237]
2021-01-07 20:47:37.881  DEBUG event.loop0 runtime:AccountId AccountId([76, 250, 199, 247, 76, 98, 51, 68, 155, 94, 84, 186, 7, 2, 49, 221, 148, 199, 27, 137, 80, 84, 130, 205, 145, 0, 0, 101, 98, 88, 211, 237])
```

Test process:

-   Run Canvas in Firefox
-   Bob uploads Flipper contract (added `selector`)
-   Add deployed Flipper contract address to Game contract test code, and build `cargo contract build`
-   Deploy Game contract with Alice's account
-   Bob calls Alice's Game contract


### 2021-01-03 {#2021-01-03}

Write code for cross-contract call.


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