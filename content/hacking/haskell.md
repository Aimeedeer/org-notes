+++
title = "Haskell"
author = ["Aimee Z"]
description = "Haskell programming"
date = 2020-12-20
tags = ["haskell"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2015
  identifier = "haskell"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Pieces](#pieces)
- [Tools](#tools)

</div>
<!--endtoc-->

[Learn You a Haskell for Great Good!](http://learnyouahaskell.com/chapters)

[Try Haskell in your browser!](https://tryhaskell.org/)

[Haskell via Sokoban](https://haskell-via-sokoban.nomeata.de/)


## Pieces {#pieces}

[Types and Typeclasses](http://learnyouahaskell.com/types-and-typeclasses#believe-the-type). Believe the type.
> Previously we mentioned that Haskell has a static type system. The type of every expression is known at compile time, which leads to safer code. If you write a program where you try to divide a boolean type with some number, it won't even compile. That's good because it's better to catch such errors at compile time instead of having your program crash. Everything in Haskell has a type, so the compiler can reason quite a lot about your program before compiling it.

[Thinking recursively](http://learnyouahaskell.com/recursion#thinking-recursively)
> So when trying to think of a recursive way to solve a problem, try to think of when a recursive solution doesn't apply and see if you can use that as an edge case, think about identities and think about whether you'll break apart the parameters of the function (for instance, lists are usually broken into a head and a tail via pattern matching) and on which part you'll use the recursive call.

[Learnings From Solving Advent Of Code 2020 In Haskell](https://notes.abhinavsarkar.net/2020/aoc-learnings)


## Tools {#tools}

[Why Stack](https://docs.haskellstack.org/)
> Stack is a build tool for Haskell designed to answer the needs of Haskell users new and experienced alike. It has a strong focus on reproducible build plans, multi-package projects, and a consistent, easy-to-learn interface, while providing the customizability and power experienced developers need. As a build tool, Stack does not stand alone. It is built on the great work provided by:

> The **Glasgow Haskell Compiler** (GHC), the premier Haskell compiler. Stack will manage your GHC installations and automatically select the appropriate compiler version for your project.
> The **Cabal build system**, a specification for defining Haskell packages, together with a library for performing builds.
> The **Hackage package repository**, providing more than ten thousand open source libraries and applications to help you get your work done.
> The **Stackage package collection**, a curated set of packages from Hackage which are regularly tested for compatibility. Stack defaults to using Stackage package sets to avoid dependency problems.