+++
title = "Advent of Code"
author = ["Aimee Z"]
description = "Rust programming practice"
date = 2020-12-14
tags = ["rust"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2001
  identifier = "advent-of-code"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Read and Learned](#read-and-learned)
- [Day1](#day1)
- [Day2](#day2)
- [Day3](#day3)
- [Day4](#day4)
- [Day5](#day5)

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

[Regex Syntax](https://docs.rs/regex/1.4.2/regex/index.html#syntax)

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

-   Source code: <https://github.com/Aimeedeer/adventofcode/tree/master/day3>

Iterator:

```rust
// skip lines
for line in reader.lines().step_by(move_down) { ... }

// get interator's index while looping
for (line_index, line_value) in reader.lines().enumerate() { ... }
```

Put mutable variables in the same block of code while changing their values.
For example, the `index` below.

```rust
for line in reader.lines().step_by(move_down) {
    let rules = line?;
    let rules = rules.chars().collect::<Vec<char>>();
    let char_num = rules.len();

    if rules[index] == '#' {
      tree_num += 1;
    }

    index += move_right;
    index %= char_num;
}
```


## Day4 {#day4}

[Day 4: Passport Processing](https://adventofcode.com/2020/day/4)

-   Source code: <https://github.com/Aimeedeer/adventofcode/tree/master/day4>

Learned to organize a mod:

-   `fn main` first
-   Data types and their impls second
-   Functions that are used in `fn main` follow
-   Detailed functions(sub functions) that are used in previous functions

Write a generic `fn` inside another function:

```rust
fn validate<T>(dest: &mut Option<T>,
	     reference: &str,
	     v: impl FnOnce(&str) -> Result<T>) {
    *dest = v(reference).ok();
}

for raw_item in raw_passport {
    let item = raw_item.split(':').collect::<Vec<_>>();
    match item[0] {
      "pid" => validate(&mut new_passport.pid, item[1], validate_pid),
      "cid" => validate(&mut new_passport.cid, item[1], validate_cid),
      "eyr" => validate(&mut new_passport.eyr, item[1], validate_eyr),
      "byr" => validate(&mut new_passport.byr, item[1], validate_byr),
      "iyr" => validate(&mut new_passport.iyr, item[1], validate_iyr),
      "ecl" => validate(&mut new_passport.ecl, item[1], validate_ecl),
      "hcl" => validate(&mut new_passport.hcl, item[1], validate_hcl),
      "hgt" => validate(&mut new_passport.hgt, item[1], validate_hgt),
      _ => {},
    };
}
```

Put parsing function in another method and make rules as a reference:

```rust
fn parse_and_capture<T: FromStr>(rule: &str, input: &str, msg: &str) -> Result<T> {
    let re = Regex::new(rule).unwrap();
    let caps = re.captures(input).ok_or(anyhow!("invalid {}: {}", msg, input))?;

    let output = caps[1].parse::<T>().map_err(|_| anyhow!("error parsing {}: {}", msg, input))?;
    Ok(output)
}
```

We can also set Regex to a global varial so that it doesn't need to be
created each time in the loop.
[Burntsushi's code](https://github.com/BurntSushi/advent-of-code/blob/master/aoc04/src/main.rs) (in 2018) is a good example:

```rust
lazy_static! {
    static ref RE: Regex = Regex::new(r"(?x)
	\[
	    (?P<year>[0-9]{4})-(?P<month>[0-9]{2})-(?P<day>[0-9]{2})
	    \s+
	    (?P<hour>[0-9]{2}):(?P<minute>[0-9]{2})
	\]
	\s+
	(?:Guard\ \#(?P<id>[0-9]+)\ begins\ shift|(?P<sleep>.+))
    ").unwrap();
}
```


## Day5 {#day5}

[Day 5: Binary Boarding](https://adventofcode.com/2020/day/5)

-   Source code: <https://github.com/Aimeedeer/adventofcode/tree/master/day5>

Use const for `8`. The magical number is defined in the problem description.

```rust
const NUM_COLUMNS: u32 = 8;
let seat_id = row_id * NUM_COLUMNS + column_id;
```

Abstract functions `get_row_id` and `get_column_id` as `search_id`.

```rust
fn get_row_id(input: &str) -> Result<u32> {
    search_id(input, 128, 'F', 'B', "row id")
}

fn get_column_id(input: &str) -> Result<u32> {
    search_id(input, 8, 'L', 'R', "column id")
}

fn search_id(
    input: &str,
    len: u32,
    match_char_one: char,
    match_char_two: char,
    msg: &str
) -> Result<u32> {
    let range = (0..len).into_iter().collect::<Vec<_>>();
    let mut range = range.as_slice();

    for c in input.chars() {
      let temp_len = range.len();
      let (one, two) = range.split_at(temp_len/2);

      if c == match_char_one {
	  range = one;
      } else if c == match_char_two {
	  range = two;
      } else {
	  bail!("get {} failed", msg);
      }
    }

    let id = range[0];
    Ok(id)
}
```

Learned `(0..10).into_iter()`,
and `bail` instead of `anyhow` for returning a result with an error message.