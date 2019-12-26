+++
title = "Visualizing Deep Neural Networks via Mutual Information"
description = ""
author = ""
date = 2019-02-08T15:24:08+01:00
tags = ["deep learning", "machine learning", "neural networks", "visualization", "dnn", "mutual information", "information theory"]
draft = true
+++
This is a presentation about the article on 
<a href="https://arxiv.org/pdf/1703.00810.pdf" target="_blank">"Opening the 
black box of Deep Neural Networks via Information" by Tishby and Schwartz-Ziv (2017).</a>

# The Information Bottleneck
* Definition of Mutual Information: 
$$
I(X, Y) = \sum_{x, y} p(x, y)\text{log}\frac{p(x,y)}{p(x)p(y)}
$$ 

* Properties of MI:
$$
I(X, Y) = H(X) - H(X \mid Y)
$$

We can also write the conditional entropy on the RHS as:
$$
H(X | Y) = \mathbb{E}_y [H(X | y)].
$$

Hence we see that mutual information becomes smaller the more peaked the conditional distribution of \\(X\\) is for any given \\(y\\).

* In machine learning we are considering representations \\(Z\\) of the input \\(X\\) and the task \\(Y\\), i.e. the Markov chain:
$$
Y \rightarrow X \rightarrow Z
$$
* Desirable properties of a representation \\(Z\\):

  1. Sufficiency: \\(I(Z, Y) = I(X, Y)\\) 

  2. Minimality: \\(I(X, Z) \rightarrow \text{min}\\), where we minimize over all *sufficient* \\(Z\\)

  3. Invariance to nuisances: \\(N \perp Y \Rightarrow I(N, Z) = 0\\)

  4. Independent components: \\(\text{TC}(Z) = \text{KL}\big(p(Z), \prod_i p(Z_i)\big) = 0\\)

It turns out 
<a href="https://arxiv.org/pdf/1706.01350.pdf" target="_blank">see Achille, Soatto, 2017</a> 
that Sufficiency and Minimality imply the other two properties.
We try to achieve a relaxed version of 1. and 2. by optimizing the Information Bottleneck Lagrangian 
over all distributions of \\(Z\\):
$$
\mathcal{L}(Z, \beta; X, Y) = I(X, Z) - \beta I(Z, Y) \rightarrow \text{min}.
$$

This was first proposed by in <a href="https://arxiv.org/abs/physics/0004057">this paper by Tishby, Pereira & Bialek from 2000</a>.
The solution to this optimization is given by the following three equations:

  1. \\( p(z \mid x) = \frac{p(z)}{N(\beta; x)} \text{exp} 
\big( -\beta \text{KL}(p(y \mid x) \mid\mid p(y \mid z))\big) \\), where \\(N(\beta; x)\\) is a normalization constant.
  
  2. \\( p(z) = \sum_x p(z \mid x) p(x) \\)

  3. \\( p(y \mid z) = \sum_x p(y \mid x) p(x \mid t) \\) 

They can be solved via an algorithm by Arimoto and Blahut.
These equations are satisfied along the so called *information curve*, which is 
monotonic, concave and separates the achievable region under the curve and the 
impossible region above the curve in the *information plane/quadrant*. [Tishby and Schwartz-Ziv write this in the paper but I couldn't find a source for this, yet]

# Mutual Information in DNN
* Tishby and Schwartz-Ziv consider DNN layers as well as input and task as random variables or more precisely as a Markov Chain:
$$
Y \rightarrow X \rightarrow Z_1 \rightarrow \cdots \rightarrow Z_L \rightarrow \hat{Y}
$$ 
* The layers can be visualized as points in the information plane with coordinates
$$
I(X, Z) \text{ and } I(Z, Y), Z \in \\{Z_1, \dots, Z_L, \hat{Y} \\}
$$
* In this context we notice how the Data Process Inequality applies to the hidden layers: For a Markov Chain
$$
Y \rightarrow X \rightarrow Z
$$
we have
$$
I(Y, X) \geq I(Y, Z)
$$

* Hence in a DNN we have:
$$
I(Y, X) \geq I(Y, Z_1) \geq \cdots \geq I(Y, Z_L) \geq I(Y, \hat{Y})
$$
and
$$
H(X) \geq I(X, Z_1) \geq \cdots \geq I(X, Z_L) \geq I(X, \hat{Y})
$$

* Here we already get a hint that deeper layers compress better [ELABORATE!] 
* Another important property of Mutual Information is reparametrization invariance: 
for bijections \\(\phi\\), \\(\psi\\), we have
$$
I(X, Y) = I(\phi(X), \psi(Y))
$$

This seems like bad news, because vanilla DNN work with deterministic functions and finite *discrete* input \\(X\\).
Hence applying functions like flipping bits will not change the Mutual Information but from experience we know that it will be impossible to learn this function. Hence Mutual Information can't give us the whole story.

# Questions for DNN visualizations in the Information Plane
1. What are the Dynamics of learning hidden layers via SGD in the Information Plane?
2. What is the effect of training sample size on the dynamics of the hidden layers?
3. What are the benefits of hidden layers?
4. What is the final location of the hidden layers?
5. Do the hidden layers form optimal IB representations?

# Experimental Setup 
* Network Architecture: up to seven hidden layers with widths
$$
12 - 10 - 7 - 5 - 4 - 3 - 2 
$$

* Inputs are 12-dimensional binary --> 4096 possible inputs \\(X\\).
* The joint distribution \\(p(X, Y)\\) is calculated via a *spherically symmetric* function \\(f(X)\\).
 This is made into a stochastic rule through the sigmoidal function 
 \\(\psi(u) = \frac{1}{1 + \text{exp}(-\gamma u)}\\) via:
$$
p(Y=1 | x) = \psi(f(X) - \theta).
$$
The threshold \\(\theta\\) is selected, such that
$$
p(Y = 1) = \sum_x p(y = 1 | x)p(x) \approx 0.5
$$
where \\(p(x)\\) is chosen to be uniform.
The sigmoidal gain \\(\gamma\\) was high enough to keep the information 
$$
I(X, Y) \approx 0.99.
$$
* The experiment is repeated 50 times with different network initializations and training sets - sampled according to \\(p(X, Y)\\)
* Each time the network learns the rule via SGD and in each epoch we estimate 
\\(I(X, Z_i)\\) and \\(I(Z_i, Y)\\) for each hidden layer.

[Tishby and Ziv say that there is more information about \\(f\\) in <a href="http://www.cs.jhu.edu/~misha/MyPapers/SGP03.pdf" target="_blank">this paper by Kazhdan et al.</a> but I didn't follow the reference, yet.
 
# Mutual Information Estimation
* MI is estimated by binning the neurons outputs between -1 and 1 into 30 different bins and assuming a uniform distribution over the 4096 possíble inputs. Hence, we get \\(p(X, Z_i)\\) directly and \\(p(Z_i, Y)\\) via
$$
p(Z_i, Y) = \sum_x p(x, Y)p(Z_i | x),
$$
where we have used the Markov Chain 
$$
Y \rightarrow X \rightarrow Z_i
$$
for every hidden layer.

# Two Optimization Phases
As we visualize the training phase, we observe a drift and a diffusion phase. During the latter, not 
only the MI between the layers and the task is optimized but also we observe compression of the input. 
![dnn-in-infoplane](/images/dnn-in-infoplane.png)
This second phase is not directly explained by the cross-entropy loss, since no regularization is used.
The transition also coincides with the norm of the mean of the gradient of the network being roughly the same 
as the standard deviation of the norm of the gradient.
![stochastic-gradient.png](/images/stochastic-gradient.png)
But an argument will be given, that this compression is due to the stochastic dynamics of the training method.

# SGD and the diffusion Phase
Tisby argues that in the diffusion phase the standard deviation of the gradient dominates the mean. Hence, it behaves like a Wiener Process and can be described by a Focker-Planck equation [see e.g. Risken (1989)]. The stationary distribution of this equation maximizes the conditional entropy, \\(H(X | Z_i)\\), and thus minimizes the mutual information
$$
I(X, Z_i) = H(X) - H(X | Z_i).
$$
QUESTION: What determines the \\(\beta\\) of the limit points of the hidden layers?

# Computational Benefit of Hidden Layers
By the dynamics of the diffusion equation, the number of training epochs is 
exponential in the resulting compression 
\\(\Delta I_X\\).
Assuming that with $L$ hidden layers, each layer \\(k\\) compresses the previous layer further by 
\\(\Delta I^k_X\\). Then, total compression approximately breaks down into \\(K\\) smaller steps,
\\(\Delta I_X \approx \sum_k \Delta I^k_X \\). Since
$$
\text{exp}(\sum_k \Delta I^k_X \gg \sum_k \text{exp} (\Delta I^k_X),
$$
we need exponentially (in \\(K\\) )  less epochs for training a deeper network to compress the 
input equally, (assuming that \\(\Delta I^k_X\\) are similar). At the same time, the number of computations 
needed for training the network grows linearly with the number of layers, so this 
exponentiall boost remains significant.

# Criticism

* See <a href="https://openreview.net/forum?id=ry_WPG-A-&noteId=ry_WPG-A-">this discussion</a> 

# Open Questions

* Can findings be reproduced with other rules and network architectures?

* Do the findings scale up to larger networks and real world problems?

* Can similar compression be observed in other models than DNN?

* What determines the \\(\beta\\) of the limit points of the hidden layers?

* Can the arguments in this paper, e.g. for SGD dynamics, be made more rigorous?

* What are the practical implications of the findings? [see Variational Information Bottleneck Paper]

* What happens in Bayesian Neural Networks or Random Forests?
