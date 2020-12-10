+++
title = "Rust and Computer Science"
author = ["Aimee Z"]
description = "Rust and CS resources."
date = 2020-11-21
tags = ["rust", "cs"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2007
  identifier = "rust-and-computer-science"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [CS](#cs)
- [Rust language](#rust-language)
- [Rust discussion](#rust-discussion)

</div>
<!--endtoc-->


## CS {#cs}

[CS 110L: Safety in Systems Programming](https://reberhardt.com/cs110l/spring-2020/)

-   [Comparison between C and Rust](https://reberhardt.com/cs110l/spring-2020/slides/lecture-18.pdf)

[CIS 198 - Rust - Spring 2016](https://github.com/cis198-2016s)

-   [Project page](https://cis198-2016s.github.io/projects/)

[MIT6.828 Operating System Engineering](https://github.com/SmallPond/MIT6.828%5FOS)

Type system

-   [Things I Was Wrong About: Types](https://v5.chriskrycho.com/journal/things-i-was-wrong-about/1-types/)

[Refinement Types](https://arxiv.org/pdf/2010.07763.pdf)

[The Humble Programmer by Edsger W. Dijkstra](https://www.cs.utexas.edu/~EWD/transcriptions/EWD03xx/EWD340.html)

[1.1  The Elements of Programming](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-10.html)
> ****Primitive expressions****, which represent the simplest entities the language is concerned with,
>
> ****Means of combination****, by which compound elements are built from simpler ones, and
>
> ****means of abstraction****, by which compound elements can be named and manipulated as units


## Rust language {#rust-language}

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


## Rust discussion {#rust-discussion}

[ReadRust: Computer Science](https://readrust.net/computer-science)

[Reddit discussion: Opinions about using Rust instead of C in Computer Science courses](https://www.reddit.com/r/rust/comments/6nw22d/opinions%5Fabout%5Fusing%5Frust%5Finstead%5Fof%5Fc%5Fin/)

Rust weaknesses:
[This question got a bunch of discussions](https://www.reddit.com/r/rust/comments/jia2xn/what%5Fare%5Fsome%5Fof%5Frusts%5Fweaknesses%5Fas%5Fa%5Flanguage/)