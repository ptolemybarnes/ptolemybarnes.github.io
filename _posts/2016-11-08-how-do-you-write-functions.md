---
layout: post
title:  How do you write functions?
date:   2016-11-08 20:00
categories: clean_code, quotes
---
>>
  **Writing software is like any other kind of writing.** When you write a paper or an article, you get your thoughts down first, then you **massage it until it reads well**. The first draft might be clumsy and disorganized, so you wordsmith it and restructure it and refine it until it reads the way you want it to read.
  When I write functions, they come out long and complicated. They have lots of indenting and nested loops. They have long argument lists. The names are arbitrary, and there is duplicated code. **But I also have a suite of unit tests that cover every one of those clumsy lines of code.**
  So then I massage and refine that code, splitting out functions, changing names, eliminating duplication. I shrink the methods and reorder them. Sometimes I break out whole classes, all the while keeping the tests passing.In the end, I wind up with functions that follow the rules I’ve laid down in this chapter.  I don’t write them that way to start. **I don’t think anyone could.**

####  Robert Cecil Martin, Clean Code.
