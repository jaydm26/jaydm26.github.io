---
layout: post
title: Uncertainty Quantification
date: 08-03-2020 23:00:00
type: post
categories: Computational Math and Engineering
tags: Stats, Uncertainty Quantification, Optimization
permalink: "/portfolio/computational-math-and-engineering/UQ"
---

In a [previous post]({{ site.baseurl }}/portfolio/insightful-posts/the-probabilistic-nature-of-our-world/), I had posted about how our deterministic world is filled with probabilities. This notion kept bugging me for a long time. So I decided to find some answers. While exploring this topic, I came across a paper by [Zachary del Rosario](http://www.zdelrosario.com), a Ph.D student at Stanford University, who written fantastic papers on Reliability Based Design Optimization (RBDO).

RBDO is used to develop reliable designs while minimizing an objective function (which could be a function of cost among other design variables) when the inputs to the design are uncertain in nature. This idea is especially useful for design in aircraft parts (as noted in [del Rosario et. al.](https://onlinelibrary.wiley.com/doi/abs/10.1002/nme.6035)).

Mathematically, RBDO can be stated as:

\\[ min \\: C(t),\\]
\\[s.t. \\mathbb{P}_{\\textbf{X}}[g(t,\\textbf{X}) > 0] > \\mathcal{R} \\]

where $latex g(t,\\textbf{X})$ is the limit-state function. If $latex g \\geq 0$, the design is considered safe.

To put the above in simple terms, we are trying to minimize $latex C(t)$ such that the probability that the limit-state function is greater than 0 (i.e. the design is safe) is greater than the desired reliability $latex \\mathcal{R}$. Here $latex \\textbf{X}$ contains the uncertain input variables.

The most convenient and intuitive way to solve the above equation is to use Monte Carlo simulations to ensure that the condition is satisfied while another loop minimizes the objective function. This "double loop" strategy, however, computationally intense. There are other techniques that can solve this problem without going into the double loop, but I will leave that aside right now.

What I want to draw your attention is to results from the method and how they could define the process of designing in the future. In the following [notebook](https://github.com/jaydm26/UQ/blob/master/Heat%20Sink.ipynb), I have gone through an example for heat transfer. While the example is crude, it goes on to show how we can create reliable designs when the inputs are uncertain. I have used the "grama" module, developed by Zachary, for coding the problem.

In a subsequent post, I will describe a more involved approach with UQ.

P.S. Shout out to Zachary for not only patiently guiding me, but also entertaining my raft of questions. Thank you Zachary!
