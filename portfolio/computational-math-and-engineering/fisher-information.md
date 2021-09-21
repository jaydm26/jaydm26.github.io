---
layout: post
title: Fisher Information
date: 17-09-2021 22:00:00
type: post
categories: Computational Math and Engineering
tags: statistics, regression, fisher information
permalink: "/portfolio/computational-math-and-engineering/fisher-information"
---

Fisher Information is a way to measure the amount of information that an observed random variable X brings in about an unknown parameter $latex \\theta$.

One may ask why is this important. To them, I suggest to follow along this sequence to understand why it is important to know about Fisher Information.

### Problem 1:

Consider a simple linear regression problem:

\\[ Y = X^TW \\]

The above problem is trivial to solve if $latex X^T$ is invertible. If $latex X^T$ is not invertible, it is possible that we have an underspecified or an overspecified problem. While underspecified system of equations will not have a unique solution, overspecified systems can have a solution that minimizes a certain residual. This can done using a pseudoinverse using the Moore-Penrose Inverse (which solves the least-square problem).

But now, consider a linear regression with stochastic noise:

\\[ Y = X^T W + \\eta \\]

where $latex \\eta \\sim \\mathcal{N}(0,\\sigma)$. Now, with the presence of stochastic noise, it may not be possible to obtain the "correct" solution as there is noise present in the system. However, once again, one can obtain the most likely result by taking expected value on both sides. This returns the standard linear regression model described above. But, in this case, since there is noise, we can only safely provide a maximum likelihood esimate for $latex W$. But what if we desire to obtain the distribution of $latex W$?

While an exact pdf of the distribution is not possible unless we are aware of the distributions for X and Y, we can still provide statistics about W (i.e. its MLE and variance). Fisher Information for the most general case is given by:

\\[ I(\\theta) = E\\left[ \\left( \\left\. \dfrac{\\partial}{\\partial \\theta} f(X;\\theta)\\right)^2 \\right \| \\theta \\right]\\int\_{\\mathbb{R}} \\left( \\dfrac{\\partial}{\\partial \\theta} \\ log \\ f(x;\\theta) \\right)^2 f(x;\\theta) dx \\]

Under certain conditions, and $latex f(x; \\theta)$ being twice differentiable, we have:

\\[ I(\\theta) = -E\\left[ \\left\. \\dfrac{\\partial^2}{\\partial \\theta^2} \\ log \\ f(X;\\theta) \\right\| \\theta \\right]\\]

Consider the scalar case:

\\[ y = x^Tw + \\eta \\]

Note that $latex x = x^T$ and $latex \\sigma = 1$. Mathematically, we can represent the Fisher Information as the negative expected value of the Hessian of the probability distribution of the random variable with respect to the parameter.

Since we know $latex \\eta$, we can argue that $latex y$ must be a Gaussian. Thus, the log-likelihood of $latex Y$ is given by:

\\[ l(\\mu, \\sigma) = \\dfrac{1}{2} \\ log(2\\pi\\sigma^2) - \\dfrac{(y - x^Tw)^2}{2\\sigma^2} \\]

Here, $latex \\mu = x^Tw$.

Taking the gradient of $latex l$ with respect to $latex w$,

\\[ S(w) = -\\dfrac{\\partial}{\\partial w} l(w) = \\dfrac{(y-x^Tw)x}{\\sigma^2} \\]

Taking the Hessian of $latex l$ with respect to $latex w$,

\\[ H(w) = -\\dfrac{\\partial^2}{\\partial w^2} l(w) = \\dfrac{-xx^T}{\\sigma^2} \\]

Taking the negative expected value of the Hessian, we obtain the Fisher Information for our linear regression model,

\\[ I(w) = -E \\left[ H(w) \\right] = - \\int\_{\\mathbb{R}} \\dfrac{-xx^T}{\\sigma^2} f(y; w) dy = \\dfrac{xx^T}{\\sigma^2} \\]

We can readily verify this information as we know that the variance for a scalar linear regression model is given by $latex \\dfrac{\\sigma^2}{xx^T}$.

We can further generalise this process for any model,

\\[ y = g(x; w) + \\eta \\]

Similar methodology is implemented to obtain the Hessian. The Hessian is given by:

\\[ H(w) = -\\dfrac{1}{\\sigma^2} \\left[ \\left( \\left. \\dfrac{\partial g}{\\partial w} \\right\|_{x} \\right)^2 + (y - g(x;w)) \\left( \\left. \\dfrac{\\partial^2 g}{ \\partial w^2} \\right\|_x \\right) \\right] \\]

The negative expected value of the above Hessian yields the Fisher Information.

\\[ I(w) = -E \\left[ H(w) \\right] = \\dfrac{1}{\\sigma^2}\\int*{\\mathbb{R}} \\left[ \\left( \\left. \\dfrac{\partial g}{\\partial w} \\right\|*{x} \\right)^2 + (y - g(x;w)) \\left( \\left. \\dfrac{\\partial^2 g}{ \\partial w^2} \\right\|\_x \\right) \\right] f(y;w)dy \\]

This results in

\\[ I(w) = \\dfrac{1}{\\sigma^2}E \\left[ \\left( \\left. \\dfrac{\partial g}{\\partial w} \\right\|_{x} \\right)^2 \\right] \\]

For multiple readings, Fisher Information is additive. Further, if $latex w$ is a vector (i.e. multi-parameter regression), additional care must be taken to derive the Hessian. The resulting Hessian is now a matrix. This leads to the formation of the Fisher Information Matrix (FIM).

We can use the Fisher Information (or Fisher Information Matrix) to provide us an idea about the variance of $latex w$. This can be shown using the Cramer-Rao Rule [refer to Wikipedia for more](https://en.wikipedia.org/wiki/Fisher_information). Thus,

\\[ Var(\\theta) \\geq \\dfrac{1}{I(\\theta)} \\]

In the case of multi-parameter estimation, we will obtain the covariance matrix. As one can already guess, with the covariance matrix, it is now possible to provide a confidence interval for the parameters, and propogate the uncertainty forwards in a model. This could also used in Gaussian Processes (if Y is distributed normally).

You can follow along with the steps described above in the notebook [here](https://github.com/jaydm26/Kalman-Filter/blob/master/Fisher%20Information.ipynb).
