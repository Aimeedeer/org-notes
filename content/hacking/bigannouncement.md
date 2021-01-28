+++
title = "Project: The Big Announcement"
author = ["Aimee Z"]
description = "A tiny smart contract."
date = 2020-12-07
tags = ["substrate", "ethereum", "smartcontract"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2003
  identifier = "project-the-big-announcement"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Project intro](#project-intro)
- [TBA on Ethereum](#tba-on-ethereum)
- [TBA's smart contract on Substrate](#tba-s-smart-contract-on-substrate)
    - [Ink code (WIP)](#ink-code--wip)
    - [Substrate Hacklog](#substrate-hacklog)
- [TBA on Dfinity](#tba-on-dfinity)
    - [Motoko code](#motoko-code)
    - [Dfinity Hacklog](#dfinity-hacklog)
    - [Using Rust to write TBA](#using-rust-to-write-tba)
    - [Dfinity references](#dfinity-references)

</div>
<!--endtoc-->


## Project intro {#project-intro}

The Big Announcement project on different chains.

TODO:

-   [ ] TBA on Substrate
-   [ ] TBA on Dfinity
-   [ ] TBA on Solana
-   [ ] TBA on NEAR
-   [ ] TBA on Nervos


## TBA on Ethereum {#tba-on-ethereum}

-   Source code: [bigannouncement.eth](https://github.com/Aimeedeer/bigannouncement)
    -   [Solidity contract](https://github.com/Aimeedeer/bigannouncement/blob/master/contracts/BigAnnouncement.sol)
-   Ethereum [Hacklog](https://github.com/Aimeedeer/bigannouncement/blob/master/doc/hacklog.md)


## TBA's smart contract on Substrate {#tba-s-smart-contract-on-substrate}


### Ink code (WIP) {#ink-code--wip}

[tba-substrate](https://github.com/Aimeedeer/tba-substrate)


### Substrate Hacklog {#substrate-hacklog}


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


## TBA on Dfinity {#tba-on-dfinity}


### Motoko code {#motoko-code}

[TBA Motoko](https://github.com/brson/rust-contract-comparison/tree/master/code/tba-dfinty/tba-motoko)


### Dfinity Hacklog {#dfinity-hacklog}

Previous log is on [Gist](https://gist.github.com/Aimeedeer/aae0d6759fb04bb30478cbf38d4d46ce).

For building TBA, I started with its example [Echo](https://github.com/dfinity/examples/tree/master/motoko/echo).
The README.md seems to be out of date --
it starts a node `dfx start` before creating a project,
while the tutorials, [Start the local network](https://sdk.dfinity.org/docs/quickstart/local-quickstart.html#start-the-local-network), need you to start
a node inside a project.
I don't think I would use Echo now.

I'll create a tba folder, and start the node:

```shell
$ dfx new tba
Fetching manifest https://sdk.dfinity.org/manifest.json
⠁ Checking for latest dfx version...
You seem to be running an outdated version of dfx.

You are strongly encouraged to upgrade by running 'dfx upgrade'!
Creating new project "tba"...
CREATE       tba/src/tba_assets/assets/sample-asset.txt (24B)...
CREATE       tba/src/tba/main.mo (107B)...
CREATE       tba/dfx.json (454B)...
CREATE       tba/.gitignore (165B)...
⠤ Checking for latest dfx version...
CREATE       tba/src/tba_assets/public/index.js (131B)...
CREATE       tba/package.json (282B)...
CREATE       tba/webpack.config.js (2.15KB)...
⠉ Checking for latest dfx version...
⠂ Installing node dependencies...
⠴ Installing node dependencies...
⠐ Checking for latest dfx version...
⠈ Checking for latest dfx version...

> fsevents@1.2.13 install /<my_path>/tba/node_modules/watchpack-chokidar2/node_modules/fsevents
> node install.js
⠦ Checking for latest dfx version...
⠙ Installing node dependencies...
⠤ Checking for latest dfx version...
⠂ Installing node dependencies...
npm WARN tba_assets@0.1.0 No repository field.
npm WARN tba_assets@0.1.0 No license field.

⠲ Installing node dependencies...

13 packages are looking for funding
  run `npm fund` for details

found 1 high severity vulnerability
  Done.
⠐ Checking for latest dfx version...

===============================================================================
        Welcome to the internet computer developer community!
                        You're using dfx 0.6.20

            ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄                ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
          ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄          ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
        ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄      ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
       ▄▄▄▄▄▄▄▄▄▄▀▀▀▀▀▄▄▄▄▄▄▄▄▄▄▄▄  ▄▄▄▄▄▄▄▄▄▄▄▄▀▀▀▀▀▀▄▄▄▄▄▄▄▄▄▄
      ▄▄▄▄▄▄▄▄▀         ▀▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▀         ▀▄▄▄▄▄▄▄▄▄
     ▄▄▄▄▄▄▄▄▀            ▀▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▀             ▄▄▄▄▄▄▄▄
     ▄▄▄▄▄▄▄▄               ▀▄▄▄▄▄▄▄▄▄▄▄▄▀                ▄▄▄▄▄▄▄
     ▄▄▄▄▄▄▄▄                ▄▄▄▄▄▄▄▄▄▄▄▄                 ▄▄▄▄▄▄▄
     ▄▄▄▄▄▄▄▄               ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄              ▄▄▄▄▄▄▄▄
      ▄▄▄▄▄▄▄▄           ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄          ▄▄▄▄▄▄▄▄▀
      ▀▄▄▄▄▄▄▄▄▄▄     ▄▄▄▄▄▄▄▄▄▄▄▄▀ ▀▄▄▄▄▄▄▄▄▄▄▄▄    ▄▄▄▄▄▄▄▄▄▄▄
       ▀▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▀     ▀▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▀
         ▀▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▀         ▀▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
           ▀▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▀▀             ▀▀▄▄▄▄▄▄▄▄▄▄▄▄▄▄▀
              ▀▀▀▀▀▀▀▀▀▀▀                    ▀▀▀▀▀▀▀▀▀▀▀



To learn more before you start coding, see the documentation available online:

- Quick Start: https://sdk.dfinity.org/docs/quickstart/quickstart-intro.html
- SDK Developer Tools: https://sdk.dfinity.org/docs/developers-guide/sdk-guide.html
- Motoko Language Guide: https://sdk.dfinity.org/docs/language-guide/motoko.html
- Motoko Quick Reference: https://sdk.dfinity.org/docs/language-guide/language-manual.html

If you want to work on programs right away, try the following commands to get started:

    cd tba
    dfx help
    dfx new --help

===============================================================================
```

By the way, I follow the suggestion to upgrade my `dfx`,
and it seems it doesn't do anything useful:

```shell
$ dfx upgrade
Current version: 0.6.20
Fetching manifest https://sdk.dfinity.org/manifest.json
Already up to date

$ dfx --version
dfx 0.6.20
```

I start another terminal window for the node:

```shell
$ dfx start
Jan 27 16:01:01.798 INFO ic-starter. Configuration: ValidatedConfig { replica_path: Some("/Users/aimeez/.cache/dfinity/versions/0.6.20/replica"), replica_version: "0.1.0", log_level: Warning, subnet_id: fscpm-uiaaa-aaaaa-aaaap-yai, cargo_bin: "cargo", cargo_opts: "", state_dir: "/<my_path>/tba/.dfx/state/replicated_state", http_listen_addr: V4(127.0.0.1:0), http_port_file: Some("/<my_path>/tba/.dfx/replica-configuration/replica-1.port"), metrics_addr: None, hypervisor_create_funds_whitelist: "*", artifact_pool_dir: "/<my_path>/tba/.dfx/state/replicated_state/node-100/ic_consensus_pool", crypto_root: "/<my_path>/tba/.dfx/state/replicated_state/node-100/crypto", state_manager_root: "/<my_path>/tba/.dfx/state/replicated_state/node-100/state", registry_file: "/<my_path>/tba/.dfx/state/replicated_state/registry.proto", bootstrap_registry: None, state_dir_holder: None }, Application: starter
Jan 27 16:01:01.799 INFO Initialize replica configuration "/<my_path>/tba/.dfx/state/replicated_state/ic.json5", Application: starter
Jan 27 16:01:01.816 INFO Executing "/Users/aimeez/.cache/dfinity/versions/0.6.20/replica" "--replica-version" "0.1.0" "--config-file" "/<my_path>/tba/.dfx/state/replicated_state/ic.json5", Application: starter
Jan 27 16:01:06.664 ERRO s:fscpm-uiaaa-aaaaa-aaaap-yai/n:owpmo-ykels-vijqd-6ql2q-gklnb-6oii4-2zkce-54jdn-vt7ob-hrn7h-hae/ic_messaging/xnet_endpoint No XNet configuration for node owpmo-ykels-vijqd-6ql2q-gklnb-6oii4-2zkce-54jdn-vt7ob-hrn7h-hae. This is an error in production, but may be ignored in single-subnet test deployments.
Jan 27 16:01:07.671 WARN s:fscpm-uiaaa-aaaaa-aaaap-yai/n:owpmo-ykels-vijqd-6ql2q-gklnb-6oii4-2zkce-54jdn-vt7ob-hrn7h-hae/ic_http_handler/ic_http_handler NNS subnet not found in network topology. Skipping fetching the delegation.
Starting webserver on port 61644 for replica at "http://localhost:61644"
binding to: V4(127.0.0.1:8000)
replica(s): http://localhost:61644/
```

I go back to the first terminal window and deploy (before `build` that I usually do) it.
Now there comes an error:

```shell
$ dfx deploy
Deploying all canisters.
Creating canisters...
Creating canister "tba"...
"tba" canister created with canister id: "rwlgt-iiaaa-aaaaa-aaaaa-cai"
Creating canister "tba_assets"...
"tba_assets" canister created with canister id: "rrkah-fqaaa-aaaaa-aaaaq-cai"
Building canisters...
Building frontend...
The post-build step failed for canister 'rrkah-fqaaa-aaaaa-aaaaq-cai' with an embedded error: The command '"npm" "run" "build"' failed with exit status 'exit code: 1'.
Stdout:

> tba_assets@0.1.0 build /<my_path>/tba
> webpack


Stderr:
/<my_path>/tba/node_modules/terser-webpack-plugin/dist/index.js:572
      const hooks = compiler.webpack.javascript.JavascriptModulesPlugin.getCompilationHooks(compilation);
                                     ^

TypeError: Cannot read property 'javascript' of undefined
    at compiler.hooks.compilation.tap.compilation (/<my_path>/tba/node_modules/terser-webpack-plugin/dist/index.js:572:38)
    at SyncHook.eval [as call] (eval at create (/<my_path>/tba/node_modules/tapable/lib/HookCodeFactory.js:19:10), <anonymous>:85:1)
    at SyncHook.lazyCompileHook (/<my_path>/tba/node_modules/tapable/lib/Hook.js:154:20)
    at Compiler.newCompilation (/<my_path>/tba/node_modules/webpack/lib/Compiler.js:631:26)
    at hooks.beforeCompile.callAsync.err (/<my_path>/tba/node_modules/webpack/lib/Compiler.js:667:29)
    at AsyncSeriesHook.eval [as callAsync] (eval at create (/<my_path>/tba/node_modules/tapable/lib/HookCodeFactory.js:33:10), <anonymous>:6:1)
    at AsyncSeriesHook.lazyCompileHook (/<my_path>/tba/node_modules/tapable/lib/Hook.js:154:20)
    at Compiler.compile (/<my_path>/tba/node_modules/webpack/lib/Compiler.js:662:28)
    at readRecords.err (/<my_path>/tba/node_modules/webpack/lib/Compiler.js:321:11)
    at Compiler.readRecords (/<my_path>/tba/node_modules/webpack/lib/Compiler.js:529:11)
    at hooks.run.callAsync.err (/<my_path>/tba/node_modules/webpack/lib/Compiler.js:318:10)
    at AsyncSeriesHook.eval [as callAsync] (eval at create (/<my_path>/tba/node_modules/tapable/lib/HookCodeFactory.js:33:10), <anonymous>:6:1)
    at AsyncSeriesHook.lazyCompileHook (/<my_path>/tba/node_modules/tapable/lib/Hook.js:154:20)
    at hooks.beforeRun.callAsync.err (/<my_path>/tba/node_modules/webpack/lib/Compiler.js:315:19)
    at AsyncSeriesHook.eval [as callAsync] (eval at create (/<my_path>/tba/node_modules/tapable/lib/HookCodeFactory.js:33:10), <anonymous>:15:1)
    at AsyncSeriesHook.lazyCompileHook (/<my_path>/tba/node_modules/tapable/lib/Hook.js:154:20)
    at Compiler.run (/<my_path>/tba/node_modules/webpack/lib/Compiler.js:312:24)
    at runWithDependencies (/<my_path>/tba/node_modules/webpack/lib/MultiCompiler.js:265:15)
    at asyncLib.map (/<my_path>/tba/node_modules/webpack/lib/MultiCompiler.js:185:6)
    at arrayEachIndex (/<my_path>/tba/node_modules/neo-async/async.js:2548:9)
    at Object.map (/<my_path>/tba/node_modules/neo-async/async.js:2900:9)
    at runCompilers (/<my_path>/tba/node_modules/webpack/lib/MultiCompiler.js:182:13)
    at MultiCompiler.runWithDependencies (/<my_path>/tba/node_modules/webpack/lib/MultiCompiler.js:194:3)
    at MultiCompiler.run (/<my_path>/tba/node_modules/webpack/lib/MultiCompiler.js:261:9)
    at processOptions (/<my_path>/tba/node_modules/webpack-cli/bin/cli.js:353:14)
    at yargs.parse (/<my_path>/tba/node_modules/webpack-cli/bin/cli.js:364:3)
    at Object.parse (/<my_path>/tba/node_modules/yargs/yargs.js:567:18)
    at /<my_path>/tba/node_modules/webpack-cli/bin/cli.js:49:8
    at Object.<anonymous> (/<my_path>/tba/node_modules/webpack-cli/bin/cli.js:366:3)
    at Module._compile (internal/modules/cjs/loader.js:778:30)
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! tba_assets@0.1.0 build: `webpack`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the tba_assets@0.1.0 build script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
```

I build directly, and still an error:

```shell
$ dfx build
Cannot find canister id. Please issue 'dfx canister create tba_assets'.
```

I don't know what to do with this error while checking back with
Dfinity's documentation. So I `rm -rf tba` and then
use `dfx new tba` to re-create tba.
Now it works:

```shell
$ dfx deploy
Deploying all canisters.
Creating canisters...
Creating canister "tba"...
"tba" canister created with canister id: "rwlgt-iiaaa-aaaaa-aaaaa-cai"
Creating canister "tba_assets"...
"tba_assets" canister created with canister id: "rrkah-fqaaa-aaaaa-aaaaq-cai"
Building canisters...
Building frontend...
Installing canisters...
Installing code for canister tba, with canister_id rwlgt-iiaaa-aaaaa-aaaaa-cai
Installing code for canister tba_assets, with canister_id rrkah-fqaaa-aaaaa-aaaaq-cai
Deployed canisters.
```

Test it:

```shell
$ dfx build
Building canisters...
Building frontend...

$ dfx canister call tba greet me
("Hello, me!")
```

My processes are:

-   Create new project with `dfx new <your project>`
-   Go into the project folder `cd <your project>`
-   Install npm with `npm install`
-   Open another terminal window, go to the project folder, and run a local node with `dfx start`
-   Go back to previous terminal window, and deploy my project: `dfx deploy`.

It seems that the deployment operation also builds the project before deploy it.

Now, I am going to write some code (change the default one).
The default code in `tba/src/tba/main.mo` is:

```js
actor {
    public func greet(name : Text) : async Text {
        return "Hello, " # name # "!";
    };
};
```

The Motoko seems to be easy to use, its syntax looks strange, though.

My contract code is:

```js
actor TBA {
    stable var tba_msg = "The Big Announcement";

    public func set_message(msg : Text) : async Text {
        tba_msg := msg;
        return "Hello, " # tba_msg # "!";
    };

    public func set_message_no_return(msg : Text) {
        tba_msg := msg;
    };

    public func loading_message() : async Text {
        return tba_msg;
    };
};
```

Test with command line:

```shell
$ dfx canister call tba set_message My_Big_Announcement_Test
("Hello, My_Big_Announcement_Test!")

$ dfx canister call tba set_message_no_return My_Big_Announcement_Test_No_Return
()

$ dfx canister call tba loading_message
("My_Big_Announcement_Test_No_Return")
```

I have to mention that error messages are not useful.
I try to change some code and build failed.
The error messages are:

```shell
$ dfx build
Building canisters...
Building frontend...
The build step failed for canister 'rwlgt-iiaaa-aaaaa-aaaaa-cai' with an embedded error: The command '"/Users/aimeez/.cache/dfinity/versions/0.6.20/moc" "/<my_path>/tba/src/tba/main.mo" "-o" "/<my_path>/tba/.dfx/local/canisters/tba/tba.did" "--idl" "--actor-idl" "/<my_path>/tba/.dfx/local/canisters/idl/" "--actor-alias" "tba" "rwlgt-iiaaa-aaaaa-aaaaa-cai" "--actor-alias" "tba_assets" "rrkah-fqaaa-aaaaa-aaaaq-cai" "--package" "base" "/Users/aimeez/.cache/dfinity/versions/0.6.20/base"' failed with exit status 'exit code: 1'.
Stdout:

Stderr:
/<my_path>/tba/src/tba/main.mo:4.21-4.25: type error, unbound type Test
```

I have no clue where to start debugging, or guessing.
Not fun.


### Using Rust to write TBA {#using-rust-to-write-tba}

Official tutorials: [Using Rust](https://sdk.dfinity.org/docs/developers-guide/work-with-languages.html)

I'm glad I can use Rust command directly to create a project.
But, that's all.

```shell
$ cargo new tba_rust
     Created binary (application) `tba_rust` package
Aimees-MacBook-Pro:dfinity-project aimeez$ cd tba_rust/
Aimees-MacBook-Pro:tba_rust aimeez$ dfx build
Command must be run in a project directory (with a dfx.json file).
Aimees-MacBook-Pro:tba_rust aimeez$ cargo build
   Compiling tba_rust v0.1.0 (/<my_path>/tba_rust)
    Finished dev [unoptimized + debuginfo] target(s) in 3.51s
```

Then I need to copy `Cargo.toml` to another place according the tutorials:

> Projects that run on the Internet Computer typically use
one project-level `Cargo.toml` file to set up a workspace
for the canister members of the project and a second `Cargo.toml` file
in the source code directory to configure settings for each canister.

After finishing the basic round of configuration, I use `cargo build`
and there is an error:

```shell
$ cargo build
error: failed to read `/<my_path>/tba_rust/src/rba_rust/Cargo.toml`

Caused by:
  No such file or directory (os error 2)
```

The path is incorrect, and I change it in the root `Cargo.toml` file:

```rust
[workspace]
members = [
    "src",
]
```

`cargo build` works but `dfx build` doesn't work (I have started `dfx start`
in another terminal window). The error is:

```shell
$ cargo build
   Compiling tba_rust v0.1.0 (/<my_path>/tba_rust/src)

$ dfx build
Building canisters...
Executing 'cargo build --target wasm32-unknown-unknown --package tba_rust'
   Compiling tba_rust v0.1.0 (/<my_path>/tba_rust/src)
warning: function is never used: `main`
 --> src/main.rs:1:4
  |
1 | fn main() {
  |    ^^^^
  |
  = note: `#[warn(dead_code)]` on by default

warning: 1 warning emitted

    Finished dev [unoptimized + debuginfo] target(s) in 0.23s
The post-build step failed for canister 'rwlgt-iiaaa-aaaaa-aaaaa-cai' with an embedded error: No such file or directory (os error 2)
```


### Dfinity references {#dfinity-references}

[A Technical Overview of the Internet Computer](https://medium.com/dfinity/a-technical-overview-of-the-internet-computer-f57c62abc20f)

[The Internet Computer’s Token Economics: An Overview](https://medium.com/dfinity/the-internet-computers-token-economics-an-overview-29e238bd1d83)

> You can do two things with ICP tokens:
>
> -  Lock them inside the NNS to create “neurons,” which can vote on proposals and earn voting rewards.
> -  Convert them into cycles, which are used to power computation by software canisters running on the Internet Computer.
>
> Cycles
>
> Software canisters that run on the Internet Computer must be charged with cycles, which are used to
power computations and memory management, and are burned in the process.
The conversion of ICP tokens to cycles occurs at a variable rate, which is
constantly configured by the NNS in response to external markets.

[Sample Apps](https://sdk.dfinity.org/docs/developers-guide/sample-apps.html)

[Dfinity Motoko Programming Language](https://sdk.dfinity.org/docs/language-guide/motoko.html)

> Motoko lets you declare certain variables as `stable`.
The values of `stable` variables are automatically preserved across software upgrades.

It's weird to use another name `stable` to replace a name that is in consensus `static`.

[Candid Specification](https://sdk.dfinity.org/docs/candid-spec/idl)

It brings some design thoughts, especially the section [Services](https://sdk.dfinity.org/docs/candid-spec/idl#%5Fservices).
An interesting read.

[Using Rust](https://sdk.dfinity.org/docs/developers-guide/work-with-languages.html)

[Crate candid](https://docs.rs/candid/0.6.13/candid/)

Forum post: [Developer Experience, January 2021](https://forum.dfinity.org/t/developer-experience-january-2021/1749)