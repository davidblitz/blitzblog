+++
title = "A Conjecture on Bohemian Matrices"
description = ""
author = "David Blitz"
date = 2021-12-29T11:00:29+01:00
tags = ["bohemian", "matrices", "proof", "open", "conjecture", "oeis", "nilpotent", "multidigraph", "acyclic", "DAG"]
draft = "False"
+++

In the past year, I found a short elementary proof of Conjecture 6 on [this website](http://www.bohemianmatrices.com/cpdb/) on bohemian matrices which is currently still marked as "open". 

Bohemian matrices are just matrices with all entries being integers.

The conjecture states that the number \\( a(n) \\) of nilpotent matrices with entries in \\(\\{ 0, 1, 2 \\} \\) and of dimension \\(n \\times n \\) is equal 
to the definition of the sequence [A188457 of the OEIS](https://oeis.org/A188457).

A nilpotent matrix is a matrix \\(A\\) such that there exists a natural number \\(k\\) such that 
 \\(A^k = 0\\).

According to a recent comment on the OEIS (from February 2021), the sequence A188457 is equal to the number of labeled acyclic \\(2\\)-multidigraphs with \\(n\\) vertices.

A digraph is a graph with directed edges and a \\(2\\)-multidigraph allows for up to \\(2\\) directed edges with same source and target, i.e. it allows for up to \\(2\\) parallel edges.

To prove the conjecture, we prove a bijective correspondence between nilpotent matrices with entries in \\(\\{0, 1, \dots, p \\}\\) and of dimension \\(n \\times n\\) on the one hand 
and 
labeled acyclic \\(p\\)-multidigraphs with \\(n\\) vertices on the other hand. 
Conjecture \\(6\\) is then covered by the special case \\(p = 2\\).

The correspondence which we want to prove is given by adjacency matrices and we can see that as follows:
Let \\(A\\) be an adjacency matrix of a \\(p\\)-multidigraph \\(D\\) whose vertices are labeled with the numbers \\(1, \dots, n\\). 

A \\(p\\)-multidigraph corresponds to an adjacency matrix with entries in \\(\\{ 0, 1, \dots, p \\}\\).

Now, consider a natural power \\(A^k\\). The entry with indices \\((i, j)\\) of \\(A^k\\) counts the number of walks from \\(i\\) to \\(j\\) with exactly \\(k\\) steps 
([proof](https://www.um.edu.mt/library/oar/bitstream/123456789/24439/1/powers%20of%20the%20adjacency%20matrix.pdf)).

So \\(A\\) is nilpotent if and only if the corresponding multidigraph, \\(D\\), doesn't contain any walks for some fixed length \\(K \in \mathbb{N}\\). But this is equivalent to the multidigraph, \\(D\\), being acyclic which finishes the proof.

Note that the special case \\(p=1\\) of our proof also covers Conjecture 1 on the [same website](http://www.bohemianmatrices.com/cpdb/).