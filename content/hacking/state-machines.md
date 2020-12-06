+++
title = "State Machines"
author = ["Aimee Z"]
description = "Theories about state machines"
date = 2020-11-28
tags = ["cs", "state"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2003
  identifier = "state-machines"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [State machines](#state-machines)
- [Consensus](#consensus)
- [Turing completeness](#turing-completeness)
- [Oracle](#oracle)

</div>
<!--endtoc-->


## State machines {#state-machines}

[Finite-state machine](https://en.wikipedia.org/wiki/Finite-state%5Fmachine)
> The finite-state machine has less computational power
than some other models of computation such as the
Turing machine.[3] The computational power distinction
means there are computational tasks that a Turing machine
can do but an FSM cannot. This is because an FSM's
memory is limited by the number of states it has.
>
> Finite-state machines are a class of automata studied
in automata theory and the theory of computation.
In computer science, finite-state machines are widely used in
modeling of application behavior, design of
hardware digital systems, software engineering,
compilers, network protocols, and the study of computation and languages.

[State machine replication](https://en.wikipedia.org/wiki/State%5Fmachine%5Freplication)

> For the subsequent discussion a State Machine will be defined as the following tuple of values
> - A set of States
> - A set of Inputs
> - A set of Outputs
> - A transition function (Input × State → State)
> - An output function (Input × State → Output)
> - A distinguished State called Start.

> A State Machine begins at the State labeled Start.
Each Input received is passed through the transition
and output function to produce a new State and an Output.
The State is held stable until a new Input is received,
while the Output is communicated to the appropriate receiver.


## Consensus {#consensus}

[Consensus (computer science)](https://en.wikipedia.org/wiki/Consensus%5F(computer%5Fscience))


## Turing completeness {#turing-completeness}

[Turing completeness](https://en.wikipedia.org/wiki/Turing%5Fcompleteness)


## Oracle {#oracle}

[Oracle machine](https://en.wikipedia.org/wiki/Oracle%5Fmachine)
> An oracle machine can be conceived as a Turing machine
connected to an oracle. The oracle, in this context,
is an entity capable of solving some problem, which
for example may be a decision problem or a function problem.
The problem does not have to be computable; the oracle is not
assumed to be a Turing machine or computer program.
The oracle is simply a "black box" that is able to
produce a solution for any instance of a given computational problem.

> In cryptography, oracles are used to make arguments for
the security of cryptographic protocols where a hash function
is used. A security reduction for the protocol is
given in the case where, instead of a hash function,
a random oracle answers each query randomly but
consistently; the oracle is assumed to be available to
all parties including the attacker, as the hash function is.
Such a proof shows that unless the attacker solves
the hard problem at the heart of the security reduction,
they must make use of some interesting property of
the hash function to break the protocol; they cannot
treat the hash function as a black box (i.e., as a random oracle).