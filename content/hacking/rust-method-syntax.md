+++
title = "Syntax of method calls"
author = ["Aimee Z"]
description = "Idiomatic way of method calls in Rust."
date = 2021-07-01
tags = ["rust"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2001
  identifier = "syntax-of-method-calls"
+++

Method chaining while reading

-   leftwards: `a.foo().bar().whatever()`
-   rightwards: `B::bar(A::foo(Z::whatever(something)))`

a.foo() -> Type B {}

b.bar() -> Type Z {}

For the readability, you almost always use `.` to call a function.
This is the idiomatic way.