+++
title = "Walnuts Hacklog"
author = ["Aimee Z"]
description = "My hacklog about Walnuts, a toy blockchain."
date = 2020-11-13
tags = ["hack", "blockchain", "walnuts", "log"]
categories = ["hacking"]
draft = false
+++

## <span class="org-todo todo TODO">TODO</span> List {#list}

-   sha2 for hash function
-   chrono for Utc time as timestamp
-   serde for storage, write & read to a file on disc
-   cli
-   [ ] create a new block from commandline,
    and save it in a file in json
-   [ ] create transactions from commandline
-   [ ] 256k for tx signature: <https://docs.rs/k256/0.5.10/k256/>


## Hacklog {#hacklog}


### 2020-11-13 {#2020-11-13}

-   Learned:
    -   cargo clean: clean the target folder
    -   cargo run -- mysubcommand == target/walnuts mysubcommand
    -   <https://rust-cli.github.io/book/tutorial/cli-args.html>
-   Use structopt as cli in main.rs
    -   When I moved the cli related code to cli.rs, there is an error

<!--listend-->

```nil
error[E0599]: no function or associated item named `from_args` found for struct `Opt` in the current scope
  --> src/main.rs:16:25
   |
16 |     let opt = cli::Opt::from_args();
   |                         ^^^^^^^^^ function or associated item not found in `Opt`
   |
  ::: src/cli.rs:5:1
   |
5  | pub struct Opt {
   | -------------- function or associated item `from_args` not found for this
   |
   = help: items from traits can only be used if the trait is in scope
help: the following trait is implemented but not in scope; perhaps add a `use` for it:
   |
1  | use structopt::StructOpt;
   |

error: aborting due to previous error

For more information about this error, try `rustc --explain E0599`.
```

Then I added this piece to previous main.rs, and it built.

```nil
use structopt::StructOpt;
```


### 2020-11-12 {#2020-11-12}

-   Cargo check / cargo build
-   std::io to deal with files
-   cli
    -   clap
    -   structopt: <https://docs.rs/structopt/0.3.20/structopt/>
        -   <https://rust-cli.github.io/book/tutorial/cli-args.html>


### 2020-11-11 {#2020-11-11}

-   Use Tony's secp256k1 library
-   Serde

About serde derive:

```nil
error: cannot find derive macro `Serialize` in this scope
 --> src/block.rs:6:10
  |
6 | #[derive(Serialize, Deserialize, Debug)]
  |          ^^^^^^^^^

error: cannot find derive macro `Deserialize` in this scope
```

Then add feature in Cargo.toml

```nil
serde = { version = "1.0.117", features = ["derive"] }
```

The doc explains:
[Using derive](https://serde.rs/derive.html)


### 2020-11-10 {#2020-11-10}

-   [Rust: chrono](https://docs.rs/chrono/0.4.19/chrono/struct.DateTime.html) for Utc time
-   Read: [How Bitcoin Timestamping Works](https://betweentwocommits.com/blog/how-bitcoin-timestamping-works)

>Bitcoin timestamping does not guarantee an exact time.
Bitcoin miners are calibrated to create blocks approximately
every ten minutes.
However, because of the way their protocol works,
this is only an average. It could be two minutes, or fifteen.
This means that the time given by a timestamp is
only precise to within a range of a few hours.
For most use cases, this is not an issue,
since getting the date right is more important than
the minute or second.

>I should mention that the act of inserting non-transactional data
in the blockchain is a disputed practice.
As I mentioned earlier,
the blockchain is now approximately 300 GB,
and it can only get bigger. Some people believe that
adding data not directly related to Bitcoin's true purpose -
managing transactions - needlessly bloats the size of the blockchain,
and should not be allowed.
I am in favour of reducing bloat (including
a restriction now in the protocol to limit the size of the data
you can insert), but I think that timestamping is an acceptable
"secondary purpose" for Bitcoin, which opens the door to
a lot of potential applications, and which promotes
the use of Bitcoin.

>The easiest application of Bitcoin timestamps
is a program called opentimestamps, created by Peter Todd himself.


### 2020-11-08 {#2020-11-08}

-   First build
-   main: Read block.hash()


### 2020-11-03 {#2020-11-03}

Creat mod

-   block.rs
-   blockchain.rs
-   blockheader.rs
-   transaction.rs
-   lib.rs

Use hash crate: <https://docs.rs/sha2/0.5.2/sha2/>
for generating block's hash string.

Some other hash functions written in Rust:
<https://github.com/RustCrypto/hashes>


### 2020-11-02 {#2020-11-02}

-   Created Walnuts: my toy blockchain in Rust