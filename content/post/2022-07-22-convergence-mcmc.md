+++
title = "How to Measure Convergence of MCMC Methods?"
description = ""
author = "David"
date = 2022-07-22T12:42:13+02:00
tags = ['convergence', 'metropolis-hastings', 'mcmc', 'state space', 'total variation distance', 'step distribution', 'mixing time', 'random walk']
draft = false
+++


*In the last post, we have seen a method for producing a series of samples of connected simple graphs where each sample exclusively depends on the previous sample. 
In this post, we will provide some statements of theorems and definitions which were a bit implicit in the previous post.*


The Metropolis-Hastings method that we chose in the last post guarantees that our series of samples will at some point 'look' like it has been drawn independently from our target distribution. The target distribution in our case was the uniform distribution on the family of connected,  simple graphs up to isomorphism.
'Looking like' an independently drawn sample is not a mathematical term. That is why we are going to talk about 'convergence' to the target distribution. Even though that is a more mathematical term it still needs to be defined in our context. 
Our general context is the following. We have a finite event space - which is also called state space. Specifically our state space is the set of connected, simple graphs on a given number of nodes up to isomorphism. We want to sample from our target distribution which is the uniform distribution on the finite state space. For that we have a dependent distribution $p(X' | X)$ which gives a rule how to sample a new state depending on the previous state. In our case, this dependent distribution is given by an instantiation of the Metropolis-Hastings method. Applying these dependent sampling steps a certain amount of times we obtain a series of states $(s_0, \dots, s_N)$. According to a theorem, which can be found [here](https://projecteuclid.org/journalArticle/Download?urlId=10.1214%2Faos%2F1033066201) in formula (9), we are guaranteed that by performing more and more of these dependent sampling steps we will obtain a single state $X_n$ that we can treat as if it had been drawn from a distribution that is arbitrarily close to the target distribution.

## What Does Convergence Even Mean?
To make this more formal, we need to define the distribution of a state after $n$ steps from starting at an initial state $s$ which is the conditional $n$-step distribution of the Markov Chain Algorithm, 
$$
p^n_{s} (s_n) := \mathbb{P}(X_n = s_n | X_0=s).
$$
We also need to define what we mean by the $n$-step distribution being close to the target distribution $\pi(X)$. For this, we first consider the [*total variation distance*](https://en.wikipedia.org/wiki/Total_variation_distance_of_probability_measures) of the two discrete distributions: 
$$
d_{TV} (p^n_{s}, \pi) = \frac{1}{2} \sum_{s_n \in S}  | p^{n}_{s}(s_n) - \pi(X=s_n) |
$$
for a specific initial state $s$.

This distance fulfills the axioms of a [metric space](https://en.wikipedia.org/wiki/Metric_space) for which we have a notion of [convergence](https://en.wikipedia.org/wiki/Limit_of_a_sequence).

The $n$-step distribution defined by a Metropolis-Hastings Markov Chain, now, is [guaranteed](https://projecteuclid.org/journalArticle/Download?urlId=10.1214%2Faos%2F1033066201) to converge for each initial state to the target distribution.

## Mixing Times
Finally, I want to briefly mention a derived measure of convergence speed since it is used a lot in the literature on MCMC: the *$\epsilon$-mixing time* at a state $s$  which we define according to [Chapter 2 of this thesis](http://dx.doi.org/10.25673/2241). We consider again the $n$-step distributions $p^n_{s}$. The *$\epsilon$-mixing time* is the minimum number of steps at which the total variation distance of the $n$-step distributions at $s$ to the target distribution has dropped below $\epsilon$. Or in a formula:
$$
\tau_{s}(\epsilon) = \min \\{N \in \mathbb{N} \mid \forall n \geq N, \space d_{TV}(p^n_{s}, \pi) \leq \epsilon \\}.
$$
The global $\epsilon$-*mixing time* is just the maximum of this expression over all possible initial states:
$$
\tau(\epsilon) = \max_{s \in S}\tau_{s}(\epsilon) = \max_{s \in S} \min \\{N \in \mathbb{N} \mid \forall n \geq N, \space d_{TV}(p^n_{s}, \pi) \leq \epsilon \\}.
$$

## Conclusion
So we can think of dependent sampling in the discrete setting by thinking of a random walk on the state graph. Convergence of the markov chain means, that we lose arbitrarily much information about the initial state the longer we continue our walk. Convergence to the target distribution then means the following: imagine we have a large amount of random walkers all starting at the same initial state. Then, after more and more steps, the distribution of their current locations will be more and more indistinguishable from the target distribution and their initial state will be more and more irrelevant.

To formally measure, how quickly the initial state becomes irrelevant, we use the total variation distance and mixing times of the $n$-step distribution.

Both total variation distance and mixing times, can be empirically estimated by experiments. For instance, by empirically estimating these two statistics, one could get a first gauge of the convergence speed of the metropolis-hastings algorithm that I presented previously. This is what I would like to do in a follow-up blog post.
