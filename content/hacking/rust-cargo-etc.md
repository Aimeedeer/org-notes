+++
title = "Rust Cargo and More"
author = ["Aimee Z"]
description = "Understand Rust programming language."
date = 2020-11-14
tags = ["hack", "log", "rust", "cargo", "rustup", "commandline"]
categories = ["hacking"]
draft = false
+++

## Cargo bin {#cargo-bin}

```nil
$ ~/.cargo/bin
-bash: /Users/aimeez/.cargo/bin: is a directory

$ cd ~/.cargo/bin
$ ls
basic-http-server	cargo-generate		diesel			rust-trending		ssmanager
cargo			cargo-make		lighthouse		rustc			ssserver
cargo-add		cargo-miri		makers			rustdoc			ssurl
cargo-casperlabs	cargo-rm		mdbook			rustfmt			wasm-pack
cargo-clippy		cargo-upgrade		mdbook-linkcheck	rustlings		wasm-pack.stamp
cargo-contract		cargo-watch		rls			rustup
cargo-fmt		cargo-web		rust-gdb		simple-http-server
cargo-fuzz		clippy-driver		rust-lldb		sslocal

$ which cargo
/Users/aimeez/.cargo/bin/cargo

$ ls -lh
total 496760
-rwxr-xr-x   1 aimeez  staff   5.1M Apr 26  2020 basic-http-server
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 cargo
-rwxr-xr-x   1 aimeez  staff   7.1M Sep 14 10:48 cargo-add
-rwxr-xr-x   1 aimeez  staff   1.0M Aug 25 13:56 cargo-casperlabs
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 cargo-clippy
-rwxr-xr-x   1 aimeez  staff   4.6M Nov  6 20:59 cargo-contract
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 cargo-fmt
-rwxr-xr-x   1 aimeez  staff   1.3M May 18 18:52 cargo-fuzz
-rwxr-xr-x   1 aimeez  staff   6.5M Oct 14 14:37 cargo-generate
-rwxr-xr-x   1 aimeez  staff   7.9M Oct 29 13:17 cargo-make
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 cargo-miri
-rwxr-xr-x   1 aimeez  staff   2.0M Sep 14 10:48 cargo-rm
-rwxr-xr-x   1 aimeez  staff   3.5M Sep 14 10:48 cargo-upgrade
-rwxr-xr-x   1 aimeez  staff   2.1M Aug 11 15:20 cargo-watch
-rwxr-xr-x   1 aimeez  staff   9.5M Oct 29 14:52 cargo-web
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 clippy-driver
-rwxr-xr-x   1 aimeez  staff   3.1M Jun 17 12:56 diesel
-rwxr-xr-x   1 aimeez  staff    42M Aug 11 12:46 lighthouse
-rwxr-xr-x   1 aimeez  staff   7.9M Oct 29 13:17 makers
-rwxr-xr-x   1 aimeez  staff    10M Sep 20 11:06 mdbook
-rwxr-xr-x   1 aimeez  staff   9.3M Sep 20 11:09 mdbook-linkcheck
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 rls
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 rust-gdb
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 rust-lldb
-rwxr-xr-x   1 aimeez  staff   6.5M Jun 13 15:51 rust-trending
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 rustc
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 rustdoc
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 rustfmt
-rwxr-xr-x   1 aimeez  staff   2.7M Jul 27 16:43 rustlings
-rwxr-xr-x  12 aimeez  staff   7.1M Jul 27 16:47 rustup
-rwxr-xr-x   1 aimeez  staff   2.6M Aug 11 15:12 simple-http-server
-rwxr-xr-x   1 aimeez  staff   6.1M Jun  8 11:05 sslocal
-rwxr-xr-x   1 aimeez  staff   4.1M Jun  8 11:04 ssmanager
-rwxr-xr-x   1 aimeez  staff   3.9M Jun  8 11:04 ssserver
-rwxr-xr-x   1 aimeez  staff   1.3M Jun  8 11:03 ssurl
-rwxr-xr-x   1 aimeez  staff   7.2M May 23 01:28 wasm-pack
-rw-r--r--   1 aimeez  staff    54B Oct 29 14:21 wasm-pack.stamp
```


## Rustup toolchains {#rustup-toolchains}

[The rustup book](https://rust-lang.github.io/rustup/installation/index.html)
>rustup installs rustc, cargo, rustup and other standard tools
to Cargo's bin directory. On Unix it is located at $HOME/.cargo/bin
and on Windows at %USERPROFILE%\\.cargo\bin.
This is the same directory that cargo install will
install Rust programs and Cargo plugins.

Toolchains on my mac:

```nil
$ ls ~/.rustup/
downloads	settings.toml	toolchains	update-hashes

$ ls ~/.rustup/toolchains/
1.34.2-x86_64-apple-darwin		nightly-2019-10-14-x86_64-apple-darwin	nightly-x86_64-apple-darwin
1.41.0-x86_64-apple-darwin		nightly-2020-03-19-x86_64-apple-darwin	stable-x86_64-apple-darwin
1.43.1-x86_64-apple-darwin		nightly-2020-05-15-x86_64-apple-darwin
1.45.2-x86_64-apple-darwin		nightly-2020-08-23-x86_64-apple-darwin

$ ls ~/.rustup/toolchains/stable-x86_64-apple-darwin/
bin	etc	lib	share

$ ls ~/.rustup/toolchains/stable-x86_64-apple-darwin/bin/
cargo		cargo-fmt	rls		rust-gdbgui	rustc		rustfmt
cargo-clippy	clippy-driver	rust-gdb	rust-lldb	rustdoc
```


## Rustc: #[lang = "start"] {#rustc-lang-start}

[fn lang\_start<T: crate::process::Termination + 'static>(main: fn() -> T, argc: isize, argv: \*const \*const u8,)](https://github.com/rust-lang/rust/blob/master/library/std/src/rt.rs#L60)

Rustc uses std to create the main() function,
with mymain() as one argument,
as lang-start for the operating system
to start executing.

From [Rust playground](https://play.rust-lang.org/), we can generate LLVM code from
our empty main function:

```nil
fn main() {
}
```

The LLVM code:

```nil
; std::rt::lang_start
; Function Attrs: nonlazybind uwtable
define hidden i64 @_ZN3std2rt10lang_start17hd0d6144126b78ac1E(void ()* nonnull %main, i64 %argc, i8** %argv) unnamed_addr #1 !dbg !42 {
start:
```