---
layout: post
title: Design Under Uncertainty
date: 2020-01-19 21:00:00.000000000 +05:30
type: portfolio
categories: Insightful Posts
tags: design, uncertainty, probability, risk
permalink: "/portfolio/insightful-posts/design-under-uncertainty/"
---

For a vast number of engineering problems, the objective of the designer is to design a system based on given parameters. While this approach is sound for parameters known with great certainty, such is not the case for most design problems. Designing under uncertainty can be attributed to a variety of factors: uncertainty in inputs, errors in the analytical model, changing markets, etc. This makes the designing process highly complicated.

To overcome this problem, designers have developed numerous computational methods. These methods take into account the uncertainty they are trying to mitigate, and output the design parameters that reduce the risks of over- or under- designing. Two methods have become mature over the recent years- the "Robust Design" method and the "Reliability Based Design Optimization" (RBDO) method.

The Robust Design method, developed by Taguchi, is a method to reduce the uncertainty in uncontrollable variables by setting the controllable parameters. To understand this, let us consider a very simple engineering example: designing a car. As a developer, you try to reduce the uncertainties in the engine design, transmission design, exhaust system design, auxiliary systems design, etc. But once the car is sold to the consumer, the designer has limited to no control over how the consumer will use the car: they could be diligent in servicing the car, or never service it till the check engine light shows up. Thus, it is up to the designer to design the car in a robust manner so that the car will survive any use/absue subjected to it.

The Reliability Based Design Optimization goes a step further. This method attempts to reduce the probability of failure of a design under uncertain design conditions while ensuring that the design is optimum. To rephrase, RBDO essentially is a double loop algorithm: it tries to optimize the design, and it tries to maximize reliability (or minimize the probability of failure). Thus, RBDO is computationally intensive.

These methods form the foundation of the concepts related to design under uncertainty.
