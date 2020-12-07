+++
title = "Play with Substrate"
author = ["Aimee Z"]
description = "Substrate is interesting and it seems powerful."
date = 2020-11-15
tags = ["rust", "substrate", "blockchain"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2002
  identifier = "play-with-substrate"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [2020-11-27~ Write a simple smart contract](#2020-11-27-write-a-simple-smart-contract)
- [2020-11-20 The example: flipper](#2020-11-20-the-example-flipper)
    - [New tool learned](#new-tool-learned)
- [2020-11-17 ink!](#2020-11-17-ink)
    - [Follow the docs](#follow-the-docs)
    - [Thoughts](#thoughts)
- [2020-11-15 Start](#2020-11-15-start)
    - [Follow the GitHub repo](#follow-the-github-repo)
    - [Install `nightly-2020-10-05` and build again](#install-nightly-2020-10-05-and-build-again)
    - [Set target:](#set-target)
    - [Built failed because my default setting is nightly but not stable.](#built-failed-because-my-default-setting-is-nightly-but-not-stable-dot)
    - [It took 13 minutes to build: my laptop is slow...](#it-took-13-minutes-to-build-my-laptop-is-slow-dot-dot-dot)
    - [Cute run!](#cute-run)
    - [Doc](#doc)

</div>
<!--endtoc-->

References:

-   Doc: [FRAME](https://substrate.dev/docs/en/knowledgebase/runtime/frame)  Framework for Runtime Aggregation of Modularized Entities (FRAME)
-   Doc: [Add a Pallet to Your Runtime](https://substrate.dev/docs/en/tutorials/add-a-pallet/import-a-pallet)
-   Repo: [Substrate Node Template](https://github.com/substrate-developer-hub/substrate-node-template#local-development)
-   Doc: [Attribute Macro ink\_lang::contract](https://paritytech.github.io/ink/ink%5Flang/attr.contract.html)
-   Doc: [Cryptography](https://substrate.dev/docs/en/knowledgebase/advanced/cryptography#public-key-cryptography)
-   Doc: [wasmi](https://paritytech.github.io/wasmi/wasmi/index.html)
-   Doc: [Consensus](https://substrate.dev/docs/en/knowledgebase/advanced/consensus)

> In order to agree on the resulting state after a transition,
all operations within a blockchain's state transition function must be deterministic.

> Substrate provides several block construction
algorithms and also allows you to create your own:
> - Aura (round robin)
> - BABE (slot-based)
> - Proof of Work


## 2020-11-27~ Write a simple smart contract {#2020-11-27-write-a-simple-smart-contract}

Follow [ink](https://github.com/paritytech/ink#play-with-it) GitHub

Creating a default smart contract with `cargo contract new mytest`,
and it generates `lib.rs` and `Cargo.toml` files.

I changed code in `lib.rs`, turn `bool` to `String` type.

```shell
$ cargo contract new mytest

Created contract mytest

$ cd mytest
$ cargo contract build && cargo contract generate-metadata
 [1/3] Building cargo project
ERROR: cargo-contract cannot build using the "stable" channel. Switch to nightly. See https://github.com/paritytech/cargo-contract#build-requires-the-nightly-toolchain
ERROR: cargo-contract cannot build using the "stable" channel. Switch to nightly. See https://github.com/paritytech/cargo-contract#build-requires-the-nightly-toolchain
```

Switch from stable to nightly:

```shell
$ rustup default nightly

info: using existing install for 'nightly-x86_64-apple-darwin'
info: default toolchain set to 'nightly-x86_64-apple-darwin'

  nightly-x86_64-apple-darwin unchanged - rustc 1.50.0-nightly (98d66340d 2020-11-14)

$ cargo contract build && cargo contract generate-metadata
```

The result:

```shell
Your contract is ready. You can find it here:
/<mypath>/mytest/target/mytest.wasm
  Generating metadata
 [1/3] Building cargo project
    Finished release [optimized] target(s) in 0.07s
 [2/3] Post processing wasm file
 [3/3] Optimizing wasm file
  Compiling ... #packages
    Finished release [optimized] target(s) in 1m 20s
     Running `target/release/metadata-gen`
      Your metadata file is ready.
You can find it here:
/<mypath>/mytest/target//metadata.json
```

Change the `bool` type to `String` in the code:

```rust
#[ink(storage)]
pub struct Mytest {
    value: String,
}
```

And run the contract:

```shell
$ cargo contract build && cargo contract generate-metadata
 [1/3] Building cargo project
   Compiling mytest v0.1.0 (/var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T/cargo-contract_p9Kcuf)
error[E0433]: failed to resolve: use of undeclared type `String`
  --> /<mypath>/mytest/lib.rs:28:23
   |
28 |             Self::new(String::from("init!"))
   |                       ^^^^^^ use of undeclared type `String`
```

Now add crate `alloc`:

```rust
extern crate alloc;
use alloc::string::String;
```

and build it:

```shell
$ cargo contract build && cargo contract generate-metadata
 [1/3] Building cargo project
   Compiling mytest v0.1.0 (/var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T/cargo-contract_p7eKiU)
error[E0412]: cannot find type `String` in this scope
  --> /Users/aimeez/github/mytest/lib.rs:11:16
   |
11 |         value: String,
   |                ^^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
7  | use alloc::string::String;
   |
7  | use crate::String;
   |
```

What's wrong?
There is a warning here that says unused import \`alloc::string::String\`.

```shell
$ cargo +nightly test
warning: unused import: `alloc::string::String`
 --> lib.rs:4:5
  |
4 | use alloc::string::String;
  |     ^^^^^^^^^^^^^^^^^^^^^
  |
  = note: `#[warn(unused_imports)]` on by default

warning: 1 warning emitted

    Finished test [unoptimized + debuginfo] target(s) in 0.08s
     Running target/debug/deps/mytest-c60315897dda6247

running 2 tests
test mytest::tests::default_works ... ok
test mytest::tests::it_works ... ok
```

Because \`String\` inside the \`mod\` used type from \`std\` when I ran
\`cargo test\` while \`build\` uses \`no\_std\` so it couldn't build.
Let's move \`alloc::string::String\` inside the \`mod\` to import
the \`String\` type we need in this contract code.

```rust
#![cfg_attr(not(feature = "std"), no_std)]

extern crate alloc;
use ink_lang as ink;

#[ink::contract]
mod mytest {
    use alloc::string::String;

    #[ink(storage)]
    pub struct Mytest {
	value: String,
    }

    impl Mytest {
```

Now it built:

```shell
$ cargo contract build && cargo contract generate-metadata
 [1/3] Building cargo project
    Finished release [optimized] target(s) in 0.11s
 [2/3] Post processing wasm file
 [3/3] Optimizing wasm file
wasm-opt is not installed. Install this tool on your system in order to
reduce the size of your contract's Wasm binary.
See https://github.com/WebAssembly/binaryen#tools

Your contract is ready. You can find it here:
/Users/aimeez/github/mytest/target/mytest.wasm
  Generating metadata
 [1/3] Building cargo project
    Finished release [optimized] target(s) in 0.06s
 [2/3] Post processing wasm file
 [3/3] Optimizing wasm file
wasm-opt is not installed. Install this tool on your system in order to
reduce the size of your contract's Wasm binary.
See https://github.com/WebAssembly/binaryen#tools
    Updating crates.io index
   Compiling metadata-gen v0.1.0 (/var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T/cargo-contract_fNuJ5a/.ink/metadata_gen)
    Finished release [optimized] target(s) in 2.09s
     Running `target/release/metadata-gen`
      Your metadata file is ready.
You can find it here:
/Users/aimeez/github/mytest/target/metadata.json
```


## 2020-11-20 The example: flipper {#2020-11-20-the-example-flipper}

[Creating an ink! Project](https://substrate.dev/substrate-contracts-workshop/#/0/creating-an-ink-project)

Run `cargo contract new flipper` again
(I already ran it last time):

```shell
$ cargo +nightly contract build
 [1/3] Building cargo project
   Compiling termcolor v1.1.2
   Compiling trybuild v1.0.35
   Compiling ink_lang_macro v3.0.0-rc2
   Compiling ink_lang v3.0.0-rc2
   Compiling flipper v0.1.0 (/var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T/cargo-contract_4mvqYY)
    Finished release [optimized] target(s) in 5.55s
 [2/3] Post processing wasm file
 [3/3] Optimizing wasm file
wasm-opt is not installed. Install this tool on your system in order to
reduce the size of your contract's Wasm binary.
See https://github.com/WebAssembly/binaryen#tools
```


### New tool learned {#new-tool-learned}

[cargo-expand](https://github.com/dtolnay/cargo-expand) by [dtolnay](https://github.com/dtolnay).

```shell
$ cargo expand --no-default-features
# Compiling...
error: ink! only support compilation as `std` or `no_std` + `wasm32-unknown`
  --> /Users/aimeez/.cargo/registry/src/github.com-1ecc6299db9ec823/ink_env-3.0.0-rc2/src/engine/mod.rs:39:9
   |
39 | /         compile_error! {
40 | |             "ink! only support compilation as `std` or `no_std` + `wasm32-unknown`"
41 | |         }
   | |_________^
error[E0432]: unresolved import `crate::engine::EnvInstance`
  --> /Users/aimeez/.cargo/registry/src/github.com-1ecc6299db9ec823/ink_env-3.0.0-rc2/src/api.rs:29:9
   |
29 |         EnvInstance,
   |         ^^^^^^^^^^^
   |         |
   |         no `EnvInstance` in `engine`
   |         help: a similar name exists in the module: `OnInstance`
error: aborting due to 2 previous errors
For more information about this error, try `rustc --explain E0432`.
error: could not compile `ink_env`
To learn more, run the command again with --verbose.
warning: build failed, waiting for other jobs to finish...
error: build failed
```

Update the command and it works:

```shell
$ cargo expand --no-default-features --target=wasm32-unknown-unknown

#![feature(prelude_import)]
#![no_std]
#[prelude_import]
use core::prelude::v1::*;
#[macro_use]
extern crate core;
#[macro_use]
extern crate compiler_builtins;
use ink_lang as ink;
mod flipper {
    impl ::ink_lang::ContractEnv for Flipper {
	type Env = ::ink_env::DefaultEnvironment;
    }
    type Environment = <Flipper as ::ink_lang::ContractEnv>::Env;
    type AccountId =
	<<Flipper as ::ink_lang::ContractEnv>::Env as ::ink_env::Environment>::AccountId;
    type Balance = <<Flipper as ::ink_lang::ContractEnv>::Env as ::ink_env::Environment>::Balance;
    type Hash = <<Flipper as ::ink_lang::ContractEnv>::Env as ::ink_env::Environment>::Hash;
    type Timestamp =
	<<Flipper as ::ink_lang::ContractEnv>::Env as ::ink_env::Environment>::Timestamp;
    type BlockNumber =
	<<Flipper as ::ink_lang::ContractEnv>::Env as ::ink_env::Environment>::BlockNumber;
    #[cfg(not(feature = "ink-as-dependency"))]
    const _: () = {
	impl<'a> ::ink_lang::Env for &'a Flipper {
	    type EnvAccess = ::ink_lang::EnvAccess<'a, <Flipper as ::ink_lang::ContractEnv>::Env>;
	    fn env(self) -> Self::EnvAccess {
		Default::default()
	    }
	}
	impl<'a> ::ink_lang::StaticEnv for Flipper {
	    type EnvAccess =
		::ink_lang::EnvAccess<'static, <Flipper as ::ink_lang::ContractEnv>::Env>;
	    fn env() -> Self::EnvAccess {
		Default::default()
	    }
	}
    };
    #[cfg(not(feature = "ink-as-dependency"))]
    /// Defines the storage of your contract.
    /// Add new fields to the below struct in order
    /// to add new static storage fields to your contract.
    pub struct Flipper {
	/// Stores a single `bool` value on the storage.
	value: bool,
    }
    const _: () = {
	impl ::ink_storage::traits::SpreadLayout for Flipper {
	    #[allow(unused_comparisons)]
	    const FOOTPRINT: u64 = [
		(0u64 + <bool as ::ink_storage::traits::SpreadLayout>::FOOTPRINT),
		0u64,
	    ][((0u64 + <bool as ::ink_storage::traits::SpreadLayout>::FOOTPRINT) < 0u64) as usize];
	    const REQUIRES_DEEP_CLEAN_UP: bool = (false
		|| (false
		    || <bool as ::ink_storage::traits::SpreadLayout>::REQUIRES_DEEP_CLEAN_UP));
	    fn pull_spread(__key_ptr: &mut ::ink_storage::traits::KeyPtr) -> Self {
		Flipper {
		    value: <bool as ::ink_storage::traits::SpreadLayout>::pull_spread(__key_ptr),
		}
	    }
	    fn push_spread(&self, __key_ptr: &mut ::ink_storage::traits::KeyPtr) {
		match self {
		    Flipper { value: __binding_0 } => {
			::ink_storage::traits::SpreadLayout::push_spread(__binding_0, __key_ptr);
		    }
		}
	    }
	    fn clear_spread(&self, __key_ptr: &mut ::ink_storage::traits::KeyPtr) {
		match self {
		    Flipper { value: __binding_0 } => {
			::ink_storage::traits::SpreadLayout::clear_spread(__binding_0, __key_ptr);
		    }
		}
	    }
	}
    };
    #[cfg(not(feature = "ink-as-dependency"))]
    const _: () = {
	#[allow(unused_imports)]
	use ::ink_lang::{Env as _, StaticEnv as _};
    };
    #[cfg(not(test))]
    #[cfg(not(feature = "ink-as-dependency"))]

    #...
    #more
```


## 2020-11-17 ink! {#2020-11-17-ink}


### Follow the docs {#follow-the-docs}

Start with
[substrate.dev/substrate-contracts-workshop](https://substrate.dev/substrate-contracts-workshop/#/0/introduction)

Install from the webpage's command, but build failed.

The existing issue in Cargo's repo:
<https://github.com/rust-lang/cargo/issues/7169>

Need to read Cargo book:
<https://doc.rust-lang.org/nightly/cargo/commands/cargo-install.html>

Keep following the doc: [Running a Canvas Node](https://substrate.dev/substrate-contracts-workshop/#/0/running-a-substrate-node).
Btw, I like the tutorials explain each file.
It's helpful to me.

```shell
$ canvas --dev --tmp
2020-11-17 17:20:43  Running in --dev mode, RPC CORS has been disabled.
2020-11-17 17:20:43  Canvas Node
2020-11-17 17:20:43  ✌️  version 0.1.0-e189090-x86_64-macos
2020-11-17 17:20:43  ❤️  by Canvas, 2020-2020
2020-11-17 17:20:43  📋 Chain specification: Development
2020-11-17 17:20:43  🏷 Node name: cute-example-7440
2020-11-17 17:20:43  👤 Role: AUTHORITY
2020-11-17 17:20:43  💾 Database: RocksDb at /var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T/substratePetkPI/chains/dev/db
2020-11-17 17:20:43  ⛓  Native runtime: canvas-8 (canvas-0.tx1.au1)
2020-11-17 17:20:43  🔨 Initializing Genesis block/state (state: 0x1611…971f, header-hash: 0x575c…6d5f)
2020-11-17 17:20:43  👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-11-17 17:20:43  ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-11-17 17:20:43  Using default protocol ID "sup" because none is configured in the chain specs
2020-11-17 17:20:43  🏷 Local node identity is: 12D3KooWSLjj5cAsQ8EeBvSRrxbg8b9mTzvoZVf7SA5NzkDzSCFx
2020-11-17 17:20:43  📦 Highest known block at #0
2020-11-17 17:20:43  〽️ Prometheus server started at 127.0.0.1:9615
2020-11-17 17:20:43  Listening for new connections on 127.0.0.1:9944.
2020-11-17 17:20:48  🙌 Starting consensus session on top of parent 0x575c06528df3f98a10aa6ac8d6d7c8f1d0ca9738206c05dc96516b1bcb836d5f
2020-11-17 17:20:48  🎁 Prepared block for proposing at 1 [hash: 0x8d82ffaef8eea6679896f4b8335a68771ab7add86a51959368030e6aad395e8a; parent_hash: 0x575c…6d5f; extrinsics (1): [0xdc0e…86a9]]
2020-11-17 17:20:48  🔖 Pre-sealed block for proposal at 1. Hash now 0x4b4fa8e91ef020d0544796b1dc9c26c046662b6bae182be5fa5548f9818863b4, previously 0x8d82ffaef8eea6679896f4b8335a68771ab7add86a51959368030e6aad395e8a.
2020-11-17 17:20:48  ✨ Imported #1 (0x4b4f…63b4)
2020-11-17 17:20:48  💤 Idle (0 peers), best: #1 (0x4b4f…63b4), finalized #0 (0x575c…6d5f), ⬇ 0 ⬆ 0
2020-11-17 17:20:53  💤 Idle (0 peers), best: #1 (0x4b4f…63b4), finalized #0 (0x575c…6d5f), ⬇ 0 ⬆ 0
2020-11-17 17:20:54  🙌 Starting consensus session on top of parent 0x4b4fa8e91ef020d0544796b1dc9c26c046662b6bae182be5fa5548f9818863b4
2020-11-17 17:20:54  🎁 Prepared block for proposing at 2 [hash: 0x42d318b1165e2217212499aad57c1d6c89637668fb5d02d482415ef8eaa9f4da; parent_hash: 0x4b4f…63b4; extrinsics (1): [0x029c…6c04]]
```


### Thoughts {#thoughts}

My experience with Polkadot, Substrate, and ink so far is pleasant.
The documentation is up to date enough with detailed step by step
descriptions. I can follow along smoothly.

There are some things I couldn't figure out at the first moment.
I realized that mostly because I am not familiar with Rust language and
its ecosystem. For example, if I know Cargo better, I would learn
to use `cargo install` and `cargo build` correctly with necessary arguments.


## 2020-11-15 Start {#2020-11-15-start}


### Follow the GitHub repo {#follow-the-github-repo}

```shell
$ WASM_BUILD_TOOLCHAIN=nightly-2020-10-05 cargo build --release
error: failed to run custom build command for `node-template-runtime v2.0.0 (/Users/aimeez/github/substrate-node-template/runtime)`

Caused by:
  process didn't exit successfully: `/Users/aimeez/github/substrate-node-template/target/release/build/node-template-runtime-329be64dd2778179/build-script-build` (exit code: 1)
  --- stderr
     Compiling wasm-build-runner-impl v1.0.0 (/Users/aimeez/github/substrate-node-template/target/release/wbuild-runner/node-template-runtime3117747485089870701)
      Finished release [optimized] target(s) in 0.54s
       Running `/Users/aimeez/github/substrate-node-template/target/release/wbuild-runner/node-template-runtime3117747485089870701/target/x86_64-apple-darwin/release/wasm-build-runner-impl`
  Rust nightly not installed, please install it!
warning: build failed, waiting for other jobs to finish...
error: build failed
```


### Install `nightly-2020-10-05` and build again {#install-nightly-2020-10-05-and-build-again}

```shell
$ rustup toolchain install nightly-2020-10-05

$ WASM_BUILD_TOOLCHAIN=nightly-2020-10-05 cargo build --release

error: failed to run custom build command for `node-template-runtime v2.0.0 (/Users/aimeez/github/substrate-node-template/runtime)`

Caused by:
  process didn't exit successfully: `/Users/aimeez/github/substrate-node-template/target/release/build/node-template-runtime-329be64dd2778179/build-script-build` (exit code: 1)
  --- stderr
     Compiling wasm-build-runner-impl v1.0.0 (/Users/aimeez/github/substrate-node-template/target/release/wbuild-runner/node-template-runtime3117747485089870701)
      Finished release [optimized] target(s) in 0.39s
       Running `/Users/aimeez/github/substrate-node-template/target/release/wbuild-runner/node-template-runtime3117747485089870701/target/x86_64-apple-darwin/release/wasm-build-runner-impl`
  Rust WASM toolchain not installed, please install it!

  Further error information:
  ------------------------------------------------------------
     Compiling wasm-test v1.0.0 (/var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T/.tmpWk51lL)
  error[E0463]: can't find crate for `std`
    |
    = note: the `wasm32-unknown-unknown` target may not be installed

  error: aborting due to previous error

  For more information about this error, try `rustc --explain E0463`.
  error: could not compile `wasm-test`

  To learn more, run the command again with --verbose.
  ------------------------------------------------------------

warning: build failed, waiting for other jobs to finish...
error: build failed
```


### Set target: {#set-target}

```shell
$ rustup target add wasm32-unknown-unknown --toolchain nightly-2020-10-05

error: failed to run custom build command for `node-template-runtime v2.0.0 (/Users/aimeez/github/substrate-node-template/runtime)`

Caused by:
  process didn't exit successfully: `/Users/aimeez/github/substrate-node-template/target/release/build/node-template-runtime-329be64dd2778179/build-script-build` (exit code: 1)
  --- stdout
  Executing build command: "/Users/aimeez/.rustup/toolchains/nightly-x86_64-apple-darwin/bin/cargo" "rustc" "--target=wasm32-unknown-unknown" "--manifest-path=/Users/aimeez/github/substrate-node-template/target/release/wbuild/node-template-runtime/Cargo.toml" "--color=always" "--release"

  --- stderr
     Compiling wasm-build-runner-impl v1.0.0 (/Users/aimeez/github/substrate-node-template/target/release/wbuild-runner/node-template-runtime3117747485089870701)
      Finished release [optimized] target(s) in 0.45s
       Running `/Users/aimeez/github/substrate-node-template/target/release/wbuild-runner/node-template-runtime3117747485089870701/target/x86_64-apple-darwin/release/wasm-build-runner-impl`
     Compiling sp-arithmetic v2.0.0
     Compiling sp-runtime-interface v2.0.0
     Compiling parity-util-mem v0.7.0
  error[E0282]: type annotations needed
```


### Built failed because my default setting is nightly but not stable. {#built-failed-because-my-default-setting-is-nightly-but-not-stable-dot}

```shell
$ rustc -V
rustc 1.50.0-nightly (98d66340d 2020-11-14)

$ rustup default stable

info: using existing install for 'stable-x86_64-apple-darwin'
info: default toolchain set to 'stable-x86_64-apple-darwin'

  stable-x86_64-apple-darwin unchanged - rustc 1.47.0 (18bf6b4f0 2020-10-07)
```


### It took 13 minutes to build: my laptop is slow... {#it-took-13-minutes-to-build-my-laptop-is-slow-dot-dot-dot}

```shell
$ WASM_BUILD_TOOLCHAIN=nightly-2020-10-05 cargo build --release

Finished release [optimized] target(s) in 13m 17s
```


### Cute run! {#cute-run}

```shell
$ ./target/release/node-template --dev --tmp
Nov 15 18:04:40.702  WARN Running in --dev mode, RPC CORS has been disabled.
Nov 15 18:04:40.703  INFO Substrate Node
Nov 15 18:04:40.703  INFO ✌️  version 2.0.0-24da767-x86_64-macos
Nov 15 18:04:40.703  INFO ❤️  by Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2020
Nov 15 18:04:40.703  INFO 📋 Chain specification: Development
Nov 15 18:04:40.703  INFO 🏷  Node name: super-top-6271
Nov 15 18:04:40.703  INFO 👤 Role: AUTHORITY
Nov 15 18:04:40.703  INFO 💾 Database: RocksDb at /var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T/substrate2jvpo0/chains/dev/db
Nov 15 18:04:40.703  INFO ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
Nov 15 18:04:40.755  INFO 🔨 Initializing Genesis block/state (state: 0xc29a…9e07, header-hash: 0x40ca…fc14)
Nov 15 18:04:40.756  INFO 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
Nov 15 18:04:40.774  INFO ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
Nov 15 18:04:40.774  WARN Using default protocol ID "sup" because none is configured in the chain specs
Nov 15 18:04:40.774  INFO 🏷  Local node identity is: 12D3KooWSMTDCBT4GHADWJxdRJTBnSKgEAXrekDVcwG6SuQy1az9 (legacy representation: 12D3KooWSMTDCBT4GHADWJxdRJTBnSKgEAXrekDVcwG6SuQy1az9)
Nov 15 18:04:41.071  INFO 📦 Highest known block at #0
Nov 15 18:04:41.072  INFO 〽️ Prometheus server started at 127.0.0.1:9615
Nov 15 18:04:41.073  INFO Listening for new connections on 127.0.0.1:9944.
Nov 15 18:04:42.012  INFO 🙌 Starting consensus session on top of parent 0x40ca582052a890e826eb0c3d3e5d9a1383f7cb95dd87d5b542b574040ea6fc14
Nov 15 18:04:42.017  INFO 🎁 Prepared block for proposing at 1 [hash: 0x384b4a0ce970b7b28dbc0764ef74ee3b3a55517c31476496db175845d03fe61e; parent_hash: 0x40ca…fc14; extrinsics (1): [0xab8e…deca]]
Nov 15 18:04:42.021  INFO 🔖 Pre-sealed block for proposal at 1. Hash now 0x753af28ba42e197ddf1df41477d452ac35cd3138afe70083f81e64637f51c1fd, previously 0x384b4a0ce970b7b28dbc0764ef74ee3b3a55517c31476496db175845d03fe61e.
Nov 15 18:04:42.021  INFO ✨ Imported #1 (0x753a…c1fd)
Nov 15 18:04:46.074  INFO 💤 Idle (0 peers), best: #1 (0x753a…c1fd), finalized #0 (0x40ca…fc14), ⬇ 0 ⬆ 0
Nov 15 18:04:48.010  INFO 🙌 Starting consensus session on top of parent 0x753af28ba42e197ddf1df41477d452ac35cd3138afe70083f81e64637f51c1fd
Nov 15 18:04:48.010  INFO 🎁 Prepared block for proposing at 2 [hash: 0x625c206bc45416b3745d544d93626a4cacaf74bf73c33cd11077edbeaaa95750; parent_hash: 0x753a…c1fd; extrinsics (1): [0xb6e9…2b6d]]
Nov 15 18:04:48.014  INFO 🔖 Pre-sealed block for proposal at 2. Hash now 0xe680ef911bd8a4c24ea2d7485255ca2cbe275cd51d0fa71dcc29846f84524d38, previously 0x625c206bc45416b3745d544d93626a4cacaf74bf73c33cd11077edbeaaa95750.
Nov 15 18:04:48.014  INFO ✨ Imported #2 (0xe680…4d38)
```


### Doc {#doc}

<https://substrate.dev/docs/en/tutorials/create-your-first-substrate-chain/interact>