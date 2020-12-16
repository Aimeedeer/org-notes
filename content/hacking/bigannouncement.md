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

- [TBA on Ethereum](#tba-on-ethereum)
- [TBA's smart contract on Substrate](#tba-s-smart-contract-on-substrate)
    - [Source code (WIP)](#source-code--wip)
    - [Hacklog](#hacklog)

</div>
<!--endtoc-->


## TBA on Ethereum {#tba-on-ethereum}

-   Source code: [bigannouncement.eth](https://github.com/Aimeedeer/bigannouncement)
    -   [Solidity contract](https://github.com/Aimeedeer/bigannouncement/blob/master/contracts/BigAnnouncement.sol)
-   [Hacklog](https://github.com/Aimeedeer/bigannouncement/blob/master/doc/hacklog.md)


## TBA's smart contract on Substrate {#tba-s-smart-contract-on-substrate}


### Source code (WIP) {#source-code--wip}

[tba-substrate](https://github.com/Aimeedeer/tba-substrate)


### Hacklog {#hacklog}


#### Contract naming {#contract-naming}

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


#### Failed of using `ink_prelude` {#failed-of-using-ink-prelude}

I use `cargo add ink_prelude`, and it shows in the `Cargo.toml` as

```rust
ink_prelude = "0.0.0"
```

But the [newest released version](https://github.com/paritytech/ink/commit/75d3b99c3b86398acaef74b84e441da79a88c53f) is different, and I change it to

```rust
ink_prelude = "3.0.0-rc2"
```

Then build it, and there is a new error:

```shell
$ cargo +nightly contract build
 [1/3] Building cargo project
   Compiling ink_prelude v3.0.0-rc2
   Compiling ink_primitives v3.0.0-rc2
error[E0463]: can't find crate for `std`
  |
  = note: the `wasm32-unknown-unknown` target may not be installed

error: aborting due to previous error

For more information about this error, try `rustc --explain E0463`.
error: could not compile `ink_prelude`
```

Keep trying:

```shell
$ rustup install nightly-2020-10-06
info: syncing channel updates for 'nightly-2020-10-06-x86_64-apple-darwin'
info: latest update on 2020-10-06, rust version 1.49.0-nightly (a1dfd2490 2020-10-05)
...

$ rustup target add wasm32-unknown-unknown --toolchain nightly-2020-10-06
info: downloading component 'rust-std' for 'wasm32-unknown-unknown'
info: installing component 'rust-std' for 'wasm32-unknown-unknown'
info: using up to 500.0 MiB of RAM to unpack components
Aimees-MacBook-Pro:tbaSubstrate aimeez$ cargo +nightly-2020-10-06 build --release

...

warning: unused imports: `format`, `string`
 --> lib.rs:4:19
  |
4 | use ink_prelude::{format, string};
  |                   ^^^^^^  ^^^^^^
  |
  = note: `#[warn(unused_imports)]` on by default

warning: 1 warning emitted

    Finished release [optimized] target(s) in 1m 24s

$ cargo contract build && cargo contract generate-metadata
 [1/3] Building cargo project
   Compiling ink_prelude v3.0.0-rc2
error[E0463]: can't find crate for `std`
  |
  = note: the `wasm32-unknown-unknown` target may not be installed

error: aborting due to previous error

For more information about this error, try `rustc --explain E0463`.
error: could not compile `ink_prelude`
```

At last, I use `alloc` instead of `ink_prelude`:

```rust
use alloc::{string::String, format};
```

It built.


#### Deploy failed: {#deploy-failed}

```shell
system.ExtrinsicFailed
```


#### Run Rust test command {#run-rust-test-command}

```shell
$ cargo +nightly test -- new_works # test new an instance
```

```shell
$ RUST_BACKTRACE=1 cargo +nightly test -- new_works
```


#### Debug\_println {#debug-println}

```rust
let message = String::from("Initialize the Big Announcement contract");
ink_env::debug_println(&format!("thanks for instantiation {:?}, and price {}", &message, price));
```

```shell
$ cargo +nightly test -- --nocapture

    Finished test [unoptimized + debuginfo] target(s) in 0.08s
     Running target/debug/deps/tba_substrate-6d0825befe66c5c8

running 2 tests
thanks for instantiation "Initialize the Big Announcement contract", and price 340282366920938463463374607431768211455
thanks for instantiation "Initialize the Big Announcement contract", and price 1
Thanks for posting the message "new message"
test tba_substrate::tests::new_works ... ok
test tba_substrate::tests::set_works ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

```shell
$ cargo +nightly test -- --nocapture set_works
```


#### Test ink's example: contract-transfer {#test-ink-s-example-contract-transfer}

```rust
#[ink::test]
fn transfer_works() {
    // given
    let contract_balance = 100;
    let accounts = default_accounts();
    let mut give_me = create_contract(contract_balance);

    println!("alice: {}", get_balance(accounts.alice));
    // when
    set_sender(accounts.eve);
    set_balance(accounts.eve, 0);
    println!("eve: {}", get_balance(accounts.eve));
    assert_eq!(give_me.give_me(80), Ok(()));

    println!("eve after: {}", get_balance(accounts.eve));
    println!("alice after: {}", get_balance(accounts.alice));

    // then
    assert_eq!(get_balance(accounts.eve), 80);
}
```

The printing shows that alice's balance didn't change
while eve's balance changed.

```shell
$ cargo +nightly test -- --nocapture transfer_works
running 1 test
alice after: 170141183460469231731687303715884105727
eve: 0
eve after: 80
alice after: 170141183460469231731687303715884105727
test give_me::tests::transfer_works ... ok
```