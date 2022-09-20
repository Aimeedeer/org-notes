+++
title = "Learn Rust"
author = ["Aimee Z"]
description = "Learn Rust programming."
date = 2020-11-14
tags = ["rust", "log"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2012
  identifier = "learn-rust"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Rust language](#rust-language)
- [main.rs and lib.rs](#main-dot-rs-and-lib-dot-rs)
- [Copy and Clone in Rust](#copy-and-clone-in-rust)
- [Rust discussion](#rust-discussion)
- [Cargo bin](#cargo-bin)
- [Rustup toolchains](#rustup-toolchains)
- [Rust lang entry](#rust-lang-entry)
- [Book: Programming Rust](#book-programming-rust)
- [Good blog posts](#good-blog-posts)

</div>
<!--endtoc-->


## Rust language {#rust-language}

[The Little Book of Rust Books](https://lborb.github.io/book/)

[Learn Rust With Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/index.html)

[Understanding and Evolving the Rust Programming Language](https://people.mpi-sws.org/~jung/phd/thesis-screen.pdf) August 2020

[Rust Starter Kit 2020](https://wiki.alopex.li/RustStarterKit2020)

Video: [Pascal Hertleif - Writing Idiomatic Libraries in Rust](https://www.youtube.com/watch?v=0zOg8%5FB71gE)

Collections:

-   Rust book: [Common Collections](https://doc.rust-lang.org/stable/book/ch08-00-common-collections.html)
-   [Module std::collections](https://doc.rust-lang.org/stable/std/collections/index.html)

> To get this out of the way: you should probably just use `Vec` or `HashMap`.
These two collections cover most use cases for generic data storage and processing.
They are exceptionally good at doing what they do.
All the other collections in the standard library have
specific use cases where they are the optimal choice,
but these cases are borderline niche in comparison.
Even when Vec and HashMap are technically suboptimal,
they're probably a good enough choice to get started.
>
> Rust's collections can be grouped into four major categories:
>
> - Sequences: Vec, VecDeque, LinkedList
> - Maps: HashMap, BTreeMap
> - Sets: HashSet, BTreeSet
> - Misc: BinaryHeap


## main.rs and lib.rs {#main-dot-rs-and-lib-dot-rs}

[Separation of Concerns for Binary Projects](https://doc.rust-lang.org/stable/book/ch12-03-improving-error-handling-and-modularity.html#separation-of-concerns-for-binary-projects)
> The responsibilities that remain in the `main` function after this process should be limited to the following:
>
> - Calling the command line parsing logic with the argument values
> - Setting up any other configuration
> - Calling a `run` function in lib.rs
> - Handling the error if `run` returns an error

> This pattern is about separating concerns: _main.rs_ handles running the program, and _lib.rs_ handles all the logic of the task at hand.

Code example: <https://github.com/Aimeedeer/minigrep-rust-practice>


## Copy and Clone in Rust {#copy-and-clone-in-rust}

[What Is Ownership?](https://doc.rust-lang.org/stable/book/ch04-01-what-is-ownership.html)

This chapter is really good. The stack and heap are well explained.

[Clone and Copy for Duplicating Values](https://doc.rust-lang.org/stable/book/appendix-03-derivable-traits.html?highlight=clone#clone-and-copy-for-duplicating-values)

[Module core::clone](https://doc.rust-lang.org/core/clone/index.html)

> Differs from `Copy` in that `Copy` is implicit and extremely inexpensive,
while `Clone` is always explicit and may or may not be expensive.
In order to enforce these characteristics, Rust does not allow you to reimplement `Copy`,
but you may reimplement `Clone` and run arbitrary code.

Blog post:
[Moves, copies and clones in Rust](https://hashrust.com/blog/moves-copies-and-clones-in-rust/)

Not much new in this post if you've already read Rust book.


## Rust discussion {#rust-discussion}

[ReadRust: Computer Science](https://readrust.net/computer-science)

[Reddit discussion: Opinions about using Rust instead of C in Computer Science courses](https://www.reddit.com/r/rust/comments/6nw22d/opinions%5Fabout%5Fusing%5Frust%5Finstead%5Fof%5Fc%5Fin/)

Rust weaknesses:
[This question got a bunch of discussions](https://www.reddit.com/r/rust/comments/jia2xn/what%5Fare%5Fsome%5Fof%5Frusts%5Fweaknesses%5Fas%5Fa%5Flanguage/)


## Cargo bin {#cargo-bin}

```shell
$ ~/.cargo/bin
-bash: /<my_path>/.cargo/bin: is a directory

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
/<my_path>/.cargo/bin/cargo

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

```shell
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


## Rust lang entry {#rust-lang-entry}

[EntryPointType](https://github.com/rust-lang/rust/blob/efbaa413061c2a6e52f06f00a60ee7830fcf3ea5/compiler/rustc%5Fpasses/src/entry.rs#L50-L76)

[rust/compiler/rustc\_codegen\_cranelift/src/main\_shim.rs](https://github.com/rust-lang/rust/blob/56293097f7f877f1350a6cd00f79d03132f16515/compiler/rustc%5Fcodegen%5Fcranelift/src/main%5Fshim.rs)

Rustc: #[lang = "start"]

[fn lang\_start<T: crate::process::Termination + 'static>(main: fn() -> T, argc: isize, argv: \*const \*const u8,)](https://github.com/rust-lang/rust/blob/master/library/std/src/rt.rs#L60)

Rustc uses std to create the main() function,
with mymain() as one argument,
as lang-start for the operating system
to start executing.

From [Rust playground](https://play.rust-lang.org/), we can generate LLVM code from
our empty main function:

```rust
fn main() {
}
```

The LLVM code:

```llvm
; std::rt::lang_start
; Function Attrs: nonlazybind uwtable
define hidden i64 @_ZN3std2rt10lang_start17hd0d6144126b78ac1E(void ()* nonnull %main, i64 %argc, i8** %argv) unnamed_addr #1 !dbg !42 {
start:
```


## Book: Programming Rust {#book-programming-rust}


## Good blog posts {#good-blog-posts}

-   [Rust in Perspective](https://people.kernel.org/linusw/rust-in-perspective)
-   [Safe Systems Programming in Rust](https://iris-project.org/pdfs/2021-rustbelt-cacm-final.pdf)
-   [Graydon Hoare's response on reddit about Rust, Swift and languages](https://www.reddit.com/r/rust/comments/7qels2/comment/dsqeh1d/?utm%5Fsource=share&utm%5Fmedium=web2x&context=3)
-   [What next?](https://graydon2.dreamwidth.org/253769.html)
    Graydon's post on people's question: After memory safety, what do you think is the next big step for compiled languages to take.
-   [Swift](https://graydon2.dreamwidth.org/5785.html)
-   [programming languages and empiricism](https://graydon2.dreamwidth.org/259333.html)
-   [Programming Models for Distributed Computation](https://github.com/heathermiller/dist-prog-book)
-   [What makes a good REPL? (2017) (vvvvalvalval.github.io)](https://news.ycombinator.com/item?id=32747088)