+++
title = "Estimating Mutual Information in Bayesian Neural Networks"
description = ""
author = ""
date = 2019-02-18T13:37:00+01:00
tags = []
draft = true
+++

Let's consider a Bayesian Neural Network, where we consider the hidden layers as random variables, \\(Z_i\\).
We also consider the input, \\(X\\), and the learning task, \\(Y\\), as random variables.
Our goal is to estimate the mutual information between the input and the layers - \\(I(X, Z_i)\\) - 
as well as between the layers and the task - \\(I(Z_i, Y)\\).
For this, we will make use of the probabilistic forward propagation approximation, as introduced [in the PBP paper].

# Estimating \\(I(X, Z_i)\\)

We have
$$
I(X, Z_i) = H(Z_i) - H(Z_i | X).
$$
We can estimate each of the two entropy terms on the RHS with help of PFP:

* \\(H(Z_i)\\) can just be approximated by taking the entropy of the forward propagated distribution of \\(X\\). This distribution we approximate in turn by a multivariate gaussian.

* for the conditional entropy we have:
$$
H(Z_i | X) = \mathbb{E}_x [H(Z_i | x)] = \sum_x p(x) \int p(Z_i | x) \log p(Z_i | x) dx
$$ 
The PFP approximation results in 
$$
p(Z_i | x) \approx \mathcal{N}(Z_i | m^{i}(x), \Sigma^{i}(x) ).
$$
Using the formula for the entropy of a gaussian, we get:
$$
H(Z_i | X) = \sum_x \frac{1}{N} \frac{1}{2} \log(2 \pi e \Sigma^{i}(x)) .
$$
Notice, that this is independent of the mean of the PFP approximation.

# Estimating \\(I(Z_i, Y)\\)

Again, we express the mutual information with entropies:
$$
I(Z_i, Y) = H(Y) - H(Y | Z_i).
$$

* the first term stays constant over training, so we can just ignore it or estimate it by approximating the distribution of \\(Y\\) by a gaussian
* the conditional entropy is more tricky:
$$
H(Y | Z_i) = \sum_y \int p(y , Z_i) \log p(y | Z_i) dZ_i 
$$
$$
= \sum_x \sum_y \int p(y, x, Z_i) \log p(y | Z_i) dZ_i
$$
$$
= \sum_x \sum_y p(x, y) \int p(Z_i | x) \log p(y | Z_i),
$$

where we have used the independence of \\(Z_i\\) and \\(Y\\).
Now, we can approximate \\( p(Z_i | x) \\) by a Gaussian via PFP. In order, to integrate, we sampling \\(z_i^1, \dots, z_i^S\\) from this Gaussian, approximate
\\( p(y | z_i^s) \\) for each \\(1 \leq s \leq S\\) via PFP and finally compute
$$
\sum_s \frac{1}{S} \log p(y | z_i^s)
$$.
