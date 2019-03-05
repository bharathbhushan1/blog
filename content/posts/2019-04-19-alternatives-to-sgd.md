---
title: "Alternatives to SGD"
date: 2019-03-05T20:00:00
draft: false
tags: ["machine learning"]
---

![SGD](https://sebastianraschka.com/images/faq/closed-form-vs-gd/ball.png)

Mini-batch SGD has been the mainstay of algorithms to optimise the cost function in neural networks. But there are multiple alternatives.

* Hessian optimization
* Momentum based techniques
* BFGS and its variants
* Nesterov's accelerated gradient
* ...
<!--more-->

## Hessian optimization
Taylor's theorem allows a multi-variable function to be expressed as a series in terms of its partial derivatives. The second order term in this expansion is the Hessian matrix (basically a 2-d gradient). If we approximate the expansion using just first and second order derivatives and then try to optimise, we get a formula for the variable update which has the inverse Hessian. 

This weight update strategy converges in fewer steps than SGD and (because of second-order information) avoids pathologies of SGD. But the **Hessian * Gradient** computation is very expensive because of the size of the Hessian (square of number of variables). 

## Momentum based gradient descent
This is inspired by the Hessian approach of using the second order derivatives, but avoids the large matrices. Basically every weight has a corresponding velocity parameter. The learning algorithm only updates the velocity and the velocity is used to update the weight. Also the current velocity is tempered by a hyperparameter μ which is called the momentum coefficient and vaguely corresponds to friction in a physical system. When μ is 1, there is no friction but when the minima approaches, overshooting is possible. When μ is 0, it is just SGD, no velocity can build up. 

Also the code changes needed to incorporate momentum are minor. And using a μ of between [0,1] helps in fast learning but without overshooting.

## Appendix
This is a set of topics which are diversions from the main topic of this post but very interesting nonetheless.

### Banach spaces
Basically there is this sequence of algebraic concepts:

* Set
* Topological space (by adding open-set axioms)
* Metric space (by adding a distance metric between two members of the set)
* Banach space (by adding norm and completeness (every cauchy sequence's limit is also in the space))
* Hilbert space (by adding inner product)

## References

* This post is based on my reading of [Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/chap3.html#other_techniques) by Micheal Nielsen.
* The image at the top of the page is from this [blog post](https://sebastianraschka.com/faq/docs/closed-form-vs-gd.html) by Sebastian Raschka
