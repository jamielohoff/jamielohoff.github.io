---
title: "Geometric View on Automatic Differentiation"
tags:
  - AD
use_math: true
comments: true
---

This article offers quite a different perspective on some of the principles behind algorithmic differentiation (AD). We look at AD through the lens of Differential Geometry, one of my favorite branches of mathematics. While I have not yet unearthed any fundamental consequences following from this viewpoint, I still think it is worth writing it down somewhere.

Differential Geometry is the branch of mathematics that studies distances, shapes and sized of objects on smooth spaces. The fundamental object in Differential Geometry is a **smooth manifold**.
Now what is a manifold and what does it mean when we say it is smooth?

### Smooth Manifolds
Most of the fundamental branches of mathematics define some kind of ambient object to set the scene. This is usually the first thing that gets constructed and checked for consistency in any reasonably well-designed book or lecture. It is typically a set equipped with some additional mathematical structure(s). Examples:
- Linear Algebra: Vector space where the set is something like the $\mathbb\{R}^n$ and the additional structure are the eight axioms that make vector addition and scalar multiplication possible
- Calculus of one variable: Metric space where the set is again something like the $\mathbb\{R^n}$ and the additional structure is a two-variable function $d(x,y)$ that satisfies positivity, symmetry and the triangle equation. This is needed so that we can define things like continuity, limits and derivatives.
- Probability theory: Probability space which consists of two sets, the sampling set and the outcome set. Furthermore, we have a function $P$ that assigns probabilities to the outcomes. The Kolmogorov axioms are the additional structure that makes everything work here.
- Topology: Topological space where the set can be any set and the additional structure is imposed through requirements onto the unions and intersections of subsets etc.

Now what is the set we study in Differential Geometry? The underlying set $M$ has to be a topological space, i.e. it has to support the basic operations such as unions and intersections. The next thing we need is a set of tuples $(U, \phi)$ called charts which form an atlas. $U$ is an open subset of $M$ called the coordinate neighborhood and $\phi$ is a homeomorphism (bijective, continous function) from $U$ into the $\mathbb\{R}^n$ called the coordinate function. The union of $U$ fully contains $M$. That's it, but what is it good for?

Differential Geometry was invented/discovered when mathematicians started to study spaces with intrinsic curvature like a sphere or a saddle. Tackling these turned out to be very hard especially if you want to define things like derivatives. However, we have a range of tools for flat spaces, i.e. $\mathbb{R}^n$, so maybe we can restrict ourselves to topological spaces that at least locally look like the $\mathbb{R}^n$? This is exactly the idea of atlas structure introduced above. Looking at the Earth as a sphere, the additional structure becomes manifest in the sense that an atlas of the Earth does the same thing as an atlas on a manifold. It contains a bunch of local representation of the Earth's surface in $\mathbb{R}^2$ (the map) and a set of coordinates that show where this map belongs globally (latitudes and longitudes) which is exactly what a chart is (actually a chart in a geographical atlas consists of $\phi(U)$ subset $\mathbb{R}^2$ and $\phi^{-1}$ from which we can then construct the mathematical chart)
Thus a smooth manifold is nothing else than a space that locally looks like the $\mathbb{R}^n$.
There is plenty of literature on these things so I will not dive further into the specifics of how to construct charts and atlasses for a given smooth manifold.

### Maps between Manifolds
The most interesting part of course is the study of maps between manifolds, i.e. f: M --> N where M,N are smooth manifolds.
In practive it is usually not possible to write down f directly and we fall back of formulating f using atlasses defined on M,N which $f = \phi^{-1} \circ f \circ \psi$.
But now a change in coordinates becomes almost trivial because we can just consider maps between different charts.
However, coordinate-free formulations are extremely helpful if one does not want to worry about things such as choosing a coordinate system and instead focus on the "gist" of what f does. This is particularly important in physics where laws should be independent of the choice of coordinates (a key insight that enabled Einstein to formulate his Theory of General Relativity). All laws of physics can be formulated in a coordinate-free, differential geometric version.

### Tangent Space and Cotangent Space
Before we move on to doing the fun stuff we have to introduce one more concept: the tangent space. It is the generatlization of the tangent in single-variable calculus.
We need it to define derivatives on manifolds. The construction of the tangent vector

Often we are interested in changes induced by f. Think of a neural network which maps the parameter manifold to the loss landscape manifold. Then we are interested in the derivatives of f. We can calculate these using charts because then we can simply apply multivariable calculus.
$h_\theta(x) = \theta_0 x + \theta_1 x$

### Literature