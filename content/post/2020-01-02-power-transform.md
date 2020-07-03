+++
title = "The Power Transform of Graphs"
description = ""
author = "David Blitz"
date = 2020-07-03T16:00:29+01:00
tags = ["embedding", "graphs", "isometry", "euclidean", "metric", "space", "power-transform"]
draft = "False"
+++

## Motivation
We've seen in a previous post, that it would be desirable to have isometries between a given 
graph to some \\( N \\) dimensional euclidean space. 
We also saw that this is in general impossible - even for very simple graphs.

In this post, we are going to explore a simple fix for that, which consists in transforming 
the metric of the graph in an invertible way before looking for an isometry.

## The Power Transform
The power transform metric \\( d\_{G, c} \\) of a graph \\( G \\) with path distance metric \\( d\_{G} \\) 
arises when we just take each distance in the graph to the power \\( c \\):
\\[
d\_{G, c}(v, w) = d\_G(v, w)^c.
\\]
In order to guarantee that we still end up with a metric, the parameter \\( c \\) for the power transform 
metric needs to be \\( 0 \leq c \leq 1 \\).

In the following we are going show that the power transform indeed preserves the metric properties.
## Definition of a Metric Space

Let's quickly review the three defining properties of a metric space. A pair \\( (X, d\_X) \\) is a metric 
space if X is a set and \\(d\_X \\) is a function from pairs of elements of \\( X \\) to the non-negative real numbers:
\\[
d\_X : X \times X \rightarrow \mathbb{R}^{+}\_0.
\\]
Additionally \\( d\_X \\) must satisfy the following three properties: 

(1) \\(d\_X(x, y) = 0 \\) if and only if \\(x = y \\) 

(2) \\( d\_X(x, y) = d\_X(y, x) \\) for all \\(x, y \in X \\)

(3) \\( d\_X(x, y) \leq d\_X(x, z) + d\_X(z, y) \\) for all \\(x, y, z \in X \\)

Property (2) is called symmetry and property (3) is the famous triangle inequality.

Now it is straightforward to see, that if we apply the power transform to a general metric space 
just as we have defined it before in the case of graphs, properties (1) and (2) are preserved by the
power transform.

## The Triangle Inequality and the Power Transform

We now want to see, that the power transform preserves the triangle inequality. 
By this we mean,
\\[
d\_3 \leq d\_1 + d\_2 \Rightarrow d\_3^c \leq d\_1^c + d\_2^c,
\\]
for all \\( c \in [0, 1] \\).
We define \\( f(d) = d^c \\) and since \\(f\\) is monotonically increasing, we can see that 
\\[
d\_3 \leq d\_1 + d\_2 \Rightarrow f(d\_3) \leq f(d\_1 + d\_2).
\\]
Hence, it suffices to show that we have the inequality 
\\[
f(d\_1 + d\_2) \leq f(d\_1) + f(d\_2).
\\]
For this, 
we realize that \\( f(d) = d^c\\), for all \\( d \in (0, \infty) \\) is a 
[concave function](https://en.wikipedia.org/wiki/Concave_function) (for instance by checking that 
the second derivative is always positive on \\( \mathbb{R}^{+}\_{0} \\)).
Also we need the fact that \\( f(0) = 0 \\).
The proof of the above inequality is heavily inspired by the Wikipedia article on 
[convexity](https://en.wikipedia.org/wiki/Convex_function).
First we note that from concavity we have:
\\[
\lambda \in [0, 1] \Rightarrow f(\lambda d) = f(\lambda d + (1 - \lambda) 0) \geq \lambda f(d) + (1 - \lambda)f(0)
\\]
Now using \\( f(0) = 0 \\) we get:
\\[
\lambda \in [0, 1] \Rightarrow f(\lambda d) \geq \lambda f(d).
\\]

This allows us to apply the following little trick:
\\[
f(d\_1 + d\_2) = \frac{d\_1}{d\_1 + d\_2} f(d\_1 + d\_2) + \frac{d\_2}{d\_1 + d\_2}f(d\_1 + d\_2).
\\]
Since \\( \frac{d\_1}{d\_1 + d\_2} \\) and \\( \frac{d\_2}{d\_1 + d\_2} \\) are both between \\(0\\) and \\(1\\) 
we can apply our lemma [REF!!!] from before and conclude:
\\[
f(d\_1 + d\_2) \leq f(\frac{d\_1}{d\_1 + d\_2} (d\_1 + d\_2)) + f(\frac{d\_2}{d\_1 + d\_2} (d\_1 + d\_2))
= f(d\_1) + f(d\_2).
\\] This concludes the proof of the power transform preserving the triangle inequality :))
 

## Fixing some Counterexamples

We now proceed to embed the counterexamples from the previous post into Euclidean space after applying a power transform. 

### Cycle of Order Four

For the cycle of order four with shortest path metric
, \\( C\_4 \\) we apply a power transform with parameter \\( c=0.5 \\). 
We denote the resulting metric space by \\( C\_4^{0.5} \\) and observe that the resulting distance matrix 
becomes
\\[
\begin{pmatrix} 
0 & 1 & \sqrt{2} & 1 \\\ 1 & 0 & 1 & \sqrt{2} \\\ \sqrt{2} & 1 & 0 & 1 \\\ 1 & \sqrt{2} & 1 & 0 
\end{pmatrix}.
\\] 
But this is just the distance matrix of the corners of an ordinary square in \\( \mathbb{R}^2 \\). 
Hence we can map the vertices of \\( C\_4^{0.5} \\) to the corners of some square in \\( \mathbb{R}^2 \\) 
such that adjacent vertices are mapped to adjacent corners.

### Star with Three Rays

The other counterexample in the previous post was the star with three rays \\( K\_{1, 3} \\). 
Again, we can apply a power transform with parameter \\( c=0.5 \\) and the resulting metric space, 
\\( K\_{1, 3}^{0.5} \\), will turn out to be embeddable into \\( \mathbb{R}^3 \\):
As before, we denote the center vertex by \\( v\_0 \\) and the other vertices by \\( v\_1, v\_2, v\_3 \\).
The map \\( f: K\_{1, 3}^{0.5} \rightarrow \mathbb{R}^3 \\) can be defined by \\( f(v\_0) = 0 \\) 
and \\( f(v\_i) = e\_i \\) for the remaining \\( i= 1, 2, 3 \\). 
Since the distance between two unit vectors is just \\( \sqrt{2} \\) this works out fine.

## Conclusion
We introduced the power transform for finite metric spaces. Then we saw that the power transform 
is a fix for embedding the two simple graphs with shortest-path-metric
(\\( C\_4 \\) and \\( K\_{1, 3} \\)) which we have proven to not be 
isometrically embeddable into Euclidean space in the previous post.

It remains to be seen that we can indeed embed ANY finite graph (and even any finite metric space) into Euclidean 
space provided we apply an appropriate power transform to it.

But that is a proof for a different time :)
