+++
title = "Rust Smart Contract"
author = ["Aimee Z"]
description = "Learning resources and references."
date = 2020-11-21
tags = ["blockchain", "rust", "smartcontract"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2006
  identifier = "rust-smart-contract"
+++

[rust-contract-comparison](https://github.com/brson/rust-contract-comparison)

[Why Write Smart Contracts in Rust?](http://troubles.md/why-write-smart-contracts-in-rust/)
> The future of smart contracts, in my eyes
and the eyes of many others, lies with WebAssembly.
This is a virtual machine specification
which essentially acts as a portable and simple RISC ISA - since
it matches the runtime model of
a CPU many existing languages can be compiled to it unchanged.
Apart from special-case DSLs like Solidity most languages
expose the runtime model of the CPU somehow.
Not only that, but its similarity to the CPU allows
it to be compiled to incredibly efficient machine code
without complex optimisations that can affect correctness
and increase code complexity.

[Fleetwood](https://github.com/paritytech/fleetwood)
> **Future Work**
>
> It would be nice to be able to write smart contracts
that are easily compiled for different chains
with no runtime overhead while allowing to use
specific details of the underlying chain.
While developing the Fleetwood technology stack
we are trying to uphold this future goal
by considering interoperability of new features in accordance to it.