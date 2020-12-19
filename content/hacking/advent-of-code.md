+++
title = "Advent of Code"
author = ["Aimee Z"]
description = "Rust programming practice"
date = 2020-12-14
tags = ["rust"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2003
  identifier = "advent-of-code"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Read and Learned](#read-and-learned)
- [Day1](#day1)
- [Day2](#day2)
- [Day3](#day3)

</div>
<!--endtoc-->

**GitHub repo**: <https://github.com/Aimeedeer/adventofcode>


## Read and Learned {#read-and-learned}

-   [Rust Macro](https://doc.rust-lang.org/book/ch19-06-macros.html)
    -   [Macros By Example](https://doc.rust-lang.org/reference/macros-by-example.html)
    -   [Procedural Macros](https://doc.rust-lang.org/reference/procedural-macros.html)
    -   [The Little Book of Rust Macros](https://danielkeep.github.io/tlborm/book/README.html)
-   [Concise Control Flow with `if let`](https://doc.rust-lang.org/stable/book/ch06-03-if-let.html)
-   Differences between runtime and compile time. Answers from
    Stackoverflow: <https://stackoverflow.com/questions/846103/runtime-vs-compile-time>
-   [Abstract Syntax Tree (AST)](https://en.wikipedia.org/wiki/Abstract%5Fsyntax%5Ftree)


## Day1 {#day1}

[Day 1: Report Repair](https://adventofcode.com/2020/day/1)

-   Source code: <https://github.com/Aimeedeer/adventofcode/tree/master/day1>
-   Question: a better performance solution than 3 loops?
-   [Other one's practice](https://fasterthanli.me/series/advent-of-code-2020/part-1). Good to learn another totally different approach.

His code:

```rust
use itertools::Itertools;

fn main() -> anyhow::Result<()> {
    let (a, b, c) = include_str!("input.txt")
	.split('\n')
	.map(str::parse::<i64>)
	.collect::<Result<Vec<_>, _>>()?
	.into_iter()
	.tuple_combinations()
	.find(|(a, b, c)| a + b + c == 2020)
	.expect("no tuple of length 3 had a sum of 2020");

    dbg!(a + b + c);
    dbg!(a * b * c);

    Ok(())
}
```

From Rust std: [std/iter/trait.Iterator](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.map)
> `map()` is conceptually similar to a `for` loop.
However, as `map()` is lazy, it is best used when you're already working
with other iterators. If you're doing some sort of looping
for a side effect, it's considered more idiomatic to use `for` than `map()`.

[tuple\_combinations](https://docs.rs/itertools/0.9.0/itertools/trait.Itertools.html#method.tuple%5Fcombinations)


## Day2 {#day2}

[Day 2: Password Philosophy](https://adventofcode.com/2020/day/2)

-   Source code: <https://github.com/Aimeedeer/adventofcode/tree/master/day2>

Use `regex` for parsing `String`.

```rust
let re = Regex::new(r"(\d+)-(\d+) ([[:alpha:]]): ([[:alpha:]]+)")?;
```

Use `xx.get(index)` instead of `xx[index]`, in case that index number is out of range.

```rust
let password = password.chars().collect::<Vec<char>>();

if password.get(index_1) == password.get(index_2) {
    continue;
}
```

`anyhow` error handling.

```rust
let caps = re.captures(&line).ok_or(anyhow!("parse line"))?;
```

`char` counting.

```rust
let num_valid_char = password.chars().filter(|c| *c == valid_char).count();
```


## Day3 {#day3}

[Day 3: Toboggan Trajectory](https://adventofcode.com/2020/day/3)