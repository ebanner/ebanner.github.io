---
layout: default
title: "Weak Vertices"
description: Understanding Kattis Weak Vertices in APL
date: 2026-06-25
permalink: /posts/weak-vertices/
math: true
---

# Understanding Kattis Weak Vertices
*{{ page.date | date: "%B %-d, %Y" }}*

Kattis [Weak Vertices](https://open.kattis.com/problems/weakvertices) has a
deceptively simple looking APL solution:

```apl
⍸∨/(~I)×+.×⍨A
```

where `A` is the adjacency matrix and `I` is the identity matrix.

Let's expand out the solution to make it more clear what's happening:

```apl
⍸∨/(~I)×A+.×A
```

All we are doing here is matrix multiplying `A` by itself, then element-wise
multiplying by the inverse identity matrix, then computing a bitwise-or along the
rows, then finally doing an argwhere to get which rows are positive.

How does a moderately complex looking problem have such a simple looking solution?

Well, let's consider the following graph:

<img src="/assets/images/triangle-graph.svg" style="max-width:300px; width:100%;">

Let's consider two vertices `A` and `B`. Computing which vertices are weakly
connected is kind of hard. So we're going to solve an auxillary task and use
that result to compute the result we want.

So instead, what would it mean for two nodes to be the opposite of weakly
connected? i.e. *strongly* connected? It means they are connected to each other
*and* have one common neighbor.

Are nodes `A` and `B` strongly connected? Well, they are connected to each other
✔️ *and* they both have a common neighbor (C) ✔️ so yes ✔️ they are strongly
connected.

If we can compute which nodes are strongly connected, then the rest must be
weakly connected.

## Adjacency matrices

Our graph has the following adjacency matrix:

```
A 0 1 1
B 1 0 1
C 1 1 0
```

Now what happens if we dot together the rows for `A` and `B`?

```
A • B = [0 1 1] • [1 0 1] = 1
```

We get 1. Why did we get 1?

Well, we got 1 because the last element of the `A` and `B` row are both 1. This
is because they both have `C` as a neighbor. So that element from the dot
product survives.

The other two elements are necessarily guaranteed not to survive. This is
becuase `A` is not connected to itself and `B` is not connected to itself.

So when we dot together two rows, the number we get is the number of neighbors
that they share in common.

So if we dot two rows, if the result is greater than zero, then those two nodes
are strongly connected!

As a final optimization -- row `i` and column `i` are equal for all `i`.  So
instead of picking out row `i` and dotting it with row `j`, we can just dot
together row `i` and column `j`.

# Enter matrix multiplication

Matrix multiplication is just a quick way to dot all the rows with all the
columns.

So the result will be for each row, it will get dotted with all the columns[^1]:

```apl
(~I)×A+.×A
0 1 1
1 0 1
1 1 0
```

Now which of these nodes is strongly connected? One way to find that out is to
elementwise-or the entire row for each node. If that's 1 then it means the node
is strongly connected. If it's 0 then that means it's not strongly connected
i.e. weakly connected.  So first let's do the elementwise-or:

```apl
∨/(~I)×+.×⍨A
1 1 1
```

And then let's take the opposite of that to get weakly connected:

```apl
~∨/(~I)×+.×⍨A
0 0 0
```

Finally, we ask for all the indices where that result holds. Which gives us our
set of weakly connected nodes[^2]:

```apl
⍸~∨/(~I)×A+.×A
⍬
```

And we have our answer --- in this case none of the nodes in our graph are
weakly connected!

[^1]: We multiply by the inverse identity matrix `~I` to zero out the main
    diagonal since these values represent the number of neighbors a node shares
    with itself (which is just how many nodes its connected to) which is
    irrelevent for our purposes.

[^2]: ⍬ (pronounced "zilde") means the empty array in APL.

