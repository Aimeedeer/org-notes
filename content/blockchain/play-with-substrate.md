+++
title = "Play with Substrate"
author = ["Aimee Z"]
description = "Substrate is interesting and it seems powerful."
date = 2020-11-15
linkTitle = "blockchain"
tags = ["substrate", "blockchain", "hacking"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2004
  identifier = "play-with-substrate"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [2020-11-17 ink!](#2020-11-17-ink)
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


## 2020-11-17 ink! {#2020-11-17-ink}

Start with
[substrate.dev/substrate-contracts-workshop](https://substrate.dev/substrate-contracts-workshop/#/0/introduction)

Install from the webpage's command, but build failed.

Existing issue in Cargo's repo:
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