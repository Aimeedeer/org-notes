+++
title = "Javascript slow, compared to wasm"
author = ["Aimee Z"]
description = "WASM vs Javascript"
date = 2021-07-01
tags = ["wasm"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2001
  identifier = "javascript-slow-compared-to-wasm"
+++

Javascript has dynamic types, which checks each step of types -- a lot of conditions.
Javascript GC needs time to run. Heap allocated everything.
Integers in Rust never touch the heap.

Wasm is approaching as fast as native code.
Wasm is basically machine code. 20% instructions to do checks.

Both C++ and Rust do their type checking in compile time but not runtime.
C is fast too. But C has weak types and is extremely unsafe.