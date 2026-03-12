---
layout: default
title: "Advent of Code Input Parsing"
description: Parsing input in Advent of Code
date: 2026-02-22
permalink: /posts/aoc-input-parsing/
---

# Advent of Code Input Parsing
*{{ page.date | date: "%B %-d, %Y" }}*

I've gotten tripped up two years on a row now with parsing this style of input:

```
3-5
10-14
16-20
12-18

1
5
8
11
17
32
```

This year it was on [day 5](https://adventofcode.com/2025/day/5). Coincidentally
in 2023 it was also on [day 5](https://adventofcode.com/2023/day/5).

I always follow the same line of reasoning - "let's read in the entire input in
as a string and split it into two string "chunks" (the top chunk and the bottom
chunk). Then split those chunks into lines."

```python
import sys
input_str = sys.stdin.read()

ingredient_ranges_str, ingredient_ids_str = input_str.split('\n\n')

ingredient_ranges = ingredient_ranges_str.split('\n')
ingredient_ids = ingredient_ids_str.split('\n')
```

Seems reasonable enough. The two chunks are separated by two newlines. So first
we split on `\n\n`. And then within those chunks we just split on `\n`.

Let's look at the first section:

```python
print(ingredient_ranges)

['3-5', '10-14', '16-20', '12-18']
```

Looks good! What about the second chunk?

```python
print(ingredient_ids)

['1', '5', '8', '11', '17', '32', '']
```

Oh no! What is that empty string doing at the end! What happened? 

This is about the point where I add a kludge/hack to filter away the trailing
empty string so I can get on with solving the problem.

But hold on a second - let's dig into this and figure out exactly why we're
getting an extra empty string in the second section but not the first!

Let's print out the two text chunks:

```python
chunks = input.split('\n\n')
print(chunks)

['3-5\n10-14\n16-20\n12-18', '1\n5\n8\n11\n17\n32\n']
```

Ah ha! We see that the second string chunk ends with a newline, whereas the first
one doesn't. Indeed, [all unix files end with a
newline](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_206).

So we *should* expect to get an empty string as the last token from
`str.split()`.

But hold on a second - why does this normally work? Why don't we always get an
empty string at the end?

Oh, well we *are* doing `str.split('\n')`. I usually just do `str.split()`
without the `\n`. Let's try that:

```python
ingredient_ids = ingredient_ids_str.split()
print(ingredient_ids)

['1', '5', '8', '11', '17', '32']
```

Hey, it works! But hold on, *why* does it work?

Well, it turns out it's because
[`string.split()`](https://docs.python.org/3/library/stdtypes.html#str.split)
with no arguments strips off leading and trailing whitespace first.

I actually had no idea that was the way it worked, which leads to some
interesting consequences. For example, out our input file could look like this:

```

    3-5
10-14
16-20
12-18

1
5
8
11
17
32

```

And we'll still get each line separated, exactly how we want:

```python
import sys
input_str = sys.stdin.read()
lines = input_str.split()
print(lines)

['3-5', '10-14', '16-20', '12-18', '1', '5', '8', '11', '17', '32']
```

Unintuitive, but it gets the job done.

Moral of the story: don't use `string.split('\n')` when parsing lines of a file
- always use `string.split()`!
