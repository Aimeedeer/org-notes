+++
title = "Walnuts Hacklog"
author = ["Aimee Z"]
description = "My hacklog for Walnuts, a toy blockchain."
date = 2020-11-02
tags = ["rust", "hacking", "blockchain", "walnuts", "log"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2001
  identifier = "walnuts-hacklog"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [TODO](#todo)
- [Hacklog](#hacklog)
    - [2020-11-21](#2020-11-21)
    - [2020-11-20](#2020-11-20)
    - [2020-11-16](#2020-11-16)
    - [2020-11-13](#2020-11-13)
    - [2020-11-12](#2020-11-12)
    - [2020-11-11](#2020-11-11)
    - [2020-11-10](#2020-11-10)
    - [2020-11-08](#2020-11-08)
    - [2020-11-03](#2020-11-03)
    - [2020-11-02](#2020-11-02)

</div>
<!--endtoc-->

[Walnuts source code](https://github.com/Aimeedeer/walnuts)


## TODO {#todo}

-   sha2 for hash function
-   chrono for UTC time as timestamp
-   serde for storage, write & read to a file on disc
-   cli
-   create a new block from commandline,
    and save it in a json file
-   change read & write to serde <> io:
    <https://docs.rs/serde%5Fjson/1.0.59/serde%5Fjson/fn.from%5Freader.html>
-   deal with unwrap() -> Result
-   [ ] create transactions from commandline
-   [ ] 256k for tx signature: <https://docs.rs/k256/0.5.10/k256/>
-   [ ] nouce verify
-   [ ] keep mining
-   [ ] clean code


## Hacklog {#hacklog}


### 2020-11-21 {#2020-11-21}

Error handling progress:

```rust
pub fn load() -> Result<Self> {
    let file = File::open("walnutsdata.json");
    match file {
      Ok(file) => {
	  Ok(serde_json::from_reader(&file)?)
      },
      Err(e) => {
	  if e.kind() == ErrorKind::NotFound {
	      Ok(Blockchain::new())
	  } else {
	      Err(Error::from(e))
	  }
      }
    }

}

fn new() -> Self {
    Blockchain { blocks: Vec::new() }
}
```

Rust doc:
[Enum std::io::ErrorKind](https://doc.rust-lang.org/std/io/enum.ErrorKind.html)


### 2020-11-20 {#2020-11-20}

[Crate serde\_json](https://docs.rs/serde%5Fjson/1.0.59/serde%5Fjson/index.html)

A worth reading issue:
[Parsing 20MB file using from\_reader is slow #160](https://github.com/serde-rs/json/issues/160)

Changes in blockchain.rs: `unwrap()` -> `?`

```rust
pub fn updateblockchain(block: Block) -> Result<()> {
    let f = File::create("walnutsdata.json")?;
    let blockdata = serde_json::ser::to_writer_pretty(f, &block)?;
    println!("write to walnutsdata.json");

    let data = std::fs::read_to_string("walnutsdata.json")?;
    let deserialized: Block = serde_json::from_str(&data)?;
    println!("deserialized data from walnutsdata.json: {:?}", deserialized);
    Ok(())
}
```

The output:

```shell
$ cargo run -- mine
    Finished dev [unoptimized + debuginfo] target(s) in 1.55s
     Running `target/debug/walnuts mine`
Opt {
    cmd: Mine,
}
write to walnutsdata.json
deserialized data from walnutsdata.json: Block { header: BlockHeader { prev_block_hash: [166, 5, 88, 255, 235, 6, 147, 45, 180, 203, 105, 29, 131, 4, 186, 246, 69, 115, 54, 230, 65, 190, 65, 172, 108, 106, 33, 220, 51, 45, 68, 150], time: 1605002954274, nonce: 1 }, txs: [] }
check mine funtion: Ok(())
```

I didn't finish implementing `anyhow::Result` error handling today.


### 2020-11-16 {#2020-11-16}

-   Created a new block (Mine) from commandline,
    and saved it to walnutsdata.json
-   Learned: always thinking in type system

Test the sample code from [serde.rs](https://serde.rs/)
in my blockchain.rs code:

```rust
let serialized = serde_json::to_string(&block).unwrap();
println!("serialized = {}", serialized);

let deserialized: Block = serde_json::from_str(&serialized).unwrap();
println!("deserialized = {:?}", deserialized);
```

The output:

```shell
$ cargo run -- mine

    Finished dev [unoptimized + debuginfo] target(s) in 3.04s
     Running `target/debug/walnuts mine`
Opt {
    cmd: Mine,
}
serialized = {"header":{"prev_block_hash":[166,5,88,255,235,6,147,45,180,203,105,29,131,4,186,246,69,115,54,230,65,190,65,172,108,106,33,220,51,45,68,150],"time":1605002954274,"nonce":1},"txs":[]}
deserialized = Block { header: BlockHeader { prev_block_hash: [166, 5, 88, 255, 235, 6, 147, 45, 180, 203, 105, 29, 131, 4, 186, 246, 69, 115, 54, 230, 65, 190, 65, 172, 108, 106, 33, 220, 51, 45, 68, 150], time: 1605002954274, nonce: 1 }, txs: [] }
Genesis block is: [166, 5, 88, 255, 235, 6, 147, 45, 180, 203, 105, 29, 131, 4, 186, 246, 69, 115, 54, 230, 65, 190, 65, 172, 108, 106, 33, 220, 51, 45, 68, 150]
write to walnutsdata.txt
check mine funtion: Ok(())
```

Read blockchain data from `walnutsdata.txt`

```rust
let fout = File::open("walnutsdata.txt")?;
let mut buf_reader = BufReader::new(fout);
let mut reader = String::new();

buf_reader.read_to_string(&mut reader)?;
println!("read from walnutsdata.txt in string: {:?}", &reader);

let deserialized: Block = serde_json::from_str(&reader).unwrap();
println!("deserialized data from walnutsdata.txt: {:?}", deserialized);
```

The output of reading data:

```shell
$ cargo run -- mine


    Finished dev [unoptimized + debuginfo] target(s) in 1.09s
     Running `target/debug/walnuts mine`
Opt {
    cmd: Mine,
}
write to walnutsdata.txt
read from walnutsdata.txt in string: "{\"header\":{\"prev_block_hash\":[166,5,88,255,235,6,147,45,180,203,105,29,131,4,186,246,69,115,54,230,65,190,65,172,108,106,33,220,51,45,68,150],\"time\":1605002954274,\"nonce\":1},\"txs\":[]}"
deserialized data from walnutsdata.txt: Block { header: BlockHeader { prev_block_hash: [166, 5, 88, 255, 235, 6, 147, 45, 180, 203, 105, 29, 131, 4, 186, 246, 69, 115, 54, 230, 65, 190, 65, 172, 108, 106, 33, 220, 51, 45, 68, 150], time: 1605002954274, nonce: 1 }, txs: [] }
check mine funtion: Ok(())
```

Change to `serde_json::to_string_pretty`:

```rust
let blockdata = serde_json::to_string_pretty(&block).unwrap();

println!("write to walnutsdata.json");

let f = File::create("walnutsdata.json")?;
{
    let mut buffer = BufWriter::new(f);

    buffer.write_all(&blockdata.as_bytes())?;
    buffer.flush()?;
}

let mut fout = File::open("walnutsdata.json")?;

// future consideration: io & os performance
// let mut buf_reader = BufReader::new(fout);

let mut data = String::new();
fout.read_to_string(&mut data)?;

println!("read from walnutsdata.json in string: {}", &data);

let deserialized: Block = serde_json::from_str(&data).unwrap();
println!("deserialized data from walnutsdata.json: {:?}", deserialized);

Ok(())
```

The pretty output:

```shell
    Finished dev [unoptimized + debuginfo] target(s) in 1.08s
     Running `target/debug/walnuts mine`
Opt {
    cmd: Mine,
}
write to walnutsdata.json
read from walnutsdata.json in string: {
  "header": {
    "prev_block_hash": [
      166,
      5,
      88,
      255,
      235,
      6,
      147,
      45,
      180,
      203,
      105,
      29,
      131,
      4,
      186,
      246,
      69,
      115,
      54,
      230,
      65,
      190,
      65,
      172,
      108,
      106,
      33,
      220,
      51,
      45,
      68,
      150
    ],
    "time": 1605002954274,
    "nonce": 1
  },
  "txs": []
}
deserialized data from walnutsdata.json: Block { header: BlockHeader { prev_block_hash: [166, 5, 88, 255, 235, 6, 147, 45, 180, 203, 105, 29, 131, 4, 186, 246, 69, 115, 54, 230, 65, 190, 65, 172, 108, 106, 33, 220, 51, 45, 68, 150], time: 1605002954274, nonce: 1 }, txs: [] }
check mine funtion: Ok(())
```


### 2020-11-13 {#2020-11-13}

-   Learned:
    -   cargo clean: clean the target folder
    -   cargo run -- mysubcommand == target/walnuts mysubcommand
    -   <https://rust-cli.github.io/book/tutorial/cli-args.html>
-   Use structopt as cli in main.rs
    -   When I moved the cli related code to cli.rs, there is an error

<!--listend-->

```shell
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

```rust
use structopt::StructOpt;
```


### 2020-11-12 {#2020-11-12}

-   Cargo check / cargo build
-   `std::io` to deal with files
-   cli
    -   clap
    -   structopt: <https://docs.rs/structopt/0.3.20/structopt/>
        -   <https://rust-cli.github.io/book/tutorial/cli-args.html>


### 2020-11-11 {#2020-11-11}

-   Use Tony's `secp256k1` library
-   Serde

About serde derive:

```shell
error: cannot find derive macro `Serialize` in this scope
 --> src/block.rs:6:10
  |
6 | #[derive(Serialize, Deserialize, Debug)]
  |          ^^^^^^^^^

error: cannot find derive macro `Deserialize` in this scope
```

Then add feature in Cargo.toml

```rust
serde = { version = "1.0.117", features = ["derive"] }
```

The doc explains:
[Using derive](https://serde.rs/derive.html)


### 2020-11-10 {#2020-11-10}

-   [Rust: chrono](https://docs.rs/chrono/0.4.19/chrono/struct.DateTime.html) for UTC time
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
-   main: Read `block.hash()`


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