---
layout: post
title: The Fin Problem
date: 2019-09-10 15:02:19.000000000 +05:30
type: portfolio
categories: Insightful Posts
tags: heat transfer, conduction, math
permalink: "/portfolio/insighful-posts/the-fin-problem/"
---

I recently wrote about my research on fins and I think I should take some time to write more about the mathematical aspect of it.

In this post, I will write about the solution to a 2D fin. In the [previous post]({{ site.baseurl }}/portfolio/solving-2-d-diffusion-using-strum-liouville-equations/), I gave the solution to the 1D fin, so I won't be going over it again.

The Problem Statement
---------------------

The problem is described in the previous post. I will describe it here once again. A rectangular fin has the dimensions LxW. It is heated from the bottom with a constant heat flux of \\( q'\'\\). The other surfaces are losing heat to surrounding through via convection. The heat transfer coefficient \\( h\\) is assumed to be a constant. Surrounding air is at temperature \\( T\_{\\infty}\\).

|![]({{ site.baseurl }}/assets/2d-fin-1.jpeg)|

Solution:
---------

To solve this problem, first let us look at the boundary conditions on each of the domain boundaries. The bottom boundary is a non-homogenous boundary with constant heat flux. The left and right boundaries are identical, and are non-homogenous convective boundaries. The top boundary is a non-homogenous convective boundary.

From the Strum-Liouville post, we know that we can solve for one non-homogenous boundary. Now since we have 4 non-homogenous boundaries, the solution to the problem will be extremely difficult.

But once again, math provides us with some elegant solutions. In the above problem, the governing equation within the domain is given by:

\\[ \\dfrac{\\partial^2 T}{\\partial x^2} + \\dfrac{\\partial^2 T}{\\partial y^2} = 0\\]  

OR  

\\[ \\nabla^2 T = 0\\]

The Laplace equation is a second order linear differential equation. Due to its linear nature, we can superimpose solutions to sub-problems to obtain a global solution.

Before we break down our problem, let us make a small simplification. Since the problem is symmetric in the X-direction, we will only solve for the positive half. The negative half will be a mirror image of this solution. Thus, the updated boundary condition will be \\( \\dfrac{\\partial T}{\\partial x} \\bigr\\vert\_{x=0,y}= 0\\).

|![]({{ site.baseurl }}/assets/2d-fin-symmetric.jpeg)|

In the above diagram, for the full problem the boundary conditions are given by:

BC1: \\( -k\\dfrac{\\partial T}{\\partial y} \\bigr\\vert\_{x,y=0} = q'\'\\)

BC2: \\( -k\\dfrac{\\partial T}{\\partial x} \\bigr\\vert\_{x=L/2,y} = h(T(L/2,y) - T\_{\\infty})\\)

BC3: \\( -k\\dfrac{\\partial T}{\\partial y} \\bigr\\vert\_{x,y=W} = h(T(x,W)-T\_{\\infty})\\)

BC4: \\( \\dfrac{\\partial T}{\\partial x} \\bigr\\vert\_{x=0,y} = 0\\)

Now, let us break the problem down into 4 parts such that each time, one of the boundary condition is the non-homogenous one, and all other boundaries are homogeneous while maintaining the type of boundary. Thus, if one of the boundaries was a Dirichlet boundary, we would ensure that the homogeneous one would also be a Dirichlet boundary. Similarly for Neumann boundaries. In the case of mixed boundaries, we ensure that the non-homogeneous part is put to 0. E.g., in the above case, we would make \\( T\_{\\infty} = 0\\) to convert the boundary to a homogenous boundary.

### Part 1: BC1 is non-homogeneous

We have,

\\[ \\dfrac{\\partial^2 T\_1}{\\partial x^2} + \\dfrac{\\partial^2 T\_1}{\\partial y^2} = 0\\]

Subject to:

BC1: \\( -k\\dfrac{\\partial T\_1}{\\partial y} \\bigr\\vert\_{x,y=0} = q'\'\\)

BC2: \\( -k\\dfrac{\\partial T\_1}{\\partial x} \\bigr\\vert\_{x=L/2,y} = h(T\_1(L/2,y))\\)

BC3: \\( -k\\dfrac{\\partial T\_1}{\\partial y} \\bigr\\vert\_{x,y=W} = h(T\_1(x,W))\\)

BC4: \\( \\dfrac{\\partial T\_1}{\\partial x} \\bigr\\vert\_{x=0,y} = 0\\)

Let \\( T\_1(x,y) = X\_1(x).Y\_1(y)\\) as before. Thus,

\\[ \\dfrac{1}{Y\_1} . \\dfrac{\\partial^2 Y\_1}{\\partial y^2} = -\\dfrac{1}{X\_1} . \\dfrac{\\partial^2 X\_1}{\\partial x^2} = \\lambda^2\\]

\\[ \\therefore \\dfrac{\\partial^2 Y\_1}{\\partial y^2} - \\lambda^2 Y\_1 = 0\\] and  
\\[ \\dfrac{\\partial^2 X\_1}{\\partial x^2} + \\lambda^2 X\_1 = 0\\]

\\[ \\therefore X\_1 = C\_1\\cos(\\lambda x) + C\_2\\sin(\\lambda x)\\]  

\\[ \\therefore Y\_1 = C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y)\\]

\\[ \\therefore T\_1(x,y) = (C\_1\\cos(\\lambda x) + C\_2\\sin(\\lambda x))(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y))\\]

Now, let us satisfy the boundary conditions one by one. We will deal with the non-homogenous boundary at the end.

\\[ \\dfrac{\\partial T\_1}{\\partial x} = (-C\_1\\lambda\\sin(\\lambda x) + C\_2\\lambda\\cos(\\lambda x))(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y))\\]

\\[ \\dfrac{\\partial T\_1}{\\partial y} = (C\_1\\cos(\\lambda x) + C\_2\\sin(\\lambda x))(C\_3\\lambda\\sinh(\\lambda y) + C\_4\\lambda\\cosh(\\lambda y))\\]

Starting with BC4,

\\[ \\dfrac{\\partial T\_1}{\\partial x} \\bigr\\vert\_{x=0,y} = 0\\]

\\[ (C\_2\\lambda)(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y)) = 0 \\Rightarrow C\_2 = 0\\]

Now, resolving BC2,

\\[ -k\\dfrac{\\partial T\_1}{\\partial x} \\bigr\\vert\_{x=L/2,y} = h(T\_1(L/2,y))\\]

\\[ (-k)(-C\_1\\lambda\\sin(\\dfrac{\\lambda L}{2}))(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y)) = (h)(C\_1\\cos(\\dfrac{\\lambda L}{2}))(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y))\\]

\\[ \\therefore \\lambda \\tan(\\dfrac{\\lambda L}{2}) = \\dfrac{h}{k}\\]

Thus, \\( \\lambda\\) are the eigenvalues of the sub-problem given by the solution to the equation \\( \\lambda \\tan(\\dfrac{\\lambda L}{2}) = \\dfrac{h}{k}\\).

Next, let us focus on BC3.

\\[ -k\\dfrac{\\partial T\_1}{\\partial y} \\bigr\\vert\_{x,y=W} = h(T\_1(x,W))\\]

\\[ (-k)(C\_1\\cos(\\lambda x))(C\_3\\lambda\\sinh(\\lambda W) + C\_4\\lambda\\cosh(\\lambda W)) = h((C\_1\\cos(\\lambda x))(C\_3\\cosh(\\lambda W) + C\_4\\sinh(\\lambda W)))\\]

\\[ \\therefore (-k)(C\_3\\lambda\\sinh(\\lambda W) + C\_4\\lambda\\cosh(\\lambda W)) = h((C\_3\\cosh(\\lambda W) + C\_4\\sinh(\\lambda W)))\\]

\\[ C\_3 = C\_4 \\dfrac{(k\\lambda\\cosh(\\lambda W) + h\\sinh(\\lambda W))}{(k\\lambda\\sinh(\\lambda W) + h\\cosh(\\lambda W))}\\]

Now, we have all the tools to solve the non-homogenous boundary.

\\[ T\_1(x,y) = \\displaystyle \\sum\\limits\_{n=0}^{\\infty} C\_1C\_4(\\cos(\\lambda\_n x))( \\dfrac{(k\\lambda\_n\\cosh(\\lambda\_n W) + h\\sinh(\\lambda\_n W))}{(k\\lambda\_n\\sinh(\\lambda\_n W) + h\\cosh(\\lambda\_n W))}\\cosh(\\lambda\_n y) + \\sinh(\\lambda\_n y))\\]

\\[ -k\\dfrac{\\partial T\_1}{\\partial y} \\bigr\\vert\_{x,y=0} = q'\'\\]

\\[ (-k)(\\displaystyle\\sum\\limits\_{n=0}^{\\infty} C\_1C\_4\\lambda\_n(\\cos(\\lambda\_n x))( \\dfrac{(k\\lambda\_n\\cosh(\\lambda\_n W) + h\\sinh(\\lambda\_n W))}{(k\\lambda\_n\\sinh(\\lambda\_n W) + h\\cosh(\\lambda\_n W))}\\sinh(\\lambda\_n y) + \\cosh(\\lambda\_n y))) \\bigr\\vert\_{x,y=0} = q'\'\\]

\\[ \\displaystyle\\sum\\limits\_{n=0}^{\\infty} a\_n\\lambda\_n(\\cos(\\lambda\_n x)) = -\\dfrac{q'\'}{k}\\] where \\( a\_n = C\_1C\_4\\)

Now, multiply by \\( \\displaystyle \\int\_{x=0}^{x=L/2} \\cos(\\lambda\_m x)dx\\).

\\[ \\therefore \\displaystyle \\int\_{x=0}^{x=L/2} \\sum\\limits\_{n=0}^{\\infty} a\_n\\lambda\_n (\\cos(\\lambda\_m x))(\\cos(\\lambda\_n x))dx = -\\dfrac{q'\'}{k}\\int\_{x=0}^{x=L/2} \\cos(\\lambda\_m x)dx\\]

Invoking the orthogonality of the eigenfunctions,

\\[ \\displaystyle \\int\_{x=0}^{x=L/2} a\_m\\lambda\_m (\\cos^2(\\lambda\_m x))dx = -\\dfrac{q'\'}{k}\\int\_{x=0}^{x=L/2} \\cos(\\lambda\_m x)dx\\]

\\[ \\therefore a\_n = -\\dfrac{q'\'}{\\lambda\_n k} \\dfrac{\\int\_{x=0}^{x=L/2} \\cos(\\lambda\_n x) dx }{\\int\_{x=0}^{x=L/2} \\cos^2(\\lambda\_n x) dx}\\]

Thus,

\\[ T\_1(x,y) = \\displaystyle \\sum\\limits\_{n=0}^{\\infty} -\\dfrac{q'\'\\cos(\\lambda\_n x)}{\\lambda\_n k} \\dfrac{\\int\_{x=0}^{x=L/2} \\cos(\\lambda\_n x) dx }{\\int\_{x=0}^{x=L/2} \\cos^2(\\lambda\_n x) dx} \\Biggr( \\dfrac{(k\\lambda\_n\\cosh(\\lambda\_n W) + h\\sinh(\\lambda\_n W))}{(k\\lambda\_n\\sinh(\\lambda\_n W) + h\\cosh(\\lambda\_n W))}\\cosh(\\lambda\_n y) + \\sinh(\\lambda\_n y)\\Biggr)\\]

where \\( \\lambda\_n\\) are the roots of \\( \\lambda \\tan(\\dfrac{\\lambda L}{2}) = \\dfrac{h}{k}\\)

### Part 2: BC2 is non-homogeneous

We have,

\\[ \\dfrac{\\partial^2 T\_2}{\\partial x^2} + \\dfrac{\\partial^2 T\_2}{\\partial y^2} = 0\\]

Subject to:

BC1: \\( -k\\dfrac{\\partial T\_2}{\\partial y} \\bigr\\vert\_{x,y=0} = 0\\)

BC2: \\( -k\\dfrac{\\partial T\_2}{\\partial x} \\bigr\\vert\_{x=L/2,y} = h(T(L/2,y)-T\_{\\infty})\\)

BC3: \\( -k\\dfrac{\\partial T\_2}{\\partial y} \\bigr\\vert\_{x,y=W} = h(T(x,W))\\)

BC4: \\( \\dfrac{\\partial T\_2}{\\partial x} \\bigr\\vert\_{x=0,y} = 0\\)

Let \\( T\_2(x,y) = X\_2(x).Y\_2(y)\\) as before. Thus,

\\[ \\dfrac{1}{Y\_2} . \\dfrac{\\partial^2 Y\_2}{\\partial y^2} = -\\dfrac{1}{X\_2} . \\dfrac{\\partial^2 X\_2}{\\partial x^2} = -\\lambda^2\\]

Note here that we changed the sign of \\( \\lambda\\) because we have homogenous boundaries in the Y-direction.

\\[ \\therefore \\dfrac{\\partial^2 Y\_2}{\\partial y^2} + \\lambda^2 Y\_2 = 0\\]
and  

\\[ \\dfrac{\\partial^2 X\_2}{\\partial x^2} - \\lambda^2 X\_2 = 0\\]

\\[ \\therefore X\_2 = C\_1\\cosh(\\lambda x) + C\_2\\sinh(\\lambda x)\\]  

\\[ \\therefore Y\_2 = C\_3\\cos(\\lambda y) + C\_4\\sin(\\lambda y)\\]

\\[ \\therefore T\_2(x,y) = (C\_1\\cosh(\\lambda x) + C\_2\\sinh(\\lambda x))(C\_3\\cos(\\lambda y) + C\_4\\sin(\\lambda y))\\]

Now, let us satisfy the boundary conditions one by one. We will deal with the non-homogenous boundary at the end.

\\[ \\dfrac{\\partial T\_2}{\\partial x} = (C\_1\\lambda\\sinh(\\lambda x) + C\_2\\lambda\\cosh(\\lambda x))(C\_3\\cos(\\lambda y) + C\_4\\sin(\\lambda y))\\]

\\[ \\dfrac{\\partial T\_2}{\\partial y} = (C\_1\\cosh(\\lambda x) + C\_2\\sinh(\\lambda x))(-C\_3\\lambda\\sin(\\lambda y) + C\_4\\lambda\\cos(\\lambda y))\\]

Starting with BC1,

\\[ -k\\dfrac{\\partial T\_2}{\\partial y} \\bigr\\vert\_{x,y=0} = 0\\]

\\[ (C\_1\\cosh(\\lambda x) + C\_2\\sinh(\\lambda x))(C\_4\\lambda) = 0 \\Rightarrow C\_4 = 0\\]

Now looking at BC3,

\\[ -k\\dfrac{\\partial T\_2}{\\partial y} \\bigr\\vert\_{x,y=W} = h(T(x,W))\\]

\\[ (-k)(C\_1\\cosh(\\lambda x) + C\_2\\sinh(\\lambda x))(-C\_3\\lambda\\sin(\\lambda W)) = h((C\_1\\cosh(\\lambda x) + C\_2\\sinh(\\lambda x))(C\_3\\cos(\\lambda W))\\]

\\[ \\therefore \\lambda\\tan(\\lambda W) = \\dfrac{h}{k}\\]

Now, turning our attention to BC4,

\\[ \\dfrac{\\partial T\_2}{\\partial x} \\bigr\\vert\_{x=0,y} = 0\\]

\\[ (C\_2\\lambda)(C\_3\\cos(\\lambda y) + C\_4\\sin(\\lambda y)) = 0 \\Rightarrow C\_2 = 0\\]

Finally, we have,

\\[ T\_2(x,y) = \\displaystyle \\sum\_{n=0}^{\\infty}C\_1C\_3\\cosh(\\lambda\_n x)\\cos(\\lambda\_n y)\\]

Now, using BC2,

\\[ -k\\dfrac{\\partial T\_2}{\\partial x} \\bigr\\vert\_{x=L/2,y} = h(T(L/2,y)-T\_{\\infty})\\]

\\[ \\displaystyle \\sum\_{n=0}^{\\infty} (-k)(C\_1\\lambda\\sinh(\\lambda\_n \\dfrac{L}{2}))(C\_3\\cos(\\lambda\_n y)) = h(\\displaystyle \\sum\_{n=0}^{\\infty}(C\_1\\cosh(\\lambda\_n \\dfrac{L}{2}))(C\_3\\cos(\\lambda\_n y)) - T\_{\\infty})\\]

\\[ \\displaystyle \\sum\_{n=0}^{\\infty} a\_n(h\\cosh(\\lambda\_n \\dfrac{L}{2}) + k\\lambda\_n \\sinh(\\lambda\_n \\dfrac{L}{2})) \\cos(\\lambda\_n y) = hT\_{\\infty}\\] where \\( a\_n = C\_1C\_3\\)

Multplying by \\( \\displaystyle \\int\_{y=0}^{y=W} \\cos(\\lambda\_m y)dy\\),

\\[ \\displaystyle \\int\_{y=0}^{y=W} \\displaystyle \\sum\_{n=0}^{\\infty} a\_n(h\\cosh(\\lambda\_n \\dfrac{L}{2}) + k\\lambda\_n \\sinh(\\lambda\_n \\dfrac{L}{2})) \\cos(\\lambda\_m y)\\cos(\\lambda\_n y)dy = hT\_{\\infty} \\displaystyle \\int\_{y=0}^{y=W} \\cos(\\lambda\_m y)dy\\]

Invoking orthogonality of eigenfunctions,

\\[ a\_n(h\\cosh(\\lambda\_n \\dfrac{L}{2}) + k\\lambda\_n \\sinh(\\lambda\_n \\dfrac{L}{2})) \\displaystyle \\int\_{y=0}^{y=W} \\cos^2(\\lambda\_m y)dy = hT\_{\\infty} \\displaystyle \\int\_{y=0}^{y=W} \\cos(\\lambda\_m y)dy\\]

\\[ a\_n = \\dfrac{hT\_{\\infty}}{(h\\cosh(\\lambda\_n \\dfrac{L}{2}) + k\\lambda\_n \\sinh(\\lambda\_n \\dfrac{L}{2}))} \\dfrac{\\int\_{y=0}^{y=W} \\cos(\\lambda\_n y)dy}{\\int\_{y=0}^{y=W} \\cos^2(\\lambda\_n y)dy}\\]

\\[ \\therefore T\_2(x,y) = \\displaystyle \\sum\_{n=0}^{\\infty} \\dfrac{hT\_{\\infty}}{\\Biggr(h\\cosh(\\lambda\_n \\dfrac{L}{2}) + k\\lambda\_n \\sinh(\\lambda\_n \\dfrac{L}{2})\\Biggr)} \\dfrac{\\int\_{y=0}^{y=W} \\cos(\\lambda\_n y)dy}{\\int\_{y=0}^{y=W} \\cos^2(\\lambda\_n y)dy} \\cosh(\\lambda\_n x)\\cos(\\lambda\_n y)\\]

where \\( \\lambda\_n\\) are the roots of \\( \\lambda\\tan(\\lambda W) = \\dfrac{h}{k}\\)

### Part 3: BC3 is non-homogeneous

We have,

\\[ \\dfrac{\\partial^2 T\_3}{\\partial x^2} + \\dfrac{\\partial^2 T\_3}{\\partial y^2} = 0\\]

Subject to:

BC1: \\( -k\\dfrac{\\partial T\_3}{\\partial y} \\bigr\\vert\_{x,y=0} = 0\\)

BC2: \\( -k\\dfrac{\\partial T\_3}{\\partial x} \\bigr\\vert\_{x=L/2,y} = h(T\_3(L/2,y))\\)

BC3: \\( -k\\dfrac{\\partial T\_3}{\\partial y} \\bigr\\vert\_{x,y=W} = h(T\_3(x,W) - T\_{\\infty})\\)

BC4: \\( \\dfrac{\\partial T\_3}{\\partial x} \\bigr\\vert\_{x=0,y} = 0\\)

Let \\( T\_3(x,y) = X\_3(x).Y\_3(y)\\) as before. Thus,

\\[ \\dfrac{1}{Y\_3} . \\dfrac{\\partial^2 Y\_3}{\\partial y^2} = -\\dfrac{1}{X\_3} . \\dfrac{\\partial^2 X\_3}{\\partial x^2} = \\lambda^2\\]

\\[ \\therefore \\dfrac{\\partial^2 Y\_3}{\\partial y^2} - \\lambda^2 Y\_3 = 0\\] and  
\\[ \\dfrac{\\partial^2 X\_3}{\\partial x^2} + \\lambda^2 X\_3 = 0\\]

\\[ \\therefore X\_3 = C\_1\\cos(\\lambda x) + C\_2\\sin(\\lambda x)\\]  

\\[ \\therefore Y\_3 = C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y)\\]

\\[ \\therefore T\_3(x,y) = (C\_1\\cos(\\lambda x) + C\_2\\sin(\\lambda x))(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y))\\]

Now, let us satisfy the boundary conditions one by one. We will deal with the non-homogenous boundary at the end.

\\[ \\dfrac{\\partial T\_3}{\\partial x} = (-C\_1\\lambda\\sin(\\lambda x) + C\_2\\lambda\\cos(\\lambda x))(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y))\\]

\\[ \\dfrac{\\partial T\_3}{\\partial y} = (C\_1\\cos(\\lambda x) + C\_2\\sin(\\lambda x))(C\_3\\lambda\\sinh(\\lambda y) + C\_4\\lambda\\cosh(\\lambda y))\\]

Starting with BC4,

\\[ \\dfrac{\\partial T\_3}{\\partial x} \\bigr\\vert\_{x=0,y} = 0\\]

\\[ (C\_2\\lambda)(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y)) = 0 \\Rightarrow C\_2 = 0\\]

Now, resolving BC2,

\\[ -k\\dfrac{\\partial T\_3}{\\partial x} \\bigr\\vert\_{x=L/2,y} = h(T\_3(L/2,y))\\]

\\[ (-k)(-C\_1\\lambda\\sin(\\dfrac{\\lambda L}{2}))(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y)) = (h)(C\_1\\cos(\\dfrac{\\lambda L}{2}))(C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y))\\]

\\[ \\therefore \\lambda \\tan(\\dfrac{\\lambda L}{2}) = \\dfrac{h}{k}\\]

Thus, \\( \\lambda\\) are the eigenvalues of the sub-problem given by the solution to the equation \\( \\lambda \\tan(\\dfrac{\\lambda L}{2}) = \\dfrac{h}{k}\\).

Now, looking at BC1,

\\[ -k\\dfrac{\\partial T\_3}{\\partial y} \\bigr\\vert\_{x,y=0} = 0\\]

\\[ (C\_1\\cos(\\lambda x))(C\_4\\lambda) = 0 \\Rightarrow C\_4 = 0\\]

Thus,

\\[ T\_3(x,y) = \\displaystyle \\sum\_{n=0}^{\\infty} C\_1C\_3 \\cos(\\lambda\_n x) \\cosh(\\lambda\_n y)\\]

Now, turning our attention to BC3.

\\[ -k\\dfrac{\\partial T\_3}{\\partial y} \\bigr\\vert\_{x,y=W} = h(T\_3(x,W) - T\_{\\infty})\\]

\\[ (-k) \\displaystyle \\sum\_{n=0}^{\\infty} C\_1C\_3\\lambda\_n \\cos(\\lambda\_n x) \\sinh(\\lambda\_n W) = h\\Biggr\[\\Bigr(\\displaystyle \\sum\_{n=0}^{\\infty} C\_1C\_3 \\cos(\\lambda\_n x) \\cosh(\\lambda\_n W)\\Bigr)-T\_{\\infty}\\Biggr\]\\]

\\[ \\displaystyle \\sum\_{n=0}^{\\infty} a\_n\\cos(\\lambda\_n x) \\Biggr( h\\cosh(\\lambda\_n W) + k\\lambda\_n \\sinh(\\lambda\_n W)\\Biggr) = hT\_{\\infty}\\] where \\( a\_n = C\_1C\_3\\)

Multiply by \\( \\displaystyle \\int\_{x=0}^{x=L/2} \\cos(\\lambda\_m x) dx\\),

\\[ \\displaystyle \\int\_{x=0}^{x=L/2} \\displaystyle \\sum\_{n=0}^{\\infty} a\_n \\Biggr( h\\cosh(\\lambda\_n W) + k\\lambda\_n \\sinh(\\lambda\_n W)\\Biggr) \\cos(\\lambda\_m x)\\cos(\\lambda\_n x)dx = hT\_{\\infty} \\displaystyle \\int\_{x=0}^{x=L/2} \\cos(\\lambda\_m x)dx\\]

Invoking orthogonality of eigenfunctions,

\\[ \\displaystyle \\int\_{x=0}^{x=L/2} a\_m \\Biggr( h\\cosh(\\lambda\_m W) + k\\lambda\_m \\sinh(\\lambda\_m W)\\Biggr) \\cos^2(\\lambda\_m x)dx = hT\_{\\infty} \\displaystyle \\int\_{x=0}^{x=L/2} \\cos(\\lambda\_m x)dx\\]

\\[ \\therefore a\_n = \\dfrac{hT\_{\\infty}}{\\Biggr( h\\cosh(\\lambda\_n W) + k\\lambda\_n \\sinh(\\lambda\_n W)\\Biggr)} \\dfrac{\\int\_{x=0}^{x=L/2} \\cos(\\lambda\_n x)dx}{ \\int\_{x=0}^{x=L/2} \\cos^2(\\lambda\_n x)dx }\\]

Thus,

\\[ T\_3(x,y) = \\displaystyle \\sum\_{n=0}^{\\infty} \\dfrac{hT\_{\\infty}}{\\Biggr( h\\cosh(\\lambda\_n W) + k\\lambda\_n \\sinh(\\lambda\_n W)\\Biggr)} \\dfrac{\\int\_{x=0}^{x=L/2} \\cos(\\lambda\_n x)dx}{ \\int\_{x=0}^{x=L/2} \\cos^2(\\lambda\_n x)dx } \\cos(\\lambda\_n x) \\cosh(\\lambda\_n y)\\]

where \\( \\lambda\_n\\) are the roots of \\( \\lambda \\tan(\\dfrac{\\lambda L}{2}) = \\dfrac{h}{k}\\)

### Part 4: BC4 is non-homogeneous

Because we used the symmetricity available in the problem, we managed to convert one of the boundaries into a homogeneous boundary. Thus, we do not have to solve for it separately.

### Summing it all up

The global solution to the above problem is given by:

\\[ T(x,y) = T\_1(x,y) + T\_2(x,y) + T\_3(x,y)\\]

For the sake of not writing an ugly six line equation, I am skipping over writing the final equation. Note here:

*   For \\( T\_1(x,y)\\) and \\( T\_3(x,y)\\), \\( \\lambda\_n\\) is given by the roots of \\( \\lambda \\tan(\\dfrac{\\lambda L}{2}) = \\dfrac{h}{k}\\)
*   For \\( T\_2(x,y)\\), \\( \\lambda\_n\\) is given by the roots of \\( \\lambda\\tan(\\lambda W) = \\dfrac{h}{k}\\).

If one were to vary \\( h\\) for the top surface, that would be accounted for by changing \\( h\\) to \\( h\_3\\).

Lastly, we have to take note that the domain of the problem we just solved is \\( x\\in(0,L)\\) and \\( y\\in(0,W)\\). To expand our solution to the other half as well, we have to account for the symmetricity of the problem.

Since the graph is symmetric about the y-axis, the solution should be same regardless of the sign of the x-coordinate. This gives us an insight into how the solution might look.

In the \\( T\_1, T\_2\\), and \\( T\_3\\), replacing \\( x\\) by \\( -x\\) does not change the equations as \\( \\cos(-x) = \\cos(x)\\) and \\( \\cosh(-x) = \\cosh(x)\\). Thus, the equations hold good for the full domain of the initial problem.

#### Side Note:

I solved this problem in the long winded sense to show the plethora of tools we have at our disposal to solve what seems to be a rather complicated problem. A faster, more direct solution is available by performing a swift variable transformation.

If we let \\( \\theta(x,y)= T(x,y) - T\_{\\infty}\\), the new problem statement is given by:

\\[ \\dfrac{\\partial^2 \\theta}{\\partial x^2} + \\dfrac{\\partial^2 \\theta}{\\partial y^2} = 0\\]

with boundary conditions:

BC1: \\( -k\\dfrac{\\partial \\theta}{\\partial y} \\bigr\\vert\_{x,y=0} = q'\'\\)

BC2: \\( -k\\dfrac{\\partial \\theta}{\\partial x} \\bigr\\vert\_{x=L/2,y} = h(\\theta(L/2,y))\\)

BC3: \\( -k\\dfrac{\\partial \\theta}{\\partial y} \\bigr\\vert\_{x,y=W} = h(\\theta(x,W))\\)

BC4: \\( \\dfrac{\\partial \\theta}{\\partial x} \\bigr\\vert\_{x=0,y} = 0\\)

The above is a standard 2D diffusion problem with one non-homogeneous boundary condition (BC1). We can now quickly use separation of variables to solve for \\( \\theta\\).

Let \\( \\theta(x,y) = X(x).Y(y)\\).

\\[ \\therefore \\dfrac{1}{Y}\\dfrac{\\partial^2 Y}{\\partial y^2} = -\\dfrac{1}{X}\\dfrac{\\partial^2 X}{\\partial x^2} = \\lambda^2\\]

\\[ \\therefore X(x) = C\_1\\cos(\\lambda x) + C\_2\\sin(\\lambda x)\\] and  
\\[ Y(y) = C\_3\\cosh(\\lambda y) + C\_4\\sinh(\\lambda y)\\]

We apply the same procedures as we did in Part 1 to get,

\\[ \\theta(x,y) = \\displaystyle \\sum\\limits\_{n=0}^{\\infty} -\\dfrac{q'\'\\cos(\\lambda\_n x)}{\\lambda\_n k} \\dfrac{\\int\_{x=0}^{x=L/2} \\cos(\\lambda\_n x) dx }{\\int\_{x=0}^{x=L/2} \\cos^2(\\lambda\_n x) dx} \\Biggr( \\dfrac{(k\\lambda\_n\\cosh(\\lambda\_n W) + h\\sinh(\\lambda\_n W))}{(k\\lambda\_n\\sinh(\\lambda\_n W) + h\\cosh(\\lambda\_n W))}\\cosh(\\lambda\_n y) + \\sinh(\\lambda\_n y)\\Biggr)\\]

where \\( \\lambda\_n\\) are the roots of \\( \\lambda \\tan(\\dfrac{\\lambda L}{2}) = \\dfrac{h}{k}\\)

Of course, once again we would have to ensure that the solution is applicable for the full domain and once again, we can note that \\( \\cos(-x) = \\cos(x)\\). Thus, the solution is valid for the full domain.
