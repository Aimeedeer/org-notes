+++
title = "RustSec"
author = ["Aimee Z"]
description = "Rust and security related"
date = 2021-10-08
draft = false
[menu.main]
  weight = 2001
  identifier = "rustsec"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Solana dependencies](#solana-dependencies)

</div>
<!--endtoc-->


## Solana dependencies {#solana-dependencies}

When we worked on Solana project, we met a problem that we couldn't update
crate dependencies with `cargo update`.

```shell
$ cargo update -p nix
    Updating crates.io index
```

But nothing changed. We kept trying:

```shell
$ cargo update -p nix --precise 0.20.2
    Updating crates.io index
error: failed to select a version for `memoffset`.
    ... required by package `nix v0.20.2`
    ... which is depended on by `ctrlc v3.1.9`
    ... which is depended on by `solana-cli v1.9.0 (/<my_path>/solana/cli)`
versions that meet the requirements `^0.6.3` are: 0.6.4, 0.6.3

all possible versions conflict with previously selected packages.

  previously selected package `memoffset v0.6.1`
    ... which is depended on by `crossbeam-epoch v0.9.5`
    ... which is depended on by `crossbeam-deque v0.8.1`
    ... which is depended on by `rayon v1.5.1`
    ... which is depended on by `dashmap v4.0.2`
    ... which is depended on by `solana-core v1.9.0 (/<my_path>/solana/core)`
    ... which is depended on by `solana-accounts-cluster-bench v1.9.0 (/<my_path>/solana/accounts-cluster-bench)`

failed to select a version for `memoffset` which could resolve this conflict
```

```shell
$ cargo update -p ctrlc -p nix
    Updating crates.io

$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

One more:

```shell
$ cargo update -p ctrlc -p nix -p crossbeam-epoch -p crossbeam-deque -p rayon -p dashmap
    Updating crates.io index
    Updating ctrlc v3.1.9 -> v3.2.1
    Updating memoffset v0.6.1 -> v0.6.4
      Adding nix v0.23.0



$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
      modified:   Cargo.lock

no changes added to commit (use "git add" and/or "git commit -a")
```

Use `tree`:

```shell
$ cargo tree -p nix
error: There are multiple `nix` packages in your project, and the specification `nix` is ambiguous.
Please re-run this command with `-p <spec>` where `<spec>` is one of the following:
  nix:0.20.0
  nix:0.23.0
```

```shell
$ cargo tree -p nix:0.20.0
nix v0.20.0
├── bitflags v1.3.2
├── cfg-if v1.0.0
└── libc v0.2.103


$ cargo tree -p nix:0.20.0 -i
nix v0.20.0
├── solana-install v1.9.0 (/<my_path>/solana/install)
├── solana-net-utils v1.9.0 (/<my_path>/solana/net-utils)
│   ├── solana-accounts-cluster-bench v1.9.0 (/<my_path>/solana/accounts-cluster-bench)
│   ├── solana-bench-streamer v1.9.0 (/<my_path>/solana/bench-streamer)
│   ├── solana-bench-tps v1.9.0 (/<my_path>/solana/bench-tps)
│   ├── solana-client v1.9.0 (/<my_path>/solana/client)
│   │   ├── solana-accounts-cluster-bench v1.9.0 (/<my_path>/solana/accounts-cluster-bench)
...
```

```shell
$ rg nix -t toml
net-utils/Cargo.toml
16:nix = "0.20.0"

sys-tuner/Cargo.toml
20:[target."cfg(unix)".dependencies]
21:unix_socket2 = "0.5.4"
23:nix = "0.20.0"

ledger-tool/Cargo.toml
43:[target."cfg(unix)".dependencies]

programs/bpf/Cargo.lock
1559:name = "nix"
3079: "nix",

streamer/Cargo.toml
20:nix = "0.20.0"

Cargo.lock
997: "nix 0.23.0",
2605:name = "nix"
2617:name = "nix"
4915: "nix 0.20.0",
5159: "nix 0.20.0",
5705: "nix 0.20.0",
5720: "nix 0.20.0",
5724: "unix_socket2",
6853:name = "unix_socket2"

validator/Cargo.toml
55:[target."cfg(unix)".dependencies]

install/Cargo.toml
23:nix = "0.20.0"
```

```shell
$ cargo update -p nix:0.20.0 --precise 0.20.2
    Updating crates.io index
error: failed to select a version for `bitflags`.
    ... required by package `nix v0.20.2`
    ... which is depended on by `solana-install v1.9.0 (/<my_path>/solana/install)`
versions that meet the requirements `>=1.1.0, <1.3.0` are: 1.2.1, 1.2.0, 1.1.0

all possible versions conflict with previously selected packages.

  previously selected package `bitflags v1.3.1`
    ... which is depended on by `nix v0.23.0`
    ... which is depended on by `ctrlc v3.2.1`
    ... which is depended on by `solana-cli v1.9.0 (/<my_path>/solana/cli)`

failed to select a version for `bitflags` which could resolve this conflict
```

Haven't solved it yet. To be continued.