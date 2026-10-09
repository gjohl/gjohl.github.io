---
title: "Probability"
description: "Probably useful"
date: "2026-10-09"
categories: [Maths, Probability, Statistics]
---

# 0. Some Useful Approaches

-   Label objects even if they seem indistinguishable
-   Adjust for overfitting
-   Is the complement $A^C$ easier to solve for than $A$ directly? Then use the fact that $A^C = 1 - A$
-   Calculate the same thing in alternative ways to verify the answer
-   Check with specific cases – edge cases and the simplest non-trivial case

# 1. Probability and Counting

> Logic is the mathematics of certainty; probability is the mathematics of uncertainty

## 1.1. Set Theory

Concepts from set theory are useful to formalise probability.

The **sample space** $S$ of an experiment is the set of all possible outcomes of the experiment.

An **event** $A$ is a subset of the sample space S, and we say that A occurred if the actual outcome is in A.

![](images/paste-1.png){width="254"}

A concise summary of useful set notation:

![](images/paste-2.png){width="453"}

## 1.2. Naive Definition of Probability

The earliest definition of probability made some very strong assumptions that *every outcome was equally likely*, so the probability of an event $A$ occurring could be calculated by counting the number of favourable outcomes, i.e.

$$
P_{naive}(A) = \frac{\lvert A \rvert}{\lvert S \rvert}
$$

## 1.3. Multiplication Rule

If we have Experiment A with $A$ possible outcomes, followed by experiment B with \$b\$ possible outcomes, the total number of combinations is $ab$.

This follows intuitively if we consider the **tree diagram**. For example, we choose ice cream cones from one of 3 flavours and one of 2 possible cone types. The total number of possible combinations is $3 \times 2$.

Furthermore, it doesn't matter which order we picked – flavour first or cone first.

![](images/paste-4.png){width="396"}

## 1.4. Counting

### 1.4.1. Sampling with Replacement, Order Matters

We have $n$ objects and choose $k$ with replacement.

There are $n$ possibilities each time, so this is obviously

$$
N_{outcomes} = n^k
$$

### 1.4.2. Sampling without Replacement, Order Matters

We have $n$ objects and choose $k$ without replacement.

The are $n$ options for the first choice, then $n-1$ for the second, then $n-2$ for the third... all the way to $n-k+1$ for the kth.

Therefore the result is:

$$
N_{outcomes} = n(n-1)(n-2)...(n-k+1) = \frac{n!}{(n-k)!}
$$

### 1.4.3. Sampling without Replacement, Order Doesn't Matter

This is essentially the same problem as previously but we have *over-counted*. Now that we don't care about the order, we can say that there \$k!\$ ways of arranging the objects, so we divide by this to correct for the duplicates.

$$
N_{outcomes} = \frac{n!}{(n-k)! k!}
$$

By convention, this is defined as the **binomial coefficient** $n \choose k$ since it crops up so frequently.

### 1.4.4. Sampling with Replacement, Order Doesn't Matter

This is the least obvious case.

It is equivalent to the **stars and bars** problem. Imagine putting $k$ indistinguishable balls into $n$ distinguishable buckets. We are choosing $k$ objects in total, and those objects can be attributed to one of the $n$ labelled categories.

In the example below, there are $n=4$ buckets and $k=7$ balls.

![](images/paste-5.png){width="350"}

The lower picture is an alternative representation. We have $n-1$ bars ($|$) (because the ends are fixed)and $k$ stars ($*$).

Therefore, there are $n+k-1$ "things" choose from, and if we can define the arrangement by fixing the placement of the $k$ stars.

$$
N_{outcomes} = {n+k-1 \choose k}
$$

Note that this is also equivalent to fixing the bars, and by symmetry:

$$
{n+k-1 \choose k} = {n+k-1 \choose n-1}
$$

# References

-   [Stat 110 Lectures](https://www.youtube.com/playlist?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo)
-   Introduction to Probability - Blitzstein and Hwang