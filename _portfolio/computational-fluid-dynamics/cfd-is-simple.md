---
layout: post
title: CFD is SIMPLE
date: 22-11-2019 13:00:00
type: post
categories: Computational Fluid Dynamics
tags: CFD, SIMPLE, finite difference
#permalink:
---

Computational Fluid Dynamics (CFD) is fairly complicated. Boiled down to the basics, CFD is trying to solve the Navier-Stokes equations and satisfying continuity while making sure that initial and boundary conditions are satisfied.

From a previous [post]({{site.base_url}}portfolio/appreciating-the-fluid-equations/), I have mentioned how complicated Navier-Stokes equations are. In 3D, there is no direct solution to the Navier-Stokes equation without any simplifications. This is why CFD is essential to solving flow problems quickly on a computer without spending money on experiments.

The process of CFD is as follows:

1.  Discretization of the domain: This can be done in plenty of ways. Broadly, one can have a structured or an unstructured grid. The grid may or may not conform to the body present in the flow.
2.  Apply Finite Methods: Use Finite Difference, Finite Element or Finite Volume Methods to discretize the Navier-Stokes equations and continuity equations. If time is involved, one needs to also include a single-step (Runge-Kutta, Euler) or multi-step (Adams-Bashforth, Adams-Moulten) time-differencing scheme.
3.  Apply the Finite method to the discrete domain and form an AX = B problem. This step accounts for the boundary conditions.
4.  Solve the AX=B problem using an iterative method. The iterative method accounts for satisfying the continuity.

Each of the above topic can have its own separate post. As for this one, I will be talking about the most common iterative solver in CFD- the SIMPLE scheme.

SIMPLE is an abbreviation for the Semi-Implicit Method for Pressure Linked Equations. This method is the most common method implemented by CFD solvers like Fluent, Star-CCM+, etc. The method is explained in great detail in "An Introduction to Computational Fluid Dynamics- The Finite Volume Method" by H.K. Versteeg & W. Malalasekera and "Numerical Heat Transfer and Fluid Flow" by Suhas V. Patankar. So let us try to understand what this method is.

The discretized steady-state Naiver-Stokes equations applied to the staggered grid takes the following form:

X-direction: $latex a\_{i,J} u\_{i,J} = \\sum a\_{nb} u\_{nb} - (p\_{I,J} - p\_{I-1,J}) A\_{i,J} + b\_{i,J}$

Y-direction: $latex a\_{I,j} v\_{I,j} = \\sum a\_{nb} v\_{nb} - (p\_{I,J} - p\_{I,J-1}) A\_{I,j} + b\_{I,j}$

Continuity: $latex (\\rho u A)\_{I+1,J} - (\\rho u A)\_{I,J} + (\\rho v A)\_{I,J+1} - (\\rho v A)\_{I,J} = 0$

The above equations are true in 2D. Here $latex a$ refers to the coefficient formed for the velocity at each location (which is covered in the discretization), the subscript $latex nb$ means the neighboring cells, $latex b$ refers to the source term. All other variables carry their usual meanings.

The SIMPLE method in verbal form is as follows:

1.  Take a guessed pressure to obtain guessed velocities using the discretized momentum equation.
2.  Obtain a pressure correction field using the continuity equation.
3.  Correct pressure using a relaxation factor ($latex \\alpha\_p$).
4.  Correct velocities using corrected pressure.
5.  Repeat steps 2-4 until convergence.

In simple terms, this is nothing more than a prediction and a correction made to that prediction. In more mathematical terms, we have the following:

Let the guessed pressure field be given by $latex p^\*$. We can use this guessed pressure to obtain the x and y velocities as follows:

$latex a\_{i,J}u^\*\_{i,J} = \\sum a\_{nb}u^\*\_{nb} + (p^\*\_{I-1,J}-p^\*\_{I,J})A\_{i,J} + b\_{i,J}$

$latex a\_{I,j} v^\*\_{I,j} = \\sum a\_{nb} v^\*\_{nb} + (p^\*\_{I,J-1} - p^\*\_{I,J}) A\_{I,j} + b\_{I,j}$

The above can be recast as an AX = B problem where X represents the x or y direction velocities stacked in column, A represents the operator operating on the x or y direction velocities, and B represents the known values (pressure field and source).

In a 2D problem, we expect A to be highly sparse, thus making inverting difficult. Thus, a fast solver is required. Thus, we employ the Gauss-Seidel (GS) Method or Successive Over-Relaxation (SOR) Method to solve the AX = B problem to desired amount of residual.

Once we obtain the velocity fields, our task is to see if these velocity fields would satisfy the continuity equation. If this fails, our velocity fields are no good. From inspection, we cannot be certain that any predicted pressure field will lead to the satisfaction of the continuity equation through the predicted velocities. Thus, we develop corrections for pressure and velocity:

$latex p = p^\* + p' $  
$latex u = u^\* + u' $  
$latex v = v^\* + v' $

If we were to subtract the equation for guessed velocities from the discretized Navier-Stokes, we would have:

$latex a\_{i,J} (u\_{i,J} - u^\*\_{i,J}) = \\sum a\_{nb}(u\_{nb} - u^\*\_{nb}) + \\biggr\[(p\_{I-1,J} - p^\*\_{I-1,J}) - (p\_{I,J} - p^\*\_{I,J})\\biggr\] A\_{i,J}$

$latex a\_{I,j} (v\_{I,j} - v^\*\_{I,j}) = \\sum a\_{nb} (v\_{nb} - v^\*\_{nb}) + \\biggr\[(p\_{I,J-1} - p^\*\_{I,J-1}) - (p\_{I,J} - p^\*\_{I,J})\\biggr\] A\_{I,j}$

If we observe, we see the correction terms (variable with a prime) in the above equations.

$latex a\_{i,J}u'\_{i,J} = \\sum a\_{nb}u'\_{nb} + (p'\_{I-1,J}-p'\_{I,J})A\_{i,J}$

$latex a\_{I,j} v'\_{I,j} = \\sum a\_{nb} v'\_{nb} + (p'\_{I,J-1} - p'\_{I,J}) A\_{I,j}$

Now comes the most important approximation and the reason why SIMPLE works so effectively for most general case problems. In the above equations, we can assume $latex \\sum a\_{nb} u'\_{nb}$ and $latex \\sum a\_{nb} v'\_{nb}$ to be very small as compared to the other terms. Thus, we are able to drop them and simplify the equations as:

$latex a\_{i,J}u'\_{i,J} = (p'\_{I-1,J}-p'\_{I,J})A\_{i,J}$ or $latex u'\_{i,J} = (p'\_{I-1,J}-p'\_{I,J})d\_{i,J}$

$latex a\_{I,j} v'\_{I,j} = (p'\_{I,J-1} - p'\_{I,J}) A\_{I,j}$ or $latex v'\_{I,j} = (p'\_{I,J-1} - p'\_{I,J}) d\_{I,j}$

where $latex d\_{i,J} = \\dfrac{A\_{i,J}}{a\_{i,J}}$ and so on.

Since we now have correction for velocities, it is time to correct the pressure as well. This is done using the continuity equation.

\\[ [\\rho\_{i+1,J}A_{i+1,J}(u^\*\_{i+1,J} + d_{i+1,J}(p'\_{I,J}-p'\_{I+1,J})) - \\rho\_{i,J}A_{i,J}(u^\*\_{i,J} + d_{i,J}(p'\_{I-1,J}-p'\_{I,J}))] \\]
\\[ + [\\rho\_{I,j+1}A_{I,j+1}(v^\*\_{I,j+1} + d_{I,j+1}(p'\_{I,J}-p'\_{I,J+1})) - \\rho\_{I,j}A_{I,j}(v^\*\_{I,j} + d_{I,j}(p'\_{I,J-1}-p'\_{I,J}))] = 0\\]

The above equation can be re-arranged to form an $latex AX=B$ problem.

\\[a\_{I,J}p'\_{I,J} = a\_{I+1,J}p'\_{I+1,J} + a\_{I-1,J}p'\_{I-1,J} + a\_{I,J+1}p'\_{I,J+1} + a\_{I,J-1}p'\_{I,J-1} + b'\_{I,J}\\]

The above equation is called the pressure correction equation. It can be solved using a iterative method such as the Gauss-Seidel or the Successive Over-Relaxation (SOR) Method. Next, we update the pressure field. Using this updated pressure field, we find the new velocity corrections and update the velocity fields.

From experience it is observed that the pressure correction equation can diverge. Thus, we employ a under-relaxation factor to update the pressure and velocity field.

Thus, the actual update step is given by $latex p = p^\* + \\alpha\_p p' $. The velocity update takes place using a weighted average:

$latex u^{new} = \\alpha\_u u^\* + (1-\\alpha\_u) u' $

$latex v^{new} = \\alpha\_u v^\* + (1-\\alpha\_v) v' $

Some more manipulation can be done to incorporate this under-relation directly into the discretized momentum equations.

X-direction: $latex \\dfrac{a\_{i,J}}{\\alpha\_u} u\_{i,J} = \\sum a\_{nb} u\_{nb} - (p\_{I,J} - p\_{I-1,J}) A\_{i,J} + b\_{i,J} + \\biggr[(1-\\alpha\_u)\\dfrac{a\_{i,J}}{\\alpha\_u}\\biggr]u^{(n-1)}\_{i,J}$

Y-direction: $latex \\dfrac{a\_{I,j}}{\\alpha\_v} v\_{I,j} = \\sum a\_{nb} v\_{nb} - (p\_{I,J} - p\_{I,J-1}) A\_{I,j} + b\_{I,j} + \\biggr[(1-\\alpha\_v)\\dfrac{a\_{I,j}}{\\alpha\_v}\\biggr]v^{(n-1)}\_{I,j}$

Note here that $latex \\alpha\_u \\neq \\alpha\_v$ (but can be equal) and $latex \\alpha\_u, \\alpha\_v$ are not related to $latex \\alpha_p$.

The most eye-catching part of this method is its ability to solve a variety of flow problems in a robust manner. The robustness of the SIMPLE method is brought in because of the diagonal dominance of $latex A$ in $latex AX=B$.

The robustness of solver has made it an attractive choice for commercial CFD solvers. There are modifications to the SIMPLE method which make this method even faster and more accurate.

There are various other methods that are used to solve the fluid equations. This was one of the first commercially deployed methods in the field of CFD.
