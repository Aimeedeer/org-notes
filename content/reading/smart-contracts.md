+++
title = "Smart Contracts"
author = ["Aimee Z"]
description = "Smart contract engineering and papers"
date = 2021-01-06
tags = ["blockchain", "smartcontract"]
categories = ["reading"]
draft = false
[menu.main]
  weight = 2012
  identifier = "smart-contracts"
+++

**[A Languge-Based Approach to Smart Contract Engineering](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2020/EECS-2020-220.html)**

> Writing Contracts as Finite State Machines
>
> A Quartz contract definition adheres to a widely used structure for describing and analyzing
computer programs known as a finite state machine. Under this organization, a contract is
in one of a fixed number of explicitly listed possible states, including a designated starting state.
>
> Insights on Language Design
>
> - Contract Lifecycles Nearly
> - Finite Virtual Resources
> - Cryptographic Hashing
> - Time-Based Logic
> - Authorization

[Quartz: A Framework for Engineering Secure Smart Contracts](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2020/EECS-2020-178.html)

-   TODO: read references

> The Quartz language contains several contract-specific features.
Many distributed ledgers, most notably Ethereum, have first-class
support for virtual currency that may be bound to contracts and
exchanged among them. State machines in Quartz use keywords to
check their balance or disburse tokens to an external contract. If
we wish to produce contracts for a ledger without first-class tokens,
we can emulate this functionality by adding an extra field and the
necessary operations to the generated implementations. Transition
authorization is treated as a first-class primitive in Quartz, unlike
in Solidity and other contract languages. Quartz allows contract
authors to express rich authorization constraints such as restricting
an operation to any member of a particular group or requiring
approval from all members of a group before it is executed. Finally,
Quartz restricts communication between state machines. A state
machine may send tokens to another state machine, but it cannot
invoke another machine’s transitions directly. This simplifies the expression and verification of contract logic. Note that Quartz makes
no assumptions about the behavior of the recipient, which may or
may not be another Quartz state machine, for model checking.
>
> Here, we formally define a subset of the operational semantics of the
Quartz DSL. Evaluation rules for expressions, which are generally
routine, are omitted for brevity. A Quartz state machine is formally
defined as a 4-tuple ⟨𝑄, 𝑞0, 𝑇 , 𝐹 ⟩ where 𝑄 is a set of states, 𝑞0 is
the initial state, 𝑇 is a set of transitions, and 𝐹 is a set of fields, each
with a specific name and type.
>
> Why Model Checking and TLA+?
>
> We chose bounded model checking as Quartz’s core verification
technique because it does not require significant intervention from
the end user, i.e., the contract author. Although a contract author
must write the invariants she would like to have verified, Quartz
fully automates the more difficult task of writing a formal specification of the contract’s behavior and its execution environment
that is suitable as input to a model checker. Model checking also
offers immediately useful feedback to the user as output — an execution trace that produces a violation of one or more of the desired
properties. This feedback helps guide a contract author in making
refinements to her state machine.
>
> TLA+ serves as Quartz’s target specification language and its
verification backend. TLA+
and its model checker, TLC, are relatively mature, well-documented, and have been successfully applied
in developing and testing significant systems [40]. More modern
model checkers have since emerged, but they tend to be inherently
tied to the semantics of particular implementation languages such
as C [26] or operate at the low level of bytecode [24]. The flexibility of TLA+
’s specification language simplifies Quartz’s task of
generating a formal contract specification. This becomes especially
important when describing the execution semantics of Solidity,
which have important differences from the semantics of traditional
programming languages. Moreover, there are ongoing efforts to
modernize verification in TLA+
, such as symbolic model checking
with SMT solvers [32], that Quartz may be able to use in the future.
>
> Specification Generation
>
> Quartz’s specification generator targets PlusCal, an intermediate
language built on top of the original TLA+
specification language.
>
> Quartz is able to translate state machine descriptions to Solidity
implementations, enabling seamless deployment once a contract
has been sufficiently validated by its developer. Quartz targets
Solidity rather than EVM bytecode for several reasons. Solidity is
more human readable than EVM bytecode, which means a contract
author may easily inspect and audit a generated implementation if
necessary. Moreover, there is an ongoing effort within the Ethereum
community to replace the original EVM with a new virtual machine
based on WebAssembly [20]. By targeting Solidity, Quartz remains
agnostic to this potential change.

[Core Concepts, Challenges, and Future Directions in Blockchain: A Centralized Tutorial](https://dl.acm.org/doi/10.1145/3366370)

> Contract Security Concerns
>
> Solidity’s syntax and basic execution semantics are, by design, very similar to those of traditional
imperative programming languages, but writing a correct and secure smart contract can be challenging. This is because Ethereum’s contract execution model and mining process introduce subtleties to Solidity’s behavior, many of which have no analogues in other programming languages
and platforms. Contract developers rely on their prior experiences and intuitions regarding the
execution of imperative code, and Solidity’s efforts to present familiar syntax can obscure the underlying blockchain’s true execution semantics. Writing a correct and secure smart contract is
particularly important, because smart contracts are immutable. When a new contract is instantiated, its code is stored on the blockchain’s ledger and cannot be changed. In Section 8, we will
summarize some of the ongoing efforts to help developers avoid introducing bugs in their smart
contracts and to defend their contracts from potential attacks.

> TOWARD ROBUST SMART CONTRACTS
>
> Because smart contracts are intended to handle management of data and enforcement of rules in
high-stakes situations, and because they are both difficult to implement correctly and impossible
to modify once deployed, there is significant interest in applying techniques from formal methods
and programming languages research to the domain of smart contracts. The general goal of these
efforts is to allow developers to reason about and establish guarantees for the behavior of contracts
before they are deployed. This interest further intensified after the theft of funds from TheDAO, a
famous contract on the main Ethereum blockchain. Here, we summarize approaches belonging
to three categories: formal analysis of existing contract code, translation of contract code into
alternative languages that facilitate formal analysis, and alternative languages to express contract
logic. We close with a brief discussion of a slightly different approach to contract defense involving
bug bounties.

[Obsidian: A safer blockchain programming language](http://obsidian-lang.com/)

> Obsidian is a new programming language for writing smart contracts, which are programs for blockchain platforms.
>
> Written in Scala and Java

[Ethereum Foundation. The solidity contract-oriented programming language](https://github.com/ethereum/solidity)

[Ethereum Foundation. The serpent contract-oriented programming language](https://github.com/ethereum/serpent)

[Pythonic Smart Contract Language for the EVM](https://github.com/vyperlang/vyper)

[Hawk: The Blockchain Model of Cryptography and Privacy-Preserving Smart Contracts](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7546538)

[Stand alone compiler for the Sophia smart contract language](https://github.com/aeternity/aesophia)

-   Written in Erlang

[Serpent](https://github.com/ethereum/serpent)

> Serpent is an assembly language that compiles to EVM code that is extended with various high-level features. It can be useful for writing code that requires low-level opcode manipulation as well as access to high-level primitives like the ABI.
>
> Being a low-level language, Serpent is NOT RECOMMENDED for building applications unless you really really know what you're doing. The creator recommends Solidity as a default choice, LLL if you want close-to-the-metal optimizations, or Viper if you like its features though it is still experimental.

Building Better Systems Podcast:
[Episode #6 Dan Guido - What the hell are the blockchain people doing & why isn't it a dumpster fire?](https://www.youtube.com/watch?v=wT-AmR7wtI8)