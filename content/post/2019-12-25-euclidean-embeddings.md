+++
title = "Euclidean Isometries of Graphs"
description = ""
author = "David Blitz"
date = 2019-12-25T16:00:29+01:00
tags = ["embedding", "graphs", "isometry", "euclidean", "metric", "space"]
draft = false 
+++

## Motivation
Last year, I became interested in (approximate) isometric embeddings of graphs with the path distance metric 
into metric spaces whose elements 
can be represented by coordinates - like \\(N \\)-dimensional 
[euclidean space](https://en.wikipedia.org/wiki/Euclidean_space)
, \\( \mathbb{E}^N \\).

Let's consider an isometric mapping from a graph with a path distance metric 
to n-dimensional euclidean space for instance: 
\\( f: G \rightarrow \mathbb{E}^N \\).
Isometric means that the distances between each two vertices \\(v, w \in G\\) is preserved by \\(f\\), i.e.
\\( d(v, w)\_G = \| f(v) - f(w) \|\_2 \\).
So, if we had such an isometry \\( f: G \rightarrow \mathbb{E}^N \\), we would have a neat way to compute distances 
between \\(v, w\\) in our original graph \\(G\\), just by computing the distances of the images \\(f(v), f(w)\\) in 
\\(\mathbb{E}^N\\). If our mapping is efficient this operation would be in \\( \mathcal{O}(N) \\) which is efficient as long as the dimension 
can be kept small.

## The Problem with Euclidean Space

The tough truth about isometrically 
embedding graphs into euclidean space, however, is that there are simple counterexamples of graphs which 
don't permit an isometric embedding into any \\(N\\)-dimensional euclidean space.

### Counterexamples
#### Cycle of Order Four
One of these counterexamples is the undirected 
cycle of order \\(4\\), 
\\(C\_4\\), i.e. the cycle contains four 
vertices \\(v\_0, v\_1, v\_2, v\_3\\) such that there is an undirected edge between 
\\(v\_i\\) and \\(v\_{i+1}\\), as well as, between \\(v\_3\\) and \\(v\_0\\).

![cycle of order 4][cycle]

We now consider the unweighted path metric on \\(C\_4\\), i.e. the weight of each edge is one, and 
the distance between any two vertices is just the number of edges in the shortest path between these vertices.

The distance matrix for this metric can be easily verified to be the following:
\\[
\begin{pmatrix} 0 & 1 & 2 & 1 \\\ 1 & 0 & 1 & 2 \\\ 2 & 1 & 0 & 1 \\\ 1 & 2 & 1 & 0\end{pmatrix}.
\\]
Here the entry in row \\( i \\) and column \\( j \\) is the distance of \\( v_i \\) to \\( v_j \\).
We see, that there are only distances 1, 2 (and 0). 
Now, we want to show that there exists no \\(4\\) points in any euclidean space, which have 
the same distance matrix with respect to the euclidean distance.

For this, we need 
[Ptolemy's Inequality](https://en.wikipedia.org/wiki/Ptolemy%27s_inequality)
 which holds four any quadrilateral in any Euclidean Space.
It states:

Given a quadrilateral with side lengths \\(s\_1, s\_2, s\_3, s\_4\\) and diagonals \\(d\_1, d\_2\\), then 

\\[s\_1s\_3 + s\_2s\_4 \geq d\_1 d\_2\\].


[INCLUDE PICTURE OF QUADRILATERAL!]

In our case we would take the images \\(f(v\_0), f(v\_1), f(v\_2), f(v\_2), f(v\_3) \\) as the four 
vertices of the quadrilateral.
Then the side lengths \\(s\_i\\) have to be all one, because the edges have weight 1. 
The diagonals on the other hand need to have length 2.

Thus the inequality becomes:

\\[
s\_1s\_3 + s\_2s\_4 = 1*1 + 1*1 = 2 \geq 4 = 2*2 = d\_1 d\_2
\\].

This is an obvious contradiction, hence there can't be an isometric embedding of 
\\(C\_4\\) into any euclidean space. 

#### Star with Three Rays 

Another counterexample is the star with three rays, 
\\( K\_{1, 3} \\). 
![star graphs with 3, 4, 5 and 6 rays][stars]
It has one center vertex \\( v\_0 \\) which is connected to three outer vertices 
\\( v\_1, v\_2, v\_3 \\) and otherwise 
there are no edges.

Now, again we have only distances 1, 2 (and 0) when we consider the hop-metric.
Let's consider the images of the star vertices under a hypothetical isometry

\\[
f: K\_{1, 3} \rightarrow \mathbb{E}^N
\\].

We must have \\( \|f(v\_0) - f(v\_i) \|\_2 = 1 \\) for \\( i \in \{1, 2, 3\} \\), as well as 
\\(
\| f(v\_i) - f(v\_j) \|\_2 = 2 
\\), for \\( i \neq j \in \{1, 2, 3\} \\).

In euclidean space we also have the triangular inequality:
\\[
\| f(v\_i) - f(v\_j) \|\_2 \leq \| f(v\_i) - f(v\_0) \|\_2 + \| f(v\_0) - f(v\_j) \|\_2.
\\]
In fact in our case, the triangular inequalities become equalities. This can only happen, 
if \\( f(v\_i), f(v\_j), f(v\_0) \\) are collinear, i.e. on a line.

More specifically, we must even have \\( f(v\_0) = \frac{f(v\_i) + f(v\_j)}{2} \\).
But this has to hold for all \\( i \neq j \\) which are not \\( 0 \\).
Hence we ge for instance
\\[
\frac{f(v\_1) + f(v\_2)}{2} = f(v\_0) = \frac{f(v\_2) + f(v\_3)}{2}.
\\]
Subtracting \\( \frac{f(v\_2)}{2} \\) on the outer-left and outer right, we get
\\[
\frac{f(v\_1)}{2} = \frac{f(v\_3)}{2}
\\]
which means that the images of \\(v\_1, v\_3\\) have to be equal and have distance \\(0\\).
This can't be, so there can't be an isometry from \\( K\_{1, 3} \\) to any euclidean space.

## Conclusion
We have seen the appeal of embedding graphs isometrically into euclidean space. However, 
there are simple examples of graphs which can't possibly embedded into euclidean space.

Now, there are some fixes for that. The most popular one seems to be to try to 
[embed graphs into hyperbolic space](http://hyperbolicdeeplearning.com/poincare-glove/).
I will try to follow up in a future post with a different solution though, which is the so called 
[power transform of finite metric spaces](https://www.sciencedirect.com/science/article/pii/S0012365X13003841).

[cycle]:https://upload.wikimedia.org/wikipedia/commons/7/70/Circle_graph_C4.svg
[stars]:https://upload.wikimedia.org/wikipedia/commons/7/7d/Star_graphs.svg
