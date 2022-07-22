+++
title = "Uniformly Drawing from A Set Of Connected Graphs Without Knowing The Whole Set"
description = ""
author = "David Blitz"
date = 2022-05-31T00:00:29+01:00
tags = ["graph", "connected", "uniform", "distribution", "mcmc", "oeis", "metropolis-hastings", "markov chain monte carlo", "non-isomorphic"]
draft = "False"
+++

## The Problem

Imagine wanting to empirically compute a [statistic](https://en.wikipedia.org/wiki/Statistic) on [connected graphs](https://en.wikipedia.org/wiki/Connectivity_%28graph_theory%29). The set of non-isomorphic connected graphs on a given number of nodes becomes huge [very quickly](https://oeis.org/A001349). This is visualized in the following plot with logarithmic y-axis:

![plot of number of connected graphs vs number of nodes](/images/2022-05-31-number_of_connected_graphs.svg)

This is why computing your statistic for every possible graph becomes quickly infeasible and you might want to resort to random sampling instead. Unfortunately, at least my favorite python package for graphs, [networkx](https://networkx.org/documentation/stable/reference/generators.html), does not provide any method to uniformly sample from the set of connected graphs with $n$ nodes.


## A Potential Solution

In order to draw from the set of non-isomorphic simple graphs on $n$ vertices uniformly, I am going to use the Metropolis-Hastings Algorithm as described in [this book by Roger D. Peng](https://bookdown.org/rdpeng/advstatcomp/metropolis-hastings.html) for instance. 

The Metropolis-Hastings Algorithm is more like a template which one can use to approximate a desired target distribution given an available proposal distribution. Specifically, the algorithm needs access to the conditional proposal distribution $q(y | x)$ and a ratio $\frac{p(y)}{p(x)}$ where $p(\cdot)$ is the desired target distribution. Its output converges to a sample-set from the target distribution $p(\cdot)$, if you let the algorithm run for *enough* iterations. Finding out when we have run *enough* iterations is out of scope for this post, however.


In the discussed problem setting, the target distribution $p(\cdot)$ is the uniform distribution on the set of all non-isomorphic, connected simple graphs on $n$ vertices. I am going to call this set $\mathcal{G}_n$ in the following. Since $p(G) = p(G')$ for all graphs $G, G' \in \mathcal{G}_n$, the ratio $\frac{p(G')}{p(G)}$ just equals $1$.

Now, we need to choose a conditional proposal distribution $q(G' | G)$. This is basically a transition rule to randomly choose a neighbouring graph $G'$ for a given graph $G$. The transition rule needs to fulfill the condition that we can reach any graph $H$ from any graph $G$ with a series of intermediate graphs $G_1, \dots, G_k$ such that $q(G_1 | G), \dots q(G_{i+1} | G_i), \dots, q(H | G_k) > 0$.


I chose a straightforward conditional proposal distribution $q(G' | G)$ by creating a list $\mathcal{N}(G)$ of neighbouring graphs of $G$ and taking a uniformly random $G'$ from this list. 
The neighbor list, $\mathcal{N}(G)$, is defined by iterating through all unordered pairs of vertices $e = \{ v, w \}$ and checking if they are an edge of $G$ or not. If $\{ v, w \}$ is not an edge of $G$, we just add $G + e$ to the list $\mathcal{N}(G)$. If $e$ is an edge of $G$ however, we first check if $G - e$ is connected. If $G - e$ is connected, we add it to $\mathcal{N}(G)$. If $G - e$ is not connected, we just ignore it. Here we used the shorthand $G + e$ to denote the graph $G$ with edge $e$ added to it and $G - e$ with edge $e$ removed from it.
In a last step, we filter $\mathcal{N}(G)$ such that it contains only non-isomorphic graphs.

With these choices, the Metropolis-Hastings Algorithm looks as follows:

1. Initialize the list of samples $S \leftarrow \emptyset$
2. Initialize $G$.
3. Uniformly draw a graph $G'$ from $\mathcal{N}(G)$
4. Compute the acceptance ratio $\alpha \leftarrow \min( \frac{|\mathcal{N}(G)|}{|\mathcal{N}(G')|}, 1)$
5. Uniformly draw a rational number $u$ from the interval $[0, 1]$
6. If $u \leq \alpha$ set $G \leftarrow G'$
7. Add $G$ to the list $S$
8. After *enough* iterations return $S$ - otherwise go to step 3.

## Next Steps

So far we have seen an idea for an instantiation of the Metropolis-Hastings Method to sample uniformly from the connected graphs on a given number of vertices. From here, I see two ways to test the idea. The first one would be to somewhat become an expert on MCMC and produce formal proofs that the method is working well, i.e. that my proposal distribution is indeed usable for Metropolis-Hastings and then give asymptotic convergence rates. The second way to test the idea would be to just code up the idea and see if it gives promising experimental results on small graphs. In the spirit of [testing ideas quickly](https://gregorygundersen.com/blog/2020/08/05/antifragile-ideas/), I want to explore my experimental results in a follow-up post.
