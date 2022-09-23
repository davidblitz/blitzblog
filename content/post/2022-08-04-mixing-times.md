+++
title = "More on Mixing Times"
description = ""
author = "David"
date = 2022-08-04T12:43:37+02:00
tags = ['convergence', 'metropolis-hastings', 'mcmc', 'state space', 'total variation distance', 'step distribution', 'mixing time', 'random walk', 'rapid mixing']
draft = false
+++

In the [last post](../../../../2022/07/22/2022-07-22-convergence-mcmc.md), we defined convergence of an MCMC sampling algorithm via the *total variation distance* of the $n$-step distribution $p^n_s$ to the target distribution $\pi$. More specifically, the MCMC algorithm converges to the target distribution, if $d_{TV}(p^n_s, \pi)$ converges to $0$ for $n \to \infty$ for each $s$ in our finite state space $S$.
In this post, we will talk more about mixing times and motivate their study. It is mainly inspired by [these lecture notes by Alistair Sinclair](https://people.eecs.berkeley.edu/~sinclair/cs294/n7.pdf).

### Mixing times
The speed of convergence can conveniently be quantified by a single number, the mixing time, $\tau_{mix}$.

However, we first need to define a function $\tau : \mathbb{R_{+}} \to \mathbb{N}$ , which we will call $\epsilon$-mixing time as follows:
$$
\tau(\epsilon) := \min \\{ N \in \mathbb{N} \mid d_{TV}( p^n_s, \pi ) \leq \epsilon, \forall n \geq N, \forall s \in S \\} 
$$
In natural language we could say, that $\tau(\epsilon)$ tells us the minimal number of steps after which the $n$-step distribution is closer than $\epsilon$ to the target distribution - whatever initial state $s$ we are choosing.

Now, there is a neat lemma which prepares a subsequent definition:

According to [the lecture notes](https://people.eecs.berkeley.edu/~sinclair/cs294/n7.pdf), for a convergent MCMC sampling algorithm and for $\epsilon < \frac{1}{2}$, we have 
$$
\tau(\epsilon) \leq \tau \left( \frac{1}{2e} \right) \cdot \lceil \log(\epsilon^{-1}) \rceil.
$$
Hence, defining $\tau_{mix} := \tau \left( \frac{1}{2e} \right)$, we have the elegant bound:
$$
\tau(\epsilon) \leq \tau_{mix} \cdot \lceil \log(\epsilon^{-1}) \rceil.
$$

### Outlook

Many techniques have been developed, in order to prove upper bounds on mixing times for a given MCMC algorithm and I hope that in the future I'll be able to apply some of these techniques successfully to my [original problem](../../../../2022/05/31/2022-05-31-graph-mcmc) of uniformly sampling simple, connected and non-isomorphic graphs on a fixed number of vertices.
