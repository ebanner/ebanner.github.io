---
layout: default
title: "Y-Combinator"
description: Understanding the Y-Combinator
date: 2024-03-28
math: true
---

# y-combinator

How to compute the factorial function with the [Y combinator](https://en.wikipedia.org/wiki/Fixed-point_combinator#Y_combinator)

## Definitions

Let $\text{Y} = \lambda f . [\lambda x . f(xx)][\lambda x . f(xx)]$

Let $\text{F} = \lambda f . \lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * f (n-1)]$

### 🚨 Key Point 🚨

$\text{YF} = \text{F} (\text{YF})$

With this Key Point™ you don't even need to know the definition of $\text{Y}$.

## $\text{YF} \ 0$

Why does $\text{YF} \ 0$ even terminate at all? 🤔

$$
\begin{align}
    \text{YF} \ 0 &= \text{F} (\text{YF}) \ 0 \\
                  &= (\lambda f. \lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * f (n-1)]) (\text{YF}) \ 0 \\
                  &= (\lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * \text{YF}(n-1)]) \ 0 \\
                  &= [\text{if } 0 == 0 \text{ then } 1 \text{ else } 0 * \text{YF}(0-1)] \\
                  &= 1
\end{align}
$$

We need laziness! Otherwise $\text{FY} = \text{F} (\text{YF}) = \text{F} (\text{F} (\text{YF})) = \text{F} (\text{F} (\cdots (\text{F} (\text{YF})) \cdots )$ would just recurse forever 😵‍💫

That is, we just expand $\text{YF}$ once then pass $\text{YF}$ to $\text{F}$ (as a thunk) then apply $0$ to that.

## $\text{YF} \ 3$ at a glance

$$
\begin{align}
    \text{YF} \ 3 &= \text{F}(\text{YF}) \ 3 \\
                         &= 3 * \text{YF} \ 2 \\
                         &= 3 * \text{F}(\text{YF}) \ 2 \\
                         &= 3 * 2 * \text{YF} \ 1 \\
                         &= 3 * 2 * \text{F} (\text{YF}) 1 \\
                         &= 3 * 2 * 1 * \text{YF} \ 0 \\
                         &= 3 * 2 * 1 * 1 \\
                         &= 6.
\end{align}
$$

## $\text{YF} \ 3$ thunkified

$$
\begin{align}
    \text{YF} \ 3 &= \text{F}(\text{YF}) \ 3 \\
                  &= (\lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * \text{YF}(n-1)]) \ 3 \\
                  &= 3 * \text{YF} \ 2 \\
                  &= 3 * \text{F}(\text{YF}) \ 2 \\
                  &= 3 * (\lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * \text{YF}(n-1)]) \ 2 \\
                  &= 3 * 2 * \text{YF} \ 1 \\
                  &= 3 * 2 * \text{F} (\text{YF}) 1 \\
                  &= 3 * 2 * (\lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * \text{YF}(n-1)]) \ 1 \\
                  &= 3 * 2 * 1 * \text{YF} \ 0 \\
                  &= 3 * 2 * \text{F} (\text{YF}) \ 0 \\
                  &= 3 * 2 * 1 * (\lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * \text{YF}(n-1)]) \ 0 \\
                  &= 3 * 2 * 1 * [\text{if } 0 == 0 \text{ then } 1 \text{ else } 0 * \text{YF}(0-1)]) \\
                  &= 3 * 2 * 1 * 1 \\
                  &= 6.
\end{align}
$$

## $\text{YF} \ 3$ full computation

$$
\begin{align}
    \text{YF} \ 3 &= \text{F}(\text{YF}) \ 3 \\
                         &= (\lambda f. \lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * f (n-1)]) (\text{YF}) \ 3 \\
                         &= (\lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * \text{YF}(n-1)]) \ 3 \\
                         &= \text{if } 3 == 0 \text{ then } 1 \text{ else } 3 * \text{YF}(3-1) \\
                         &= 3 * \text{YF} \ 2 \\
                         &= 3 * \text{F}(\text{YF}) \ 2 \\
                         &= 3 * (\lambda f. \lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * f (n-1)]) (\text{YF}) \ 2 \\
                         &= 3 * (\lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * \text{YF}(n-1)]) \ 2 \\
                         &= 3 * (\text{if } 2 == 0 \text{ then } 1 \text{ else } 2 * \text{YF}(2-1)) \\
                         &= 3 * 2 * \text{YF} \ 1 \\
                         &= 3 * 2 * \text{F} (\text{YF}) 1 \\
                         &= 3 * 2 * (\lambda f. \lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * f (n-1)]) (\text{YF}) \ 1 \\
                         &= 3 * 2 * (\lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * \text{YF}(n-1)]) \ 1 \\
                         &= 3 * 2 * (\text{if } 1 == 0 \text{ then } 1 \text{ else } 1 * \text{YF}(1-1)) \\
                         &= 3 * 2 * 1 * \text{YF} \ 0 \\
                         &= 3 * 2 * 1 * (\lambda f. \lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * f (n-1)]) (\text{YF}) \ 0 \\
                         &= 3 * 2 * 1 * (\lambda n . [\text{if } n == 0 \text{ then } 1 \text{ else } n * \text{YF}(n-1)]) \ 0 \\
                         &= 3 * 2 * 1 * 1 \\
                         &= 6.
\end{align}
$$

## Helpful resources

- [Essentials: Functional Programming's Y Combinator - Computerphile](https://www.youtube.com/watch?v=9T8A89jgeTI)
- ChatGPT
- [The Y Combinator (no, not that one) - Ayaka Nonaka](https://medium.com/@ayanonagon/the-y-combinator-no-not-that-one-7268d8d9c46)

