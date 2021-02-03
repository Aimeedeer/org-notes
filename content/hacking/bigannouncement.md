+++
title = "Project: The Big Announcement"
author = ["Aimee Z"]
description = "A tiny smart contract."
date = 2020-12-07
tags = ["substrate", "ethereum", "smartcontract"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2004
  identifier = "project-the-big-announcement"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Project intro and status](#project-intro-and-status)
- [TBA on Ethereum](#tba-on-ethereum)
- [TBA on Substrate](#tba-on-substrate)
    - [Ink code](#ink-code)
    - [Substrate Hacklog](#substrate-hacklog)
- [TBA on Dfinity](#tba-on-dfinity)
    - [Motoko code](#motoko-code)
    - [Dfinity Hacklog](#dfinity-hacklog)
    - [Using Rust to write TBA](#using-rust-to-write-tba)
    - [Dfinity references](#dfinity-references)
- [TBA on Solana](#tba-on-solana)
    - [Solana hacklog](#solana-hacklog)
- [TBA on Aleo](#tba-on-aleo)
    - [Aleo hacklog](#aleo-hacklog)
    - [Aleo references](#aleo-references)

</div>
<!--endtoc-->


## Project intro and status {#project-intro-and-status}

The Big Announcement project on different chains.

TODO:

-   [ ] TBA on Substrate (WIP)
-   [ ] TBA on Solana (WIP)
-   [ ] TBA on NEAR
-   [ ] TBA on Nervos
-   [ ] TBA on Aleo
-   [ ] TBA on Concordium


## TBA on Ethereum {#tba-on-ethereum}

-   Source code: [bigannouncement.eth](https://github.com/Aimeedeer/bigannouncement)
    -   [Solidity contract](https://github.com/Aimeedeer/bigannouncement/blob/master/contracts/BigAnnouncement.sol)
-   Ethereum [Hacklog](https://github.com/Aimeedeer/bigannouncement/blob/master/doc/hacklog.md)


## TBA on Substrate {#tba-on-substrate}


### Ink code {#ink-code}

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
error: failed to read `/<my_path>/tba_rust/src/tba_rust/Cargo.toml`

Caused by:
  No such file or directory (os error 2)
```

The path is incorrect, and I change it in the root `Cargo.toml` file:

```rust
[workspace]
members = [
    "src/",
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

However, the config
`members = ["src/",]`
isn't right according to Rust configuration.
I create another folder `tba_rust` under `/<my_path>/tba_rust/src/`,
and move all the files from `src` to `src/tba_rust`.
Then I add `cdylib` to my `/<my_path>/tba_rust/src/Cargo.toml` file.

```rust
[lib]
path = "main.rs"
crate-type = ["cdylib"]
```

Now both `cargo build` and `dfx build` work.
The terminal prints:

```shell
$ dfx build
Building canisters...
Executing 'cargo build --target wasm32-unknown-unknown --package tba_rust'
   Compiling tba_rust v0.1.0 (/<my_path>/tba_rust/src/tba_rust)
warning: function is never used: `main`
 --> src/tba_rust/main.rs:1:4
  |
1 | fn main() {
  |    ^^^^
  |
  = note: `#[warn(dead_code)]` on by default

warning: 1 warning emitted

    Finished dev [unoptimized + debuginfo] target(s) in 1.37s
```

**The documentation has been misguiding so far.**

Deploy it:

```shell
$ dfx canister install --all
Installing code for canister tba_rust, with canister_id rwlgt-iiaaa-aaaaa-aaaaa-cai
The Replica returned an error: code 5, message: "Canister rwlgt-iiaaa-aaaaa-aaaaa-cai cannot be installed because the canister is not empty. Try installing with mode='reinstall' instead."

$ dfx canister install --mode reinstall
error: The following required arguments were not provided:
    --all

USAGE:
    dfx canister install --mode <mode> --all

For more information try --help

$ dfx canister install --mode reinstall --all
Installing code for canister tba_rust, with canister_id rwlgt-iiaaa-aaaaa-aaaaa-cai
```

I am not going to move forward since there is no more tutorials about Rust programming on Dfinity.


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


## TBA on Solana {#tba-on-solana}


### Solana hacklog {#solana-hacklog}

Doc: <https://docs.solana.com/cli/install-solana-cli-tools>

Install Solana:

```shell
$ sh -c "$(curl -sSfL https://release.solana.com/v1.5.5/install)"
downloading v1.5.5 installer
Configuration: /<my_path>/.config/solana/install/config.yml
Active release directory: /<my_path>/solana/install/active_release
• Release version: 1.5.5
• Release URL: https://github.com/solana-labs/solana/releases/download/v1.5.5/solana-release-x86_64-apple-darwin.tar.bz2
  ✨ Update successful
Adding export PATH="/<my_path>/solana/install/active_release/bin:$PATH" to /<my_other_path>/.profile
Adding export PATH="/<my_path>/solana/install/active_release/bin:$PATH" to /<my_other_path>/.bash_profile

Close and reopen your terminal to apply the PATH changes or run the following in your existing shell:
  export PATH="/<my_path>/solana/install/active_release/bin:$PATH"
```

Run `export PATH` as the output says:

```shell
$ export PATH="/<my_path>/solana/install/active_release/bin:$PATH"
```

Close the current terminal and open a new one.
Check Solana:

```shell
$ solana --version
solana-cli 1.5.5 (src:10e12d14; feat:1043916741)
```

Try `update` command:

```shell
$ solana-install update
Configuration: /<my_path>/.config/solana/install/config.yml
Active release directory: /<my_path>/.local/share/solana/install/active_release
• Release version: 1.5.5
• Release URL: https://github.com/solana-labs/solana/releases/download/v1.5.5/solana-release-x86_64-apple-darwin.tar.bz2
1.5.5 is present, no download required.
  ✨ Update successful
```

I use a pre-build version, so I download it as the doc says:

```shell
$ cd solana-release/
$ ls
bin		version.yml

$ export PATH=$PWD/bin:$PATH
```

`solana --help` shows a huge list of commands:

```shell
$ solana --help
solana-cli 1.4.25 (src:893cc764; feat:2339469691)
Blockchain, Rebuilt for Scale

USAGE:
    solana [FLAGS] [OPTIONS] <SUBCOMMAND>

FLAGS:
    -h, --help                           Prints help information
        --no-address-labels              Do not use address labels in the output
        --skip-seed-phrase-validation    Skip validation of seed phrases. Use this if your phrase does not use the BIP39
                                         official English word list
    -V, --version                        Prints version information
    -v, --verbose                        Show additional information

OPTIONS:
        --commitment <COMMITMENT_LEVEL>    Return information at the selected commitment level [possible values: recent,
                                           single, singleGossip, root, max]
    -C, --config <FILEPATH>                Configuration file to use [default:
                                           /<my_path>/.config/solana/cli/config.yml]
    -u, --url <URL_OR_MONIKER>             URL for Solana's JSON RPC or moniker (or their first letter): [mainnet-beta,
                                           testnet, devnet, localhost]
    -k, --keypair <KEYPAIR>                Filepath or URL to a keypair
        --output <FORMAT>                  Return information in specified output format [possible values: json, json-
                                           compact]
        --ws <URL>                         WebSocket URL for the solana cluster

SUBCOMMANDS:
    account                        Show the contents of an account
    address                        Get your public key
    airdrop                        Request lamports
    authorize-nonce-account        Assign account authority to a new entity
    balance                        Get your balance
    block                          Get a confirmed block
    block-height                   Get current block height
    block-production               Show information about block production
    block-time                     Get estimated production time of a block
    catchup                        Wait for a validator to catch up to the cluster
    cluster-date                   Get current cluster date, computed from genesis creation time and network time
    cluster-version                Get the version of the cluster entrypoint
    config                         Solana command-line tool configuration settings
    confirm                        Confirm transaction by signature
    create-address-with-seed       Generate a derived account address with a seed
    create-nonce-account           Create a nonce account
    create-stake-account           Create a stake account
    create-vote-account            Create a vote account
    deactivate-stake               Deactivate the delegated stake from the stake account
    decode-transaction             Decode a base-58 binary transaction
    delegate-stake                 Delegate stake to a vote account
    deploy                         Deploy a program
    epoch                          Get current epoch
    epoch-info                     Get information about the current epoch
    feature                        Runtime feature management
    fees                           Display current cluster fees
    first-available-block          Get the first available block in the storage
    genesis-hash                   Get the genesis hash
    gossip                         Show the current gossip network nodes
    help                           Prints this message or the help of the given subcommand(s)
    inflation                      Show inflation information
    largest-accounts               Get addresses of largest cluster accounts
    leader-schedule                Display leader schedule
    live-slots                     Show information about the current slot progression
    logs                           Stream transaction logs
    merge-stake                    Merges one stake account into another
    new-nonce                      Generate a new nonce, rendering the existing nonce useless
    nonce                          Get the current nonce value
    nonce-account                  Show the contents of a nonce account
    pay                            Deprecated alias for the transfer command
    ping                           Submit transactions sequentially
    program                        Program management
    rent                           Calculate per-epoch and rent-exempt-minimum values for a given account data
                                   length.
    resolve-signer                 Checks that a signer is valid, and returns its specific path; useful for signers
                                   that may be specified generally, eg. usb://ledger
    slot                           Get current slot
    split-stake                    Duplicate a stake account, splitting the tokens between the two
    stake-account                  Show the contents of a stake account
    stake-authorize                Authorize a new signing keypair for the given stake account
    stake-history                  Show the stake history
    stake-set-lockup               Set Lockup for the stake account
    stakes                         Show stake account information
    supply                         Get information about the cluster supply of SOL
    transaction-count              Get current transaction count
    transaction-history            Show historical transactions affecting the given address from newest to oldest
    transfer                       Transfer funds between system accounts
    validator-info                 Publish/get Validator info on Solana
    validators                     Show summary information about the current validators
    vote-account                   Show the contents of a vote account
    vote-authorize-voter           Authorize a new vote signing keypair for the given vote account
    vote-authorize-withdrawer      Authorize a new withdraw signing keypair for the given vote account
    vote-update-commission         Update the vote account's commission
    vote-update-validator          Update the vote account's validator identity
    wait-for-max-stake             Wait for the max stake of any one node to drop below a percentage of total.
    withdraw-from-nonce-account    Withdraw SOL from the nonce account
    withdraw-from-vote-account     Withdraw lamports from a vote account into a specified account
    withdraw-stake                 Withdraw the unstaked SOL from the stake account
```

And then a lot of wallet explanations:
[Using Solana CLI](https://docs.solana.com/cli/conventions)


## TBA on Aleo {#tba-on-aleo}

TODO:

-   [ ] Next, change the default code: <https://developer.aleo.org/developer/getting%5Fstarted/syntax>
-   [ ] Create TBA program


### Aleo hacklog {#aleo-hacklog}

**Install Aleo**

```shell
$ cargo install aleo
    Updating crates.io index
  Downloaded aleo v0.2.0
  ...
   Compiling aleo v0.2.0
    Finished release [optimized] target(s) in 3m 50s
  Installing /Users/aimeez/.cargo/bin/aleo                                       Installed package `aleo v0.2.0` (executable `aleo`)

$ aleo
aleo 0.2.0
The Aleo Team <hello@aleo.org>

USAGE:
    aleo [FLAGS] <SUBCOMMAND>

FLAGS:
    -d, --debug      Enable debug mode
    -h, --help       Prints help information
    -V, --version    Prints version information
    -v, --verbose    Enable verbose mode

SUBCOMMANDS:
    help    Prints this message or the help of the given subcommand(s)
    new     Generates a new Aleo account
```

Test some commands:

```shell
aleo new --seed testtest
error: Invalid value for '--seed <seed>': invalid digit found in string
Aimees-MacBook-Pro:aleo aimeez$ aleo new --seed <my_test_seed>

  Private Key  APrivateKey1yg4Nkz7waZ4Smte38XSg9a4tH7vydLCQCr2GPr7QArjhmiQ # only for test purposes
     View Key  AViewKey1cXkqYJ3exGTVhrMCJrQPCwcGFnsb1d153HSGk5U5b7Q4
      Address  aleo1ztpasdk5acqz2nphfcn8mz3dyd9d5z23kjun04r7vy0gqfszrsyq2tw9qk

$ cargo run --release --example dummy_transaction
error: failed to parse lock file at: /<my_path>/aleo/Cargo.lock

Caused by:
  package `bytes` is specified twice in the lockfile
```

There are more errors because some packages are specified twice in the lockfile.
The lockfile is generated by cargo and is not supposed to be edited manually.
So I delete `Cargo.lock` file, and run the example again.
Now it built.
(Read cargo book: [Cargo.toml vs Cargo.lock](https://doc.rust-lang.org/cargo/guide/cargo-toml-vs-cargo-lock.html)).

```shell
$ rm Cargo.lock
$ cargo clean
$ cargo run --release --example dummy_transaction
    Updating crates.io index
  Downloaded httparse v1.3.5
  ...
   Compiling aleo v0.2.0 (/<my_path>/aleo)
    Finished release [optimized] target(s) in 5m 00s
     Running `target/release/examples/dummy_transaction`

WARNING - "outer_snark_pk-12b0f50.params" does not exist. snarkVM will download this file remotely and store it locally. Please ensure "outer_snark_pk-12b0f50.params" is stored in "/<my_path>/.cargo/registry/src/github.com-1ecc6299db9ec823/snarkvm-parameters-0.0.2/src/params/outer_snark_pk-12b0f50.params".

snarkvm_parameters::params - Downloading parameters...
snarkvm_parameters::params - 100.00% complete (479 MB total)
snarkvm_parameters::params - Download complete
snarkvm_parameters::params - Storing parameters ("/<my_path>/.cargo/registry/src/github.com-1ecc6299db9ec823/snarkvm-parameters-0.0.2/src/params/outer_snark_pk-12b0f50.params")
2878386d9bc55cd70edc2f8a8ed00776bd0e25dabdf47ff351e59cc75d055508dc47296a7708e2bb340e89bf4e33d7aa58ff91ebb81cdf996bc44aa91c67548912f7dd3c4950b603c8a4f75e0e431c46aa8ad4547c3858098be9f10559f80001b0bbef8f226950f0ff2228ab46d33507a1f5e88260549f1bcfc099ab15624610c894eee83712e5f2edd32a44587589564402555280599c226c7e681c30a4eeb8b058934993bcd2666d45bce96ffb2b96c33a8b7548022d401f2b8fe28366bf0c84f313ea49db0e697c0c17e590a818a35d218bf710c984008d1c1d02834b124ef839763d51350b5bc4e950baa790a30086f3d16a56b679b538ec7f376c2153f6800a15da00b97345f5e426550b9ed4530c0ff010a375b10c297dc1cfcf980870a68f197e46fee23f2165e0e7178870b7e240e38cf77190f9b033128c2b92fc8500a3da185847ee02830fba4658c23a00f30572d9e4c3ccad9dcd9827a3fa916fc4995f19cf21a564ade7a60a33212dc113fce37b131df331ddf26a9c1dc51fccbbcb7097c0dcd67bc21893a09245d093efa2cbe739bf506ec577754ef6f3f5c0b8d22c20a823a467dd916d1db1fb0001005e790fd960087fc90f157164bbc6c5fb65e27660c9e2020f5af25f2e4b0f990ecc5d7553b989681dba0a1b0981f6a186a8cb87e4a4b7de7e26cba3e5acbc59c37739832b85385abf2bcca6a6cb4711e72d395fd45dac64242447550393de040055d1fda3b04916320fc4a302fcb1be7af0ccd6c04184ae42d5c34f245dfd74f06d1a2467ce69b0d7880a1e882c10482bb3e1d583cda0b8e32c8040f9e11d22c30118661412417f5ed2794f9b737aa22efb92eb815126968ea01d4cc8580c28000079d39218d00e129b822d3647c548bb63fc24cd009064ec497fb585f1d10f3274e3c7e0bc2423532d4cbb8dad4ecb8022cfb3a1705d115c2de10c0212c87c061499d5acd1ad63208d9a2d56f2c580ca214172b8d8a938be132126a0c02621630075e97d8c17a3d577c0f3d891978bf55f6f5202a632cbc46a6a21f7bf33f340ea29002a84e526bf1e9af07a0db3acd5b1580b61372042df326c582be684f6a60e06944a5dec5e48c10225ff31eeaa9be2dc81f4522d045218ff14326be3a0c20000cbb3c9cddbb049a71941b237665f9f36adafd0d0e9853670a50e1ab0f0821c606df9632af66df58abccbdf35c8a13ac1eb4b250e26780e1c73d0f67249c8950f000000000000000001097bfc730a603426e535d187d67a198869a355208a73b5230214f464a2c4be0048c0d41d1e0d4744ac74511cd316acb84572900fe57d5fed6d926ed8b08b4c01521ca5ed84865e274f05ade37e14ac388350149bd5a5a162038f29883adf940362810ea9753049d55c5f7cb4086ad65e44db5fd23048f10dd6d826504619920008af4b3f4a14aaebc83449780a3c06368479d54518e4c9394b107196a6d4799d052d2d44d4ec904094b57fb0e071226e0e08e5f5342f37b65cd75b9ba45912ab0551c7ddfac4313ea26fd63be3acb08dc3a5e37a0c61037d1b397e4039cbab81044890c6bfaa7e4ddba39a929cfcaa7a86e1f045502f96d85131be5254f530eb0182af40f3cb0ae138cb8592169fcbe15d769f6fb805a288f689262865bc42b20a7ce3121a346aaa0c8827c5513d92150fd87a60aca46ecf6eaee5970cda8ce8053ffe83f915f1fcd2fdc5f1071b3099176002112dbeb6aa9b37e45cd25618c40bdb8808b3be13a53bf80a374d5961efa487f7764c782f41154e387e5def32dd0f8200085621e0b9c3bb8d56afd297e8bf3377f4498e5727d2f723173154df1e600a040b530b063f511bcd856fa013090fbacf4e4dfdbe10017cfbfff9bdee91221fea02ef840cea3eac78a9ced0f633235ac80c772de318667f28738c908b998422ce035ebfc13d70403b719525c5b6b1dc23db4b463e4c6a03eb39c9d4fcda48c8b8078114f6395636232416f48c449ed8efe270ddbef30e95880b19c0d37a336b3a0de942246d92fe10f51140b1139d9ee073631a2673e856389acb8f8da443f819058eda09a75fb40ba50e94e14e7846176cdf7be03bc104f47d194f23958899910c12afdaa934939f3f6b3c8f4cb3a4453ab89b62326c163f9cf4d06ce572493f11f300
```

I don't know what to expect from this example, though.

**Install Leo**

```shell
$ cargo install leo-lang
    Updating crates.io index
  Downloaded leo-lang v1.0.8
  ...
   Compiling leo-lang v1.0.8
    Finished release [optimized] target(s) in 9m 14s
  Installing /<my_path>/.cargo/bin/leo                                        Installed package `leo-lang v1.0.8` (executable `leo`)

$ leo
leo v1.0.8
The Aleo Team <hello@aleo.org>
Leo compiler and package manager

USAGE:
    leo [FLAGS] [SUBCOMMAND]

FLAGS:
    -d, --debug    Enables debugging mode
    -h, --help     Prints help information

SUBCOMMANDS:
    new        Create a new Leo package in a new directory
    init       Create a new Leo package in an existing directory
    build      Compile the current package as a program
    watch      Watch the changes of the leo's source files
    test       Compile and run all tests in the current package
    setup      Run a program setup
    prove      Run the program and produce a proof
    run        Run a program with input variables
    login      Login to the Aleo Package Manager
    add        Install a package from the Aleo Package Manager
    remove     Uninstall a package from the current package
    publish    Publish the current package to the Aleo Package Manager
    deploy     Deploy the current package as a program to the network (*)
    clean      Clean the output directory
    lint       Lints the Leo files in the package (*)
    update     Update Leo to the latest version
```

Let create a project `firsttest`:

```shell
$ leo new firsttest
Initializing Successfully initialized package "firsttest"

$ cd firsttest/
$ ls
Leo.toml	README.md	inputs		src
$ ls src/
main.leo
$ ls inputs/
firsttest.in	firsttest.state
```

The file tree looks like this:

```shell
firsttest/
├── Leo.toml
├── README.md
├── inputs/
│ ├── firsttest.in
│ └── firsttest.state
└── src/
  └── main.leo
```

Run firsttest:

```shell
$ leo run
 Compiling Starting...
 Compiling Compiling main program... ("/<my_path>/firsttest/src/main.leo")
 Compiling Complete
      Done Finished in 6 milliseconds

     Setup Starting...
     Setup Saving proving key ("/<my_path>/firsttest/outputs/firsttest.lpk")
     Setup Complete
     Setup Saving verification key ("/<my_path>/firsttest/outputs/firsttest.lvk")
     Setup Complete
      Done Finished in 40 milliseconds

   Proving Starting...
   Proving Saving proof... ("/<my_path>/firsttest/outputs/firsttest.proof")
      Done Finished in 10 milliseconds

 Verifying Starting...
 Verifying Proof is valid
      Done Finished in 2 milliseconds
```


### Aleo references {#aleo-references}

-   Doc: <https://developer.aleo.org/aleo/getting%5Fstarted/overview/>
-   SDK: <https://github.com/AleoHQ/aleo>
-   Package manager: <https://aleo.pm/>
-   Node: <https://github.com/AleoHQ/snarkOS>
-   VM: <https://github.com/AleoHQ/snarkVM>