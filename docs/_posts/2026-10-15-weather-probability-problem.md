---
layout: default
title: "Weather Probability"
description: Delving deep in a probability weather problem
date: 2026-10-15
permalink: /posts/weather-probability-problem/
math: true
---

# Weather Probability
*{{ page.date | date: "%B %-d, %Y" }}*

Check out this weather probability problem from the fantastic [6.041 class on MIT
OCW](https://ocw.mit.edu/courses/6-041-probabilistic-systems-analysis-and-applied-probability-fall-2010/):

<img src="/assets/images/weather-prob.png" style="width: 100%; border: 1px solid black;" >

I'm amazed at how rich of a problem (with the subsequent questions) this
winds up being. It taught me so much about independence and marginalization!

I also had a really hard time with it. The "official" solution given is
generally inscrutable to me, even to this day. It seems to skip a lot of steps
and I can't figure out how they get to their framing (in their defense, I
haven't put in a ton of effort).

I was determined to do this problem as mechanistically as possible, without
skipping any steps. I really wanted to reduce it to the level of an algebra
problem where there's no "magic" in any of the steps.

So, with this in mind, Let's begin by looking at part (a):

<img src="/assets/images/weather-prob2.png" style="width: 100%; border: 1px solid black;">

Let's translate this into math. We're looking for

$$
P(F_r | {}^{\neg}F \cap R \cap S_w)
$$

where

- $F_r$ is the event that the forecast is rain
- $F$ is the event that Victor sees the forecast for that day
- $R$ is the event that it rains
- $S_w$ is the event that the season is winter.

That is, we're trying to figure out what is the probability that the forecast
is rain given that Victor didn't see the forecast, it rained, and the season
is Winter. So where do we start?

## Simplifying the expression

Well, let's see if we can simplify the expression we're trying to solve at all.
We notice we can drop the ${}^{\neg}F$ from the right side of the conditioning
bar:


$$
P(F_r | R \cap S_w)
$$

Why can we do that??


## Conditional Independence

Well, think about it intuitively --- does knowing that Victor checked the
forecast that morning influence whether the forecast is rain?  ...no, it
doesn't. The two events are completely unrelated. So we can drop the $F$.

We can do this formally, because

$$F_r \perp F | S_w \cap R.$$

That is, the forecast being rain and Victor checking the forecast are independent
given that the season is Winter and it rains. So now our expression is pretty
much as simple as it can be.

A promising next step is to rewrite it with Bayes' Rule:

$$
P(F_r | S_w \cap R)
$$

$$
= \frac{P(F_r \cap R \cap S_w)}{P(R \cap S_w)}.
$$

Okay, and... now what? How should we proceed? It was very unclear to me when I
was first working through the problem.

We know that we can break up the numerator into a chain of multiplications,
but... *how* should we break it up? 

## Thinking through our causal model

A very helpful trick is to think about the chain of events in our causal model.
We can think of our causal model like this:

$$
S \rightarrow F_r \rightarrow R.
$$

That is, on any given day, we kind of "start" with the season. Then the forecast
says whether or not it's going to rain. Then we observe whether it rains or not.
This is how our stages evolve through *time*.

Conveniently, the problem description gives us $P(S)$ (implicitly),
$P(F_r | S)$, and $P(R | F_r)$. So let's break things up in that order.

$$
\frac{P(F_r \cap R \cap S_w)}{P(R \cap S_w)}
$$

$$
= \frac{P(S_w \cap F_r \cap R)}{P(S_w \cap R)}
$$

$$
= \frac{P(S_w) P(F_r | S_w) P(R | S_w \cap F_r)}{P(S_w \cap R)}.
$$

This is good progress! But what do we do about that pesky last term
$P(R | S_w \cap F_r)$?

We arent't given it directly. We are given something *close* in $P(R | F_r)$. Is
there some way we can we get rid of the $S_w$ on the right hand side of the
conditioning bar?

## Applying conditional independence once more

Well, think about independence again! If we know $F_r$, does knowing $S_w$ give
us any more information about $R$? Well, presumably the fact that the current
season is Winter is known to the forecasters and consequently is already baked
into the forecast. So, no, knowing $S_w$ *doesn't* tell us anything more about
$R$ given $F_r$. So we can drop $S_w$ from the right hand side of the
conditioning bar.

Formally, we have

$$P(R | S_w \cap F_r) = P(R | F_r) \ \text{ since } \ R \perp S_w | F_r.$$

So now our goal becomes:

$$
= \frac{P(S_w) P(F_r | S_w) P(R | F_r)}{P(S_w \cap R)}.
$$

All the terms in the numerator are no known!. Let us now turn our attention to
the denominator.

## The denominator term

Again, we face the problem that if we expand out the denominator into a chain of
multiplications according to the order of our causal model, we're going to get a
term of $P(R | S_w)$. But we're not given that. We only have $P(F_r | S_w)$ and
$P(R | F_r)$. 

So what do we do?

## "Injecting" a variable via marginalization

Well, we can "introduce" or "inject" a variable of $F_r$ in there by
marginalizing over it e.g.

$$
P(S_w \cap R) = \sum_{F_r} P(S_w \cap F_r \cap R)
$$

by the law of total probability. Now it has all the terms in our causal model
and we can break it up again like above:

$$
\sum_{F_r} P(S_w \cap F_r \cap R) = \sum_{F_r} P(S_w) P(F_r | S_w) P(R | S_w \cap F_r)
$$

And by the same logic above, we can drop the $S_w$ from the right side of the
conditioning bar:

$$
\sum_{F_r} P(S_w) P(F_r | S_w) P(R | S_w \cap F_r) = \sum_{F_r} P(S_w) P(F_r |
S_w) P(R | F_r).
$$

So now our goal becomes

$$
\frac{P(S_w) P(F_r | S_w) P(R | F_r)}{\sum_{F_r} P(S_w) P(F_r | S_w) P(R | F_r)}.
$$

The problem statement gives us all these terms, so we can just plug them all in!

$$
\frac{P(S_w) P(F_r | S_w) P(R | F_r)}{P(S_w) P(F_r | S_w) P(R | F_r) + P(S_w)
P({}^{\neg}F_r | S_w) P(R | {}^{\neg}F_r)}
$$

$$
= \frac{(0.25) (0.70) (0.80)}{(0.25) (0.70) (0.80) + (0.25) (0.30) (0.10)}
$$

$$
= \frac{0.14}{0.14 + 0.0075}
$$

$$
= \frac{0.14}{0.1475}
$$

$$
\approx 0.9492.
$$

## Conclusion

To recap, this problem got us practice with:

- Conditional independence
- Writing down a causal model
- Marginalization

One quick and dirty way to think about what this problem got me comfortable with
is how to "drop" terms from the right hand side of the conditioning bar, "add"
terms to the left hand side, and figure out *which* terms to add/drop.
