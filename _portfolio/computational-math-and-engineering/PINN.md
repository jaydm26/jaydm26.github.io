---
layout: post
title: Physics Informed Neural Networks
date: 22-08-2020 23:55:00
type: post
categories: Computational Math and Engineering
tags: ML, AI, deep learning, fluid mechanics, neural networks
permalink: "/portfolio/computational-math-and-engineering/PINN"
---

Physics Informed Neural Networks (PINNs) are the latest attempt in merging two of my favourite communities: machine learning and partial differential equations. PINNs are really useful at solving PDEs at remarkable speed and accuracy based of very little learning data.

The remarkable results are as a result of using the data in a wise manner: the data is used to reduce the loss in prediction of the variable, and further to reduce the residual losses from the PDE. This double use of data allows this network to predict shocks from the Viscous Burgers Equation with great accuracy. It can also predict field variables a substantial time into the future without performing intermediate step calculations by the use a huge RK method jump [Maziar et. al. used more than 50](https://arxiv.org/abs/1711.10561)(https://arxiv.org/abs/1711.10566). This circumvents the CFL condition, and makes simulating the future incredibly fast.

Additional advantages of the PINNs are its ability to predict parameters of a PDE with incredible accuracy too. Moreover, the framework of the PINN allows for calculating additional quantities like gradients and fluxes with relative ease (gradients are freely available since they are coded using Automatic Differentiation).

However, PINNs are not the panacea to all problems. For example, the Lorenz attractor is a problem that does not seem to have a tractable solution using a feed forward deep neural network. Here, the losses are informed by the error in predicting the state, as well as ensuring the coupled ODEs are satisfied. Here, I would like to qualify this conclusion by saying that the work is not completely exhaustive in nature (and probably cannot be for all practical purposes). Moreover, the [Universal Approximation Theorem](https://en.wikipedia.org/wiki/Universal_approximation_theorem) implies that any function can be approximated by the first layer can approximate any function. Further, from [Zhou et. al](http://papers.nips.cc/paper/7203-the-expressive-power-of-neural-networks-a-view-from-the-width.pdf), we observe that width-bound, feed-forward neural networks are universal approximators. Thus, while a simple Physics Informed Neural Network (or in this case, Dynamical System Informed Neural Network) cannot approximate the Lorenz attractor, a more involved approach may be able to do so (like adding previous time-steps prediction as input to the network, using CNNs, using RNNs, etc.).

My work on it is available [here](https://github.com/jaydm26/PINNs/blob/master/Lorenz%20Attractor/Lorenz_Attractor.ipynb). I am open to knowing what I can do to improve the network, and other tricks I could try to make this work.
