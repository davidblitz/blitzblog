+++
title = "Mutual Information and Generalization"
description = ""
author = ""
date = 2019-02-04T17:31:33+01:00
tags = []
draft = true
+++
# I(T, Y) as Measure of Performance

The generalization error is bounded more or less by 
$$
(n-1) \text{exp} \big( -\frac{n}{|Y|-1} I(Z, Y) \big)
$$
This follows from Stein's Lemma [REF!], the definition of
 mutual information and the convexity of the KL-Divergence [see Shamir, Sabato and Tishby, 2008].

# First Principles For Learning Representations

At first, let's not start to try to minimize generalization error but by stipulating desirable properties 
of a representation \\(Z\\)of our data \\(X\\) for a task \\(Y\\).
$$
X \rightarrow Z \rightarrow Y
$$
[CHECK OUT BAHADUR]
.

* Sufficiency for the task: \\(I(Z, Y) = I(X, Y)\\)

* Minimality of representation: \\( I(X, Z) \\) minimal

* Invariance to noise: \\(N \perp Y \Rightarrow I(N, Z) = 0\\) [insensitivity]

* (Independent components: \\( \text{TC}(z) := \text{KL} \big( p(Z) \mid \prod_i p(Z_i) \big) = 0 \\) )

Corresponding optimization problem:
...

# Invariance \\( \Leftrightarrow \\) Minimality \\( \mid \\) Sufficiency

Claim: 
$$
I(N, Z) \leq I(X, Z) - I(X, Y)
$$
and there is a nuisance for which equality holds.
[HOW DOES THIS IMPLY TITLE???]

# What happens in Deep Learning?
Representation computed from Training data via SGD:
$$
w = \text{arg}\text{min} H_{p, q}(D \mid w)
$$
[UNDERSTAND!!!]

Massaging this we get the decomposition:
$$
H_{p, q} (\mathcal{D} \mid w) = A - I(\mathcal{D}, w \mid \theta),
$$
where \\(A)\\ is positive.
[VERIFY!!!]
If we minimize this aggressively, we end up with overfitting. [UNDERSTAND!!!]

* In order to minimize overfitting, we want to minimize:
$$
H_{p, q} (\mathcal{D} \mid w) + I(\mathcal{D}, w \mid \theta)
$$
--> This is intractable... [WHY???]

* Choose to upper-bound overfitting term (remove dependence on \\(\theta\\) ):
$$
L = H_{p, q}(\mathcal{D} \mid w) + \beta I(\mathcal{D}, w)
$$
--> Information Bottleneck for Weights

* Can optimize with SGVB (Stochastic Gradient Variational Bayes) [HOW???] or inductive bias of SGD
[jordan-kinderlehrer-otto '97]

[CHECK OUT FOKKER-PLANCK EQUATION]
There is a Duality Bound:
$$
g(I(w, \mathcal{D})) \leq \beta I(Z, X) + \gamma \text{TC} (Z) \leq g(I(w, \mathcal{D})) + c
$$

* Claim: minimality of the weights (a representation of the training set) induces disentanglement 
minimality (hence invariance) of the activations (a representation of the test datum)

* Tight for one layer

[see Achille, Soatto, June 2017 "On the Emergence of Invariance and Disentanglement..."]


