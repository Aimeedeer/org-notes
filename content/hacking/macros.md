+++
title = "Macros"
author = ["Aimee Z"]
description = "Dive in Macros"
date = 2020-12-20
tags = ["macro", "cs"]
categories = ["hacking"]
draft = false
[menu.main]
  weight = 2012
  identifier = "macros"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Papers](#papers)
- [Posts](#posts)
- [Rust](#rust)
- [Racket](#racket)
- [Emacs and orgmode](#emacs-and-orgmode)
- [Lisp](#lisp)

</div>
<!--endtoc-->


## Papers {#papers}

-   [Composable and Compilable Macros](https://www.cs.utah.edu/plt/publications/macromod.pdf) by Matthew Flatt, University of Utah
-   [Fortifying Macros](https://www2.ccs.neu.edu/racket/pubs/icfp10-cf.pdf) by Ryan Culpepper & Matthias Felleisen, Northeastern University


## Posts {#posts}

-   [Practical macros in Racket and how to work with them](https://kevin.stravers.net/2017/11/practical-macros-in-racket-and-how-to-work-with-them.html) by Kevin R. Stravers in 2017
-   [Fear of Macros](http://www.greghendershott.com/fear-of-macros/) by Greg Hendershott in 2020
-   [Why macros?](http://www.greghendershott.com/2014/10/why-macros.html) by Greg Hendershott in 2014

> Later I remembered Matthias Felleisen boiling down macros into three main categories:
>
> 1. **Binding forms**. You can make your own syntax for binding values to identifiers, including function definition forms. You may hear people say, in a Lisp you don’t have to wait for the language designers to add a feature (like `lambda` for Java?). Using macros you can add it yourself. Binding forms is one example.
>
> 2. **Changing order of evaluation**. Something like `or` or `if` can’t really be a function, because you want it to “short-circuit” — if the first test evaluates to true, don’t evaluate the other test at all.
>
> 3. **Abstractions like domain specific langagues (DSLs)**. You want to provide a special language, which is simpler and/or more task-specific than the full/raw Lisp you’re using. This DSL might be for users of your software, and/or it might be something that you use to help implement parts of your own program.
>
> Every macro is doing one of those three things. Only macros can really do the first two, at all1. Macros let you do the last one more elegantly.


## Rust {#rust}

-   [Rust Macros](https://doc.rust-lang.org/book/ch19-06-macros.html)


## Racket {#racket}

-   [Racket Macros guide](https://docs.racket-lang.org/guide/macros.html)
-   [Racket Macros reference](https://docs.racket-lang.org/reference/Macros.html)


## Emacs and orgmode {#emacs-and-orgmode}

-   orgmode [Maro Replacement](https://orgmode.org/manual/Macro-Replacement.html)
-   [Structure of Code Blocks](https://orgmode.org/manual/Structure-of-Code-Blocks.html#Structure-of-Code-Blocks)
-   [lepisma.xyz's notes](https://lepisma.xyz/wiki/emacs/org-mode/macros.html)
-   [fniessen/org-macros](https://github.com/fniessen/org-macros/)


## Lisp {#lisp}

-   [Lisp lang Macros](https://lisp-lang.org/learn/macros)
-   Macro chapters from **Practical Common Lisp**:
    -   [Macros: Standard Control Constructs](http://www.gigamonkeys.com/book/macros-standard-control-constructs.html)
    -   [Macros: Defining Your Own](http://www.gigamonkeys.com/book/macros-defining-your-own.html)
-   [GNU Lisp macro](https://www.gnu.org/software/emacs/manual/html%5Fnode/eintr/Lisp-macro.html#Lisp-macro)
-   [GNU Emacs Lisp Macros](https://www.gnu.org/software/emacs/manual/html%5Fnode/elisp/Macros.html#Macros)
-   [Macros and Byte Compilation](https://www.gnu.org/software/emacs/manual/html%5Fnode/elisp/Compiling-Macros.html#Compiling-Macros)

> When a macro call appears in a Lisp program being compiled, the Lisp compiler calls the macro definition just as the interpreter would, and receives an expansion. But instead of evaluating this expansion, it compiles the expansion as if it had appeared directly in the program. As a result, the compiled code produces the value and side effects intended for the macro, but executes at full compiled speed. This would not work if the macro body computed the value and side effects itself—they would be computed at compile time, which is not useful.
>
> In order for compilation of macro calls to work, the macros must already be defined in Lisp when the calls to them are compiled. The compiler has a special feature to help you do this: if a file being compiled contains a defmacro form, the macro is defined temporarily for the rest of the compilation of that file.