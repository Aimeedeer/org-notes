+++
title = "Captain Game"
author = ["Aimee Z"]
description = "A game smart contract with Parity ink."
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
    - [2021-02-14](#2021-02-14)
    - [2021-02-13](#2021-02-13)
    - [2021-02-06](#2021-02-06)
    - [2021-02-04](#2021-02-04)
    - [2021-02-03](#2021-02-03)
    - [2021-02-01](#2021-02-01)
    - [2021-01-18](#2021-01-18)
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

Captain game with Parity ink.


## My hacklog {#my-hacklog}


### 2021-02-14 {#2021-02-14}

Some differences between ink's doc and my terminal:

[ink's doc](https://paritytech.github.io/ink-docs/cargo-contract-cli):

```shell
cargo-contract 0.8.0
Utilities to develop Wasm smart contracts.

USAGE:
    cargo contract <SUBCOMMAND>

OPTIONS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    new                  Setup and create a new smart contract project
    build                Compiles the contract, generates metadata, bundles both together in a '.contract' file
    check                Check that the code builds as Wasm; does not output any build artifact to the top level `target/` directory
    test                 Test the smart contract off-chain
    deploy               Upload the smart contract code to the chain
    instantiate          Instantiate a deployed smart contract
    help                 Prints this message or the help of the given subcommand(s)
```

My terminal:

```shell
$ cargo contract --version
cargo-contract 0.8.0

$ cargo contract --help
cargo-contract 0.8.0
Utilities to develop Wasm smart contracts

USAGE:
    cargo contract <SUBCOMMAND>

OPTIONS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    new                  Setup and create a new smart contract project
    build                Compiles the contract, generates metadata, bundles both together in a `<name>.contract`
                         file
    generate-metadata    Command has been deprecated, use `cargo contract build` instead
    check                Check that the code builds as Wasm; does not output any build artifact to the top level
                         `target/` directory
    test                 Test the smart contract off-chain
    help                 Prints this message or the help of the given subcommand(s)

$ cargo contract deploy
error: Found argument 'deploy' which wasn't expected, or isn't valid in this context

USAGE:
    cargo contract <SUBCOMMAND>

For more information try --help
```

While there isn't a `upgrade` or `update` command, I installed it again:

```shell
$ cargo install cargo-contract --vers 0.8.0 --force --locked
```

But, `cargo contract --help` doesn't show me any differences.

Gladly, I find something from GitHub:
[cargo-contract](https://github.com/paritytech/cargo-contract)

**[Features](https://github.com/paritytech/cargo-contract#features)**

> The \`deploy\` and \`instantiate\` subcommands are ****disabled by default****, since they are not fully stable yet and increase the build time.
>
> If you want to try them, you need to enable the \`extrinsics\` feature:
>
> \`cargo install --git <https://github.com/paritytech/cargo-contract> cargo-contract --features extrinsics --force\`
>
> Once they are stable and the compilation time is acceptable, we will consider removing the \`extrinsics\` feature.

I am trying but it doesn't work: it's broken, and Brian has submitted a PR to fix it.

```shell
$ cargo install --git https://github.com/paritytech/cargo-contract cargo-contract --features extrinsics --force

...
error: aborting due to 60 previous errors

Some errors have detailed explanations: E0034, E0308.
For more information about an error, try `rustc --explain E0034`.
error: could not compile `bitvec`

To learn more, run the command again with --verbose.
warning: build failed, waiting for other jobs to finish...
error: failed to compile `cargo-contract v0.8.0 (https://github.com/paritytech/cargo-contract#79dbcb65)`, intermediate artifacts can be found at `/var/folders/g5/hf7q78jn0vngnqtqj_3qfm6r0000gn/T/cargo-installBtwvjf`

Caused by:
  build failed
```

Changes in the smart contract:
I add more verification in the run-highest-level logic that
prints congratulations and
**do not** level\_up once the player's run\_level\_2 is succeeded.


### 2021-02-13 {#2021-02-13}

Test with submit\_level and run\_level + level\_up after run\_level succeed:

```shell
2021-02-13 12:26:36.009  DEBUG tokio-runtime-worker runtime:game account: PlayerAccount { level: 1, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230]), 1: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-13 12:26:36.010  DEBUG tokio-runtime-worker runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-13 12:26:36.011  DEBUG tokio-runtime-worker runtime:dispatch level: 1, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-13 12:26:36.012  DEBUG tokio-runtime-worker runtime:run_level_1_flipper, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-13 12:26:36.012  DEBUG tokio-runtime-worker runtime:get method call success
2021-02-13 12:26:36.012  DEBUG tokio-runtime-worker runtime:get return value true
2021-02-13 12:26:36.012  DEBUG tokio-runtime-worker runtime:verified flipper current state
2021-02-13 12:26:36.013  DEBUG tokio-runtime-worker runtime:get method call success
2021-02-13 12:26:36.013  DEBUG tokio-runtime-worker runtime:get return value false
2021-02-13 12:26:36.013  DEBUG tokio-runtime-worker runtime:verify flipper new state
2021-02-13 12:26:36.014  DEBUG tokio-runtime-worker runtime:run_level_1_flipper call success
2021-02-13 12:26:36.015  DEBUG tokio-runtime-worker runtime:player_account: PlayerAccount { level: 2, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230]), 1: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
```

Now a player can submit (and run) programs up to 3 levels.

I also add more `Debugging` log, and clean up the code.


### 2021-02-06 {#2021-02-06}

First log:

```shell
$ canvas --dev --tmp -lerror,runtime=debug


# Bob creates a player account

2021-02-06 13:21:47.719  DEBUG event.loop0 runtime:new player account PlayerAccount { level: 0, level_contracts: {} }
2021-02-06 13:21:54.009  DEBUG tokio-runtime-worker runtime:new player account PlayerAccount { level: 0, level_contracts: {} }


# Bob submits a level_program

2021-02-06 13:22:12.498  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
2021-02-06 13:22:12.521  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
2021-02-06 13:22:12.529  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
2021-02-06 13:22:12.539  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([142, 175, 4, 21, 22, 135, 115, 99, 38, 201, 254, 161, 126, 37, 252, 82, 135, 97, 54, 147, 201, 18, 144, 156, 178, 38, 170, 71, 148, 242, 106, 72])
2021-02-06 13:22:17.161  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-06 13:22:24.007  DEBUG tokio-runtime-worker runtime:insert level 0, and contract AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])


# choose "run_level" method from the dropdown list

2021-02-06 13:22:42.891  DEBUG          event.loop0 runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-06 13:22:42.892  DEBUG          event.loop0 runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-06 13:22:42.893  DEBUG          event.loop0 runtime:dispatch level: 0, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-06 13:22:42.893  DEBUG          event.loop0 runtime:contract selector: [222, 173, 190, 239]
2021-02-06 13:22:42.895  DEBUG          event.loop0 runtime:get method call failed: Decode(Error)
2021-02-06 13:22:42.902  DEBUG          event.loop0 runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-06 13:22:42.903  DEBUG          event.loop0 runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-06 13:22:42.905  DEBUG          event.loop0 runtime:dispatch level: 0, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-06 13:22:42.905  DEBUG          event.loop0 runtime:contract selector: [222, 173, 190, 239]
2021-02-06 13:22:42.906  DEBUG          event.loop0 runtime:get method call failed: Decode(Error)


# click "Call" button

2021-02-06 13:23:06.012  DEBUG tokio-runtime-worker runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-06 13:23:06.013  DEBUG tokio-runtime-worker runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-06 13:23:06.014  DEBUG tokio-runtime-worker runtime:dispatch level: 0, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-06 13:23:06.015  DEBUG tokio-runtime-worker runtime:contract selector: [222, 173, 190, 239]
2021-02-06 13:23:06.015  DEBUG tokio-runtime-worker runtime:get method call failed: Decode(Error)
```

I have wasted so much time only because I didn't pay attention to
the two selectors and the different method return values.
Cross contract call works finally after I corrected the selector.

I add more prints for verification in the `run_level` method,
and create run\_level\_0, run\_level\_1, and more levels.
Each one uses the same flipper contract for testing.


### 2021-02-04 {#2021-02-04}

Is this a network problem?

```shell
contracts.call
1014: Priority is too low: (133148198758 vs 133148198758): The transaction has too low priority to replace another transaction already in the pool.
```

Again, this time, our demo doesn't work:

```shell
2021-02-04 11:44:06.057  DEBUG          event.loop0 runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-04 11:44:06.058  DEBUG          event.loop0 runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 11:44:06.059  DEBUG          event.loop0 runtime:dispatch level: 0, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 11:44:06.060  DEBUG          event.loop0 runtime:contract selector: Selector { bytes: [222, 173, 190, 239] }
2021-02-04 11:44:06.061  DEBUG          event.loop0 runtime:get method call failed: Decode(Error)
2021-02-04 11:44:06.069  DEBUG          event.loop0 runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-04 11:44:06.070  DEBUG          event.loop0 runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 11:44:06.072  DEBUG          event.loop0 runtime:dispatch level: 0, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 11:44:06.072  DEBUG          event.loop0 runtime:contract selector: Selector { bytes: [222, 173, 190, 239] }
2021-02-04 11:44:06.073  DEBUG          event.loop0 runtime:get method call failed: Decode(Error)
2021-02-04 11:44:18.011  DEBUG tokio-runtime-worker runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-04 11:44:18.013  DEBUG tokio-runtime-worker runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 11:44:18.014  DEBUG tokio-runtime-worker runtime:dispatch level: 0, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 11:44:18.015  DEBUG tokio-runtime-worker runtime:contract selector: Selector { bytes: [222, 173, 190, 239] }
2021-02-04 11:44:18.015  DEBUG tokio-runtime-worker runtime:get method call failed: Decode(Error)
```

Why it prints three rounds of debug messages?

I go back to our previous sample test, and it doesn't work either:

```shell
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:calling get on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:DispatchError
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:8
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:6
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:OutOfGas
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:PostInfo:
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:actual_weight=
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:6113641282
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:pays_fee=
2021-02-04 11:47:18.005  DEBUG tokio-runtime-worker runtime:Yes
```

I change the sample code and remove
`.gas_limit(0)` and `.transferred_value(0)`,
then it works!
I don't understand.

```shell
2021-02-04 11:59:12.124  DEBUG          event.loop0 runtime:calling get on AccountId([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
2021-02-04 11:59:12.124  DEBUG          event.loop0 runtime:return value Err(NotCallable)
2021-02-04 11:59:12.130  DEBUG          event.loop0 runtime:calling get on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-04 11:59:12.132  DEBUG          event.loop0 runtime:return value Ok(false)
2021-02-04 11:59:14.451  DEBUG          event.loop0 runtime:calling get on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-04 11:59:14.451  DEBUG          event.loop0 runtime:return value Ok(false)
2021-02-04 11:59:30.002  DEBUG tokio-runtime-worker runtime:calling get on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-04 11:59:30.003  DEBUG tokio-runtime-worker runtime:return value Ok(false)
2021-02-04 11:59:36.004  DEBUG tokio-runtime-worker runtime:calling get on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-04 11:59:36.005  DEBUG tokio-runtime-worker runtime:return value Ok(false)
```

Now I am going to test the game contract **again**.
`run_level` call prints debug messages:

```shell
# call submit_level method

2021-02-04 12:03:53.456  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
2021-02-04 12:03:53.483  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
2021-02-04 12:03:53.490  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
2021-02-04 12:03:53.495  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([142, 175, 4, 21, 22, 135, 115, 99, 38, 201, 254, 161, 126, 37, 252, 82, 135, 97, 54, 147, 201, 18, 144, 156, 178, 38, 170, 71, 148, 242, 106, 72])
2021-02-04 12:04:21.854  DEBUG          event.loop0 runtime:insert level 0, and contract AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 12:04:36.010  DEBUG tokio-runtime-worker runtime:insert level 0, and contract AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])


# choose run_level method from Canvas UI

2021-02-04 12:06:35.054  DEBUG          event.loop0 runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-04 12:06:35.055  DEBUG          event.loop0 runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 12:06:35.056  DEBUG          event.loop0 runtime:dispatch level: 0, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 12:06:35.056  DEBUG          event.loop0 runtime:contract selector: Selector { bytes: [222, 173, 190, 239] }
2021-02-04 12:06:35.057  DEBUG          event.loop0 runtime:get method call failed: Decode(Error)
2021-02-04 12:06:35.065  DEBUG          event.loop0 runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-04 12:06:35.066  DEBUG          event.loop0 runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 12:06:35.067  DEBUG          event.loop0 runtime:dispatch level: 0, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 12:06:35.067  DEBUG          event.loop0 runtime:contract selector: Selector { bytes: [222, 173, 190, 239] }
2021-02-04 12:06:35.068  DEBUG          event.loop0 runtime:get method call failed: Decode(Error)


# click call button and sign it

2021-02-04 12:07:18.008  DEBUG tokio-runtime-worker runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])} }
2021-02-04 12:07:18.009  DEBUG tokio-runtime-worker runtime:program id: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 12:07:18.011  DEBUG tokio-runtime-worker runtime:dispatch level: 0, calling contract: AccountId([154, 109, 9, 43, 214, 19, 68, 75, 177, 212, 196, 5, 184, 224, 248, 107, 32, 56, 240, 228, 240, 158, 222, 41, 53, 1, 138, 195, 219, 58, 141, 230])
2021-02-04 12:07:18.011  DEBUG tokio-runtime-worker runtime:contract selector: Selector { bytes: [222, 173, 190, 239] }
2021-02-04 12:07:18.012  DEBUG tokio-runtime-worker runtime:get method call failed: Decode(Error)
```

Seems there is something wrong with my changed code.


### 2021-02-03 {#2021-02-03}

Ink team seems to plan to implement trait for  cross-contract calls:
[[Feature] Dynamic trait based contract calling #631](https://github.com/paritytech/ink/issues/631).
We currently use hard-coded code in method attributes, called `Selector`.

While our game contract needs to allow players upload their programs for
each multiple level challenges, I plan to use multiple ~Selector~s for
different programs.
I changed the code a bit:

```rust
let selector;
match level {
    0 => selector = Selector::new([0xDE, 0xAD, 0xBE, 0xEF]),
    1 => selector = Selector::new([0xDE, 0xAD, 0xEE, 0xEE]),
    _ => unreachable!(),
}
```

Call method failed, and I don't know what's wrong from the error message.
I'll run the previous game-test example again later.


### 2021-02-01 {#2021-02-01}

After about two weeks without working on it,
I need to restore my previous log in my memory to start with.

I build my Game contract and Example-levels contract respectively: `cargo contract build`,
and run a Substrate node: `canvas --dev --tmp -lerror,runtime=debug`,
then visit Canvas from browser: <https://paritytech.github.io/canvas-ui/#/upload>

Test process from Canvas:

-   Alice deploys Game contract
-   Bob deploys Flipper(Example-levels) contract
-   Bob call Alice's Game contract
    -   RPC call: have\_player\_account
        -   return false
    -   Tx call: creat\_player\_account
    -   RPC call: have\_player\_account
        -   return true
    -   RPC call: get\_player\_account (Error below)

<!--listend-->

```shell
Uncaught error. Something went wrong with the query and rendering of this component. createType(Result):: Cannot construct unknown type Result
```

-   Tx call: submit\_level
-   Tx call: run\_level

<!--listend-->

```shell
contracts.call
inblock

system.ExtrinsicSuccess
extrinsic event
```

We print a lot of activities in our code:

```rust
ink_env::debug_println(&format!("print_some_thing {:?}", print_content));~
```

The node terminal prints below.
I don't know why it prints three times.

```shell
2021-02-01 11:26:05.699  DEBUG          event.loop0 runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])} }
2021-02-01 11:26:05.700  DEBUG          event.loop0 runtime:program id: AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-01 11:26:05.701  DEBUG          event.loop0 runtime:dispatch level: calling flip on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-01 11:26:05.703  DEBUG          event.loop0 runtime:calling get on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-01 11:26:05.704  DEBUG          event.loop0 runtime:get method call success
2021-02-01 11:26:05.704  DEBUG          event.loop0 runtime:get return value true
2021-02-01 11:26:05.712  DEBUG          event.loop0 runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])} }
2021-02-01 11:26:05.713  DEBUG          event.loop0 runtime:program id: AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-01 11:26:05.714  DEBUG          event.loop0 runtime:dispatch level: calling flip on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-01 11:26:05.716  DEBUG          event.loop0 runtime:calling get on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-01 11:26:05.717  DEBUG          event.loop0 runtime:get method call success
2021-02-01 11:26:05.717  DEBUG          event.loop0 runtime:get return value true
2021-02-01 11:26:18.012  DEBUG tokio-runtime-worker runtime:game account: PlayerAccount { level: 0, level_contracts: {0: AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])} }
2021-02-01 11:26:18.013  DEBUG tokio-runtime-worker runtime:program id: AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-01 11:26:18.014  DEBUG tokio-runtime-worker runtime:dispatch level: calling flip on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-01 11:26:18.016  DEBUG tokio-runtime-worker runtime:calling get on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-02-01 11:26:18.016  DEBUG tokio-runtime-worker runtime:get method call success
2021-02-01 11:26:18.016  DEBUG tokio-runtime-worker runtime:get return value true
```


### 2021-01-18 {#2021-01-18}

It works today, for no reason.
[Testing source code](https://github.com/Aimeedeer/game-test).

```shell
2021-01-18 17:21:12.010  DEBUG tokio-runtime-worker runtime:calling get on AccountId([143, 23, 140, 104, 129, 87, 181, 199, 82, 222, 77, 198, 172, 231, 178, 249, 251, 156, 129, 233, 134, 167, 114, 60, 101, 73, 245, 85, 139, 84, 27, 156])
2021-01-18 17:21:12.010  DEBUG tokio-runtime-worker runtime:return value Ok(false)
```


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
  --> /<my_path>/contract-game/src/game/lib.rs:49:51
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