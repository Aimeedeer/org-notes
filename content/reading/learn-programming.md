+++
title = "Learn Programming"
author = ["Aimee Z"]
description = "Learning notes from books and lectures."
date = 2021-01-17
tags = ["rust", "lisp", "programming", "cs"]
categories = ["reading"]
draft = false
[menu.main]
  weight = 2010
  identifier = "learn-programming"
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [Book: Code Complete](#book-code-complete)
- [Videos: MIT 6.001 Structure and Interpretation, 1986](#videos-mit-6-dot-001-structure-and-interpretation-1986)

</div>
<!--endtoc-->


## Book: Code Complete {#book-code-complete}

**Abstract Data Types (ADTs)**

An abstract data type is a collection of data and operations that work on that data.
The operations both describe the data to the rest of the program and allow the rest of the program
to change the data. The word “data” in “abstract data type” is used loosely.
An ADT might be a graphics window with all the operations that affect it,
a file and file operations, an insurance-rates table and the operations on it, or something else.

Understanding ADTs is essential to understanding object-oriented programming.
Without understanding ADTs, programmers create classes that are “classes” in name only—in reality,
they are little more than convenient carrying cases for loosely related collections of data and routines.
With an understanding of ADTs, programmers can create classes that are easier to
implement initially and easier to modify over time.

Thinking about ADTs first and classes second is an example of programming into a language vs. programming in one.

Abstract data types are exciting because you can use them to manipulate real-world entities
rather than low-level, implementation entities. Instead of inserting a node into
a linked list, you can add a cell to a spreadsheet, a new type of window to a list of
window types, or another passenger car to a train simulation. Tap into the power of
being able to work in the problem domain rather than at the low-level implementation domain!

**Reasons to Create a Class**

-   Model real-world objects
-   Model abstract objects
-   Reduce complexity
-   Isolate complexity
-   Hide implementation details
-   Limit effects of changes
-   Hide global data
-   Streamline parameter passing
-   Make central points of control
-   Facilitate reusable code

> NASA’s Software Engineering Laboratory studied ten projects that pursued reuse aggressively (McGarry, Waligora, and McDermott 1989). In both the object-oriented and the functionally oriented approaches, the initial projects weren’t able to take much of their code from previous projects because previous projects hadn’t estab- lished a sufficient code base. Subsequently, the projects that used functional design were able to take about 35 percent of their code from previous projects. Projects that used an object-oriented approach were able to take more than 70 percent of their code from previous projects. If you can avoid writing 70 percent of your code by planning ahead, do it!
>
>Notably, the core of NASA’s approach to creating reusable classes does not involve “designing for reuse.” NASA identifies reuse candidates at the ends of their projects. They then perform the work needed to make the classes reusable as a special project at the end of the main project or as the first step in a new project. This approach helps prevent “gold-plating”—creation of functionality that isn’t required and that unneces- sarily adds complexity.

-   Plan for a family of programs
-   Package related operations
-   Accomplish a specific refactoring

**Classes to Avoid**

-   **Avoid creating god classes**. Avoid creating omniscient classes that are all-knowing and all-powerful. If a class spends its time retrieving data from other classes using Get() and Set() routines (that is, digging into their business and telling them what to do), ask whether that functionality might better be organized into those other classes rather than into the god class (Riel 1996).
-   **Eliminate irrelevant classes**. If a class consists only of data but no behavior, ask your- self whether it’s really a class and consider demoting it so that its member data just becomes attributes of one or more other classes.
-   **Avoid classes named after verbs**. A class that has only behavior but no data is gener- ally not really a class. Consider turning a class like DatabaseInitialization() or String- Builder() into a routine on some other class.


## Videos: MIT 6.001 Structure and Interpretation, 1986 {#videos-mit-6-dot-001-structure-and-interpretation-1986}

I like this lecture a lot:
[Lecture 3A: Henderson Escher Example](https://www.youtube.com/watch?v=PEwZL3H2oKg&list=PLE18841CABEA24090&index=5)

The blur line between procedure and data.