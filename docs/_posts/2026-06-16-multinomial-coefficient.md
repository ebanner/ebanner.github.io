---
layout: default
title: "Understanding the Multinomial Coefficient"
description: Understanding the Multinomial Coefficient
date: 2026-06-16
permalink: /posts/multinomial-coefficient/
math: true
---

# Understanding the Multinomial Coefficient
*{{ page.date | date: "%B %-d, %Y" }}*

The multinomial coefficient is defined as the number of ways can we partition
the set $\\{1, 2, \ldots, n\\}$ into groups of size $k_1, k_2, \ldots, k_r$. It
is written as follows:

$$\binom{n}{k_1,\, k_2,\, \ldots,\, k_r}$$

For example, $\binom{4}{2, 2}$ is the number of ways to partition the numbers $1,
2, 3, 4$ into groups of size $2$ and $2$. It turns out there are $3$ ways to do
that[^4]:

- $\\{1, 2\\} \| \\{3, 4\\}$
- $\\{1, 3\\} \| \\{2, 4\\}$
- $\\{1, 4\\} \| \\{2, 3\\}$

How do we get $3$? In this small example, it was easy enough for us to write out
all the partitions. But what if $n$ is large? How can we derive the multinomial
coefficient for ourselves?

## Deriving it for ourselves

Well, consider this---we're going to start with something easier. Let's consider
all the permutations of $\\{1, 2, 3, 4\\}$ e.g.

- $(1, 2, 3, 4)$
- $(1, 2, 4, 3)$
- $(1, 3, 2, 4)$
- $(1, 3, 4, 2)$
- $(1, 4, 2, 3)$
- $(1, 4, 3, 2)$
- $(2, 1, 3, 4)$
- $\vdots$
- $(4, 3, 2, 1)$

There are $n! = 4! = 24$ of these permuations.

Now, let's think about grouping these permutations together into clusters, where
every cluster is going to correspond to all the permutations of a particular
partitioning.

For example, consider the partition $\\{1, 2\\} \| \\{3, 4\\}$. All of its
"permutations" are:

- $(1, 2, 3, 4)$
- $(1, 2, 4, 3)$
- $(2, 1, 3, 4)$
- $(2, 1, 4, 3)$

So we see there are $4$ ways to permute this partitioning. 

Now imagine repeating this process for each possible partitioning. We'd have a
similar set of permutations for each partitioning---each would have $4$
elements.  So if we just divide the total number of permutations by the number
of permutations in each partitioning (i.e. $4$), we should get the number of
partitionings!

Do you see why that is? It's because that's what division does![^1] Indeed
$\frac{4!}{4} = 6$ which is the number of partitionings.[^2]

How did we get $4$? Well, if we think of the number of ways to arrange elements
in a particular partitioning, we get $2! 2!$. In general, it would be $k_1! k_2!
\cdots k_r!$.

So the answer would be:

$$\binom{n}{k_1,\, k_2,\, \ldots,\, k_r} = \frac{n!}{k_1! k_2! \cdots k_r!}$$

And that's the multinomial coefficient!

[^1]: Division tells you: if we divide $n$ things into groups of size $k$, how
    many groups are there?[^3]

[^2]: This actually gives us the number of partitionings if we consider that the
    groups are different (e.g. every group has a group ID). In this case, the
    groups are all the same, so we divide by an additional $2!$ (the number of
    ways to order two groups).

[^3]: Alternatively, it says if we divide $n$ things into $k$ groups, how many
    elements are in each group?

[^4]: There are actually $6$ ways to do it if we care about the ID of the groups
    ($2$ for each of the partitionings we listed). In this blog post, we'll just
    concern ourselves when the order of the groups doesn't matter.
