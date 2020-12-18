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

- [Day1](#day1)
- [Day2](#day2)
- [Day3](#day3)

</div>
<!--endtoc-->

**GitHub repo**: <https://github.com/Aimeedeer/adventofcode>


## Day1 {#day1}

[Day 1: Report Repair](https://adventofcode.com/2020/day/1)

-   Source code: <https://github.com/Aimeedeer/adventofcode/tree/master/day1>
-   Question: a better performance solution than 3 loops?
-   [Other one's practice](https://fasterthanli.me/series/advent-of-code-2020/part-1). Good to learn a totally different approach.

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


## Day2 {#day2}

[Day 2: Password Philosophy](https://adventofcode.com/2020/day/2)

-   Source code: <https://github.com/Aimeedeer/adventofcode/tree/master/day2>
-   Question:


## Day3 {#day3}

[Day 3: Toboggan Trajectory](https://adventofcode.com/2020/day/3)