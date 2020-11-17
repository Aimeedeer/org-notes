+++
title = "Data Structures in Popular Blockchains"
author = ["Aimee Z"]
description = "Data structures in popular rust blockchains."
date = 2020-11-02
tags = ["hacking", "blockchain", "rust"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2007
  identifier = "data-structures-in-popular-blockchains"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Bitcoin](#bitcoin)
- [Ethereum](#ethereum)
- [Polkadot](#polkadot)
- [NEAR](#near)
- [Nervos CKB](#nervos-ckb)
- [More to do](#more-to-do)

</div>
<!--endtoc-->

Not as simple as I thought,
data structure are designed quite differently in different projects.

Let's take a look at some examples.


## [Bitcoin](https://github.com/rust-bitcoin/rust-bitcoin) {#bitcoin}

[Bitcoin Headers](https://en.bitcoin.it/wiki/Protocol%5Fdocumentation#Block%5FHeaders)

[Block hashing algorithm](https://en.bitcoin.it/wiki/Block%5Fhashing%5Falgorithm)

>A block header contains these fields:
4  Bytes Version	       		Block version number
32 Bytes hashPrevBlock		256-bit hash of the previous block header
32 Bytes hashMerkleRoot		256-bit hash based on all of the transactions in the block
4 Bytes	 Time			Current block timestamp as seconds since 1970-01-01T00:00 UTC
4 Bytes	 Bits			Current target in compact format
4 Bytes	 Nonce			32-bit number (starts at 0)

[Rust Bitcoin Library](https://docs.rs/bitcoin/0.25.0/bitcoin/)

I started with the well-known example of Bitcoin.
Its protocol is simple and clearly explained in the whitepaper.

src: rust-bitcoin/src/blockdata/block.rs

I like developers put an introduction in front of a page!

```rust
//! Bitcoin Block
//!
//! A block is a bundle of transactions with a proof-of-work attached,
//! which commits to an earlier block to form the blockchain. This
//! module describes structures and functions needed to describe
//! these blocks and the blockchain.
//!
```

A Bitcoin block data structure is simple,
and it looks like this:

```rust
/// A Bitcoin block, which is a collection of transactions with an attached
/// proof of work.
#[derive(PartialEq, Eq, Clone, Debug)]
pub struct Block {
    /// The block header
    pub header: BlockHeader,
    /// List of transactions contained in the block
    pub txdata: Vec<Transaction>
}
```

And the blockheader:

```rust
/// A block header, which contains all the block's information except
/// the actual transactions
#[derive(Copy, PartialEq, Eq, Clone, Debug)]
pub struct BlockHeader {
    /// The protocol version. Should always be 1.
    pub version: i32,
    /// Reference to the previous block in the chain
    pub prev_blockhash: BlockHash,
    /// The root hash of the merkle tree of transactions in the block
    pub merkle_root: TxMerkleNode,
    /// The timestamp of the block, as claimed by the miner
    pub time: u32,
    /// The target value below which the blockhash must lie, encoded as a
    /// a float (with well-defined rounding, of course)
    pub bits: u32,
    /// The nonce, selected to obtain a low enough blockhash
    pub nonce: u32,
}
```

In order to save disk space, Bitcoin uses Merkle tree to compress
transaction history.
[Merkle tree](https://en.wikipedia.org/wiki/Merkle%5Ftree) is a binary hash tree.
The \`merkle\_root\` in the \`BlockHeader\` struct refers to
the root of a Merkle tree,
which indicates the proof of transaction history.

[Bitcoin Whitepaper](https://bitcoin.org/bitcoin.pdf) is simple paper that explains
its architecture.

Then let's take a look at its value field, \`txdata: Vec<Transaction>\`.

```rust
//! Bitcoin Transaction
//!
//! A transaction describes a transfer of money. It consumes previously-unspent
//! transaction outputs and produces new ones, satisfying the condition to spend
//! the old outputs (typically a digital signature with a specific key must be
//! provided) and defining the condition to spend the new ones. The use of digital
//! signatures ensures that coins cannot be spent by unauthorized parties.
//!
//! This module provides the structures and functions needed to support transactions.
//!
```

[Struct bitcoin::blockdata::transaction::Transaction](https://docs.rs/bitcoin/0.25.0/bitcoin/blockdata/transaction/struct.Transaction.html)

In Bitcoin, transactions issues started with
searching for recievers' addresses.
Then find the input UTXOs with approved signature.
So the input and output data structures are a bit different.

```rust
/// A transaction output, which defines new coins to be created from old ones.
#[derive(Clone, PartialEq, Eq, Debug, Hash)]
pub struct TxOut {
    /// The value of the output, in satoshis
    pub value: u64,
    /// The script which must satisfy for the output to be spent
    pub script_pubkey: Script
}
```

```rust
/// A transaction input, which defines old coins to be consumed
#[derive(Clone, PartialEq, Eq, Debug, Hash)]
pub struct TxIn {
    /// The reference to the previous output that is being used an an input
    pub previous_output: OutPoint,
    /// The script which pushes values on the stack which will cause
    /// the referenced output's script to accept
    pub script_sig: Script,
    /// The sequence number, which suggests to miners which of two
    /// conflicting transactions should be preferred, or 0xFFFFFFFF
    /// to ignore this feature. This is generally never used since
    /// the miner behaviour cannot be enforced.
    pub sequence: u32,
    /// Witness data: an array of byte-arrays.
    /// Note that this field is *not* (de)serialized with the rest of the TxIn in
    /// Encodable/Decodable, as it is (de)serialized at the end of the full
    /// Transaction. It *is* (de)serialized with the rest of the TxIn in other
    /// (de)serialization routines.
    pub witness: Vec<Vec<u8>>
}
```


## [Ethereum](https://github.com/openethereum/openethereum) {#ethereum}

OpenEthereum is Ethereum 1.0 Rust client.

Different from Bitcoin's data structure,
Ethereum introduces one more field.

src: openethereum/ethcore/types/src/block.rs

```rust
/// A block, encoded as it is on the block chain.
#[derive(Default, Debug, Clone, PartialEq)]
pub struct Block {
      /// The header of this block.
      pub header: Header,
      /// The transactions in this block.
      pub transactions: Vec<UnverifiedTransaction>,
      /// The uncles of this block.
      pub uncles: Vec<Header>,
}
```

Its header data structure is much more complicated compared to Bitcoin.

src: openethereum/ethcore/types/src/header.rs

```rust
/// A block header.
///
/// Reflects the specific RLP fields of a block in the chain with additional room for the seal
/// which is non-specific.
///
/// Doesn't do all that much on its own.
#[derive(Debug, Clone, Eq, MallocSizeOf)]
pub struct Header {
      /// Parent hash.
      parent_hash: H256,
      /// Block timestamp.
      timestamp: u64,
      /// Block number.
      number: BlockNumber,
      /// Block author.
      author: Address,

      /// Transactions root.
      transactions_root: H256,
      /// Block uncles hash.
      uncles_hash: H256,
      /// Block extra data.
      extra_data: Bytes,

      /// State root.
      state_root: H256,
      /// Block receipts root.
      receipts_root: H256,
      /// Block bloom.
      log_bloom: Bloom,
      /// Gas used for contracts execution.
      gas_used: U256,
      /// Block gas limit.
      gas_limit: U256,

      /// Block difficulty.
      difficulty: U256,
      /// Vector of post-RLP-encoded fields.
      seal: Vec<Bytes>,

      /// Memoized hash of that header and the seal.
      hash: Option<H256>,
}
```

I can't fully understand the header data structure
design just from the code.
I guess I need to reread [Ethereum Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf).

src: openethereum/ethcore/types/src/transaction/transaction.rs

```rust
/// A set of information describing an externally-originating message call
/// or contract creation operation.
#[derive(Default, Debug, Clone, PartialEq, Eq, MallocSizeOf)]
pub struct Transaction {
      /// Nonce.
      pub nonce: U256,
      /// Gas price.
      pub gas_price: U256,
      /// Gas paid up front for transaction execution.
      pub gas: U256,
      /// Action, can be either call or contract create.
      pub action: Action,
      /// Transfered value.
      pub value: U256,
      /// Transaction data.
      pub data: Bytes,
}
```

Yellow paper:
>The Transaction. A transaction (formally, T) is a
single cryptographically-signed instruction constructed by
an actor externally to the scope of Ethereum.
on1
>
>There are two types of transactions: those
which result in message calls and those which result in
the creation of new accounts with associated code (known
informally as ‘contract creation').

```rust
/// Transaction action type.
#[derive(Debug, Clone, PartialEq, Eq, MallocSizeOf)]
pub enum Action {
      /// Create creates new contract.
      Create,
      /// Calls contract at given address.
      /// In the case of a transfer, this is the receiver's address.'
      Call(Address),
}
```

The "contract" in the code comments are referring to a "smart contract"
on the Ethereum blockchain platform, which can be considered
as backend service in traditional internet programming,
and a dapp, which with the full name of "decentralized application",
can be regarded as the frontend.


## [Polkadot](https://github.com/paritytech/polkadot) {#polkadot}

Polkadot is built on Substrate, a blockchain framework.
The Substrate doc has explainations of its [Block Structure](https://substrate.dev/docs/en/knowledgebase/learn-substrate/extrinsics#block-structure)
> A block in Substrate is composed of a header and
  an array of extrinsics. The header contains a
  block height, parent hash, extrinsics root, state root, and digest.

src: core-primitives/src/lib.rs

src: parachain/test-parachains/adder/src/lib.rs

```rust
#[derive(Default, Clone, Hash, Eq, PartialEq, Encode, Decode)]
pub struct HeadData {
      /// Block number
      pub number: u64,
      /// parent block keccak256
      pub parent_hash: [u8; 32],
      /// hash of post-execution state.
      pub post_state: [u8; 32],
}
```

```rust
/// Block data for this parachain.
#[derive(Default, Clone, Encode, Decode)]
pub struct BlockData {
      /// State to begin from.
      pub state: u64,
      /// Amount to add (overflowing)
      pub add: u64,
}
```

```rust
/// Execute a block body on top of given parent head, producing new parent head
/// if valid.
pub fn execute(
      parent_hash: [u8; 32],
      parent_head: HeadData,
      block_data: &BlockData,
) -> Result<HeadData, StateMismatch> {
      debug_assert_eq!(parent_hash, parent_head.hash());

      if hash_state(block_data.state) != parent_head.post_state {
	      return Err(StateMismatch);
      }

      let new_state = block_data.state.overflowing_add(block_data.add).0;

      Ok(HeadData {
	      number: parent_head.number + 1,
	      parent_hash,
	      post_state: hash_state(new_state),
      })
}
```

src: substrate/primitives/blockchain/src/header\_metadata.rs

```rust
/// Handles header metadata: hash, number, parent hash, etc.
pub trait HeaderMetadata<Block: BlockT> {
      /// Error used in case the header metadata is not found.
      type Error;

      fn header_metadata(
	      &self,
	      hash: Block::Hash,
      ) -> Result<CachedHeaderMetadata<Block>, Self::Error>;
      fn insert_header_metadata(
	      &self,
	      hash: Block::Hash,
	      header_metadata: CachedHeaderMetadata<Block>,
      );
      fn remove_header_metadata(&self, hash: Block::Hash);
}
```

src: substrate/primitives/blockchain/src/backend.rs


## [NEAR](https://github.com/near/nearcore.git) {#near}

NEAR has two types of block structure that looks more or less the same.
I choose to use the "V2" version as the example code.

src: nearcore/core/primitives/src/block.rs

```rust
#[derive(BorshSerialize, BorshDeserialize, Serialize, Debug, Clone, Eq, PartialEq)]
pub struct BlockV2 {
    pub header: BlockHeader,
    pub chunks: Vec<ShardChunkHeader>,
    pub challenges: Challenges,

    // Data to confirm the correctness of randomness beacon output
    pub vrf_value: near_crypto::vrf::Value,
    pub vrf_proof: near_crypto::vrf::Proof,
}
```

src: nearcore/core/primitives/src/block\_header.rs

```rust
/// Versioned BlockHeader data structure.
/// For each next version, document what are the changes between versions.
#[derive(BorshSerialize, BorshDeserialize, Serialize, Debug, Clone, Eq, PartialEq)]
pub enum BlockHeader {
    BlockHeaderV1(Box<BlockHeaderV1>),
    BlockHeaderV2(Box<BlockHeaderV2>),
}
```

Still, we choose to use V2 code.

```rust
/// V1 -> V2: Remove `chunks_included` from `inner_reset`
#[derive(BorshSerialize, BorshDeserialize, Serialize, Debug, Clone, Eq, PartialEq)]
#[borsh_init(init)]
pub struct BlockHeaderV2 {
    pub prev_hash: CryptoHash,

    /// Inner part of the block header that gets hashed, split into two parts, one that is sent
    ///    to light clients, and the rest
    pub inner_lite: BlockHeaderInnerLite,
    pub inner_rest: BlockHeaderInnerRestV2,

    /// Signature of the block producer.
    pub signature: Signature,

    /// Cached value of hash for this block.
    #[borsh_skip]
    pub hash: CryptoHash,
}
```

src: nearcore/core/primitives/src/sharding.rs

```rust
#[derive(BorshSerialize, BorshDeserialize, Serialize, Clone, PartialEq, Eq, Debug)]
#[borsh_init(init)]
pub struct ShardChunkHeaderV2 {
    pub inner: ShardChunkHeaderInner,

    pub height_included: BlockHeight,

    /// Signature of the chunk producer.
    pub signature: Signature,

    #[borsh_skip]
    pub hash: ChunkHash,
}
```

```rust
#[derive(BorshSerialize, BorshDeserialize, Serialize, Clone, PartialEq, Eq, Debug)]
pub struct ShardChunkHeaderInner {
    /// Previous block hash.
    pub prev_block_hash: CryptoHash,
    pub prev_state_root: StateRoot,
    /// Root of the outcomes from execution transactions and results.
    pub outcome_root: CryptoHash,
    pub encoded_merkle_root: CryptoHash,
    pub encoded_length: u64,
    pub height_created: BlockHeight,
    /// Shard index.
    pub shard_id: ShardId,
    /// Gas used in this chunk.
    pub gas_used: Gas,
    /// Gas limit voted by validators.
    pub gas_limit: Gas,
    /// Total balance burnt in previous chunk
    pub balance_burnt: Balance,
    /// Outgoing receipts merkle root.
    pub outgoing_receipts_root: CryptoHash,
    /// Tx merkle root.
    pub tx_root: CryptoHash,
    /// Validator proposals.
    pub validator_proposals: Vec<ValidatorStake>,
}
```

src: nearcore/core/primitives/src/challenge.rs

```rust
#[derive(BorshSerialize, BorshDeserialize, Serialize, PartialEq, Eq, Clone, Debug)]
#[borsh_init(init)]
pub struct Challenge {
    pub body: ChallengeBody,
    pub account_id: AccountId,
    pub signature: Signature,

    #[borsh_skip]
    pub hash: CryptoHash,
}

pub type Challenges = Vec<Challenge>;
```

Seems NEAR heavily typed their data.
I couldn't find NEAR's architecture design
from its paper or documentation.


## [Nervos CKB](https://github.com/nervosnetwork/ckb) {#nervos-ckb}

src: ckb/util/types/src/core/cell.rs

```rust
#[derive(Clone, Eq, PartialEq, Default)]
pub struct CellMeta {
    pub cell_output: CellOutput,
    pub out_point: OutPoint,
    pub transaction_info: Option<TransactionInfo>,
    pub data_bytes: u64,
    /// In memory cell data and its hash
    /// A live cell either exists in memory or DB
    /// must check DB if this field is None
    pub mem_cell_data: Option<(Bytes, Byte32)>,
}
```

src: ckb/util/types/src/generated/blockchain.rs

```rust
#[derive(Debug, Default)]
pub struct BlockBuilder {
    pub(crate) header: Header,
    pub(crate) uncles: UncleBlockVec,
    pub(crate) transactions: TransactionVec,
    pub(crate) proposals: ProposalShortIdVec,
}
```

```rust
#[derive(Debug, Default)]
pub struct UncleBlockBuilder {
    pub(crate) header: Header,
    pub(crate) proposals: ProposalShortIdVec,
}
```

There is almost no comment on these codes,
and I don't understand that \`ProposalShortIdVec\` means.
How they define \`UncleBlockVec\`, which raises
new questions about "an uncle block".
I hope there is a meaningful explanation here
to eliminate the confusion of learning its code.

I finally found an [explaination of data structure](https://github.com/nervosnetwork/rfcs/blob/master/rfcs/0019-data-structures/0019-data-structures.md)
from its GitHub doc.
It would be much convenient if I could
read this information in code comments.

In Bitcoin, the uncle block is stored in the header data;
In CKB, it is stored in the block directly as a field.
I don't have any further thoughts here at the moment.

After reading this page, I still don't know
what \`ProposalShortIdVec\` is.
I guess that might because I lack some pre-knowledge to start with.

src: ckb/util/types/src/core/cell.rs

```rust
#[derive(Default)]
pub struct CellMetaBuilder {
    cell_output: CellOutput,
    out_point: OutPoint,
    transaction_info: Option<TransactionInfo>,
    data_bytes: u64,
    mem_cell_data: Option<(Bytes, Byte32)>,
}
```

src: ckb/util/types/src/core/transaction\_meta.rs

```rust
#[derive(Default, Debug, PartialEq, Eq, Clone)]
pub struct TransactionMeta {
    pub(crate) block_number: u64,
    pub(crate) epoch_number: u64,
    pub(crate) block_hash: Byte32,
    pub(crate) cellbase: bool,
    /// each bits indicate if transaction has dead cells
    pub(crate) dead_cell: BitVec,
}
```

src: ckb/util/types/src/core/blockchain.rs


## More to do {#more-to-do}

-   Solana