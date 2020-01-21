---
layout: post
title: Bessel Functions
date: 2020-01-19 13:00:00.000000000 +05:30
type: portfolio
categories: Insightful Posts
tags: pde, bessel, sturm-liouville
permalink: "/portfolio/insightful-posts/bessel-functions/"
---

In a [previous post]({{site.baseurl}}"/portfolio/insightful-posts/solving-2-d-diffusion-using-strum-liouville-equations/"), the discussion revolved around using the idea of Sturm-Liouville equations to solve two dimensional heat equations in cartesian coordinates. This idea is in fact applicable to problems in the cylindrical and spherical system too. Today, let us discuss the Bessel Functions, a type of function that we encounter when solving for the laplace equation in the cylindrical coordinate system.

Mathematically speaking, the Bessel Functions are a set of functions that are solutions to the Bessel's differential equation given by:

\\[x^2 \\dfrac{d^2y}{dx^2} + x\\dfrac{dy}{dx} + (x^2 - \\alpha^2)y = 0\\]

For our purposes, the value of \\(\\alpha\\) is always an integer or a half integer. This results in the solution for cylindrical systems.

Bessel functions are of two kinds, and quite unimaginatively named Bessel Function of the first kind (denoted by \\(J_\\alpha(x))\\), and Bessel Function of the second kind (denoted by \\(Y_\\alpha(x))\\).

The most general form of the solution to the Bessel's differential equation is given by:

\\[y(x) = C_1 J_\\alpha(x) + C_2 Y_\\alpha(x)\\]

where \\(J_\\alpha(x)\\) is the Bessel Function of the first kind of order \\(\\alpha\\), and \\(Y_\\alpha(x)\\) is the Bessel Function of the second kind of order \\(\\alpha\\).

Using boundary conditions, the values of \\(C_1\\) and \\(C_2\\) can be obtained.

Like any other Sturm-Liouville equation, Bessel functions enjoy the fact that their eigenfunctions are orthogonal when multiplied by a weighting function \\(w(x)\\). For Bessel Functions, the weighting function \\(w(x)\\) is just \\(x\\). Thus,

\\[\\int_{x=0}^{x=R} x J_\\alpha(\\lambda_n x) J_\\alpha(\\lambda_m x) dx = 0\\]
if \\(m \\neq n\\). If \\(m = n\\), we obtain the norm given by:

\\[N(\\lambda_n) = \\int_{x=0}^{x=R} x J_\\alpha^2(\\lambda_n x) dx = \\dfrac{R^2}{2}[J_{\\alpha+1}(\\lambda_n R)]^2\\]

Some other important properties of the Bessel Functions are:

\\[J_0(0) = 1\\]
\\[J_\\alpha(0) = 0\\]
\\[Y_\\alpha(x\\rightarrow 0) \\rightarrow -\\infty \\;\\;for \\;\\alpha \\, \\geq 0\\]

The differential properties of Bessel Functions are:

\\[\\dfrac{d}{dx}J_0(x) = -J_1(x)\\]
\\[\\dfrac{d}{dx}Y_0(x) = -Y_1(x)\\]
\\[\\dfrac{d}{dx}J_n(x) = \\dfrac{1}{2}[J_{n-1}(x) - J_{n+1}(x)]\\]
\\[\\dfrac{d}{dx}Y_n(x) = \\dfrac{1}{2}[Y_{n-1}(x) - Y_{n+1}(x)]\\]

Furthermore, there exist another set of Bessel Functions for the differential equation:

\\[x^2 \\dfrac{d^2y}{dx^2} + x\\dfrac{dy}{dx} - (x^2 + \\alpha^2)y = 0\\]

The most general to this differential equation is given by:

\\[y(x) = C_1 I_\\alpha(x) + C_2 K_\\alpha(x)\\]

where \\(I_\\alpha(x)\\) is the modified Bessel Function of the first kind of order \\(\\alpha\\), and \\(K_\\alpha(x)\\) is the modified Bessel Function of the second kind of order \\(\\alpha\\).

Some properties of the modified Bessel Functions are:

\\[I_0(0) = 1\\]
\\[I_\\alpha(0) = 0\\]
\\[K_\\alpha(x\\rightarrow 0) \\rightarrow +\\infty\\ \\;\\;for \\;\\alpha \\, \\geq 0\\]

The differential properties of the modified Bessel Functions are:

\\[\\dfrac{d}{dx}I_0(x) = I_1(x)\\]
\\[\\dfrac{d}{dx}K_0(x) = -K_1(x)\\]
\\[\\dfrac{d}{dx}I_n(x) = \\dfrac{1}{2}[I_{n-1}(x) + I_{n+1}(x)]\\]
\\[\\dfrac{d}{dx}K_n(x) = -\\dfrac{1}{2}[K_{n-1}(x) + K_{n+1}(x)]\\]

Bessel functions allow us to solve the Laplace equation in cylindrical coordinates. This is vital for analytically solving a large number of engineering problems.
