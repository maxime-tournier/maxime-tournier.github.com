---
title: Convex Optimization
categories: [math]
---

{% include toc.md %}

# Convex Sets

Given a real vector space of finite dimensnion $$E$$, a subset $$C
\subseteq E$$ is *convex* when the line segment between any two points
remains in $$C$$. Formally, $$C$$ is convex if for all $$x, y \in C$$:

$$\forall \lambda \in [0, 1] : (1 - \lambda) x + \lambda y \in C$$

It is easy to see that the above is equivalent to $$C$$ being stable
by (finite) convex combinations:

$$\forall \block{x_i}_i \in C, \block{\lambda_i}_i \in \RR^+, \sum_i \lambda_i = 1: \sum_i \lambda_i x_i \in C$$

The space of convex weights $$\lambda \in \RR^{n+}, \mathbb{1}^T\lambda = 1$$ is usually called the $$n$$-dimensional *unit simplex*, denoted by $$\Delta^n$$.

# Convex Functions

Given a convex subset $$X \subseteq E$$, a function $$f: X \to \RR$$
is convex when its epigraph is convex:

$$\forall x, y \in X: f\block{\block{1 - \lambda} x + \lambda y} \leq (1 - \lambda) f(x) + \lambda f(y)$$

That is: the line segment between $$f(x)$$ and $$f(y)$$ remains
*above* the graph of $$f$$.

- TODO properties
  - above its tangent iff differentiable & convex
  - positive semidefinite hessian
- TODO examples
- TODO strong convexity?
- TODO minimization


# Cones

A subset $$\cone{K}$$ of a vector space $$E$$ is a *cone* if it is stable by
*positive* scalar multiplication:

$$\forall x \in \cone{K}, \lambda > 0\quad \lambda x \in \cone{K}$$

A cone is *pointed* when $$0 \in \cone{K}$$. A non-empty closed cone
is always pointed.  One generally deals with *closed* convex cones so
that the projection $$\pi_\cone{K}$$ onto $$\cone{K}$$ is
well-defined. A convex pointed cone $$K$$ induces a preorder defined
as:

$$x \leq_\cone{K} y \iff y - x \in \cone{K}$$

This preorder can be made into a partial order by requiring
$$\cone{K}$$ to be *flat*, that is stable by negation (*i.e.* it
contains lines).

## Dual Cone

When $$E$$ is an Euclidean space, the inner product
provides[^dual-cone] a generalization of the orthogonal complement for
cones, called the *dual cone*. Given a subset $$X \subseteq E$$, its
dual cone is defined as the set:

$$X^* = \left\{y \in E : \inner{y, x} \geq 0 \quad \forall x \in X\right\}$$

The dual cone is obviously a cone, and it is easy to check that it is
convex even though $$X$$ might not be. As an intersection of closed
half-spaces, it is also closed. The negative of the dual cone $$-X^*$$
is called the *polar* (or negative-dual) cone of $$X$$, usually
denoted $$X^\circ$$ (or $$X^-$$).

- TODO bidual
- TODO bidual for closed convex cones

## Moreau Decomposition

The Moreau decomposition generalizes the direct sum decomposition
between a linear subspace and its orthogonal complement to a convex
cone and its (negative) dual.

# Optimality Conditions

Suppose we want to minimize a function $$f: E \to \RR$$ over some set $$C$$. For
a given $$x \in C$$ to be a local minimizer means that $$f$$ can only increase
locally around $$x$$ in $$C$$. If $$f$$ is differentiable, this means that the
derivative of $$f$$ in any *admissible* direction $$v$$ should be positive:

$$\dd f(x).v = \lim_{\epsilon \underset{>0}{\to} 0}\ \frac{f(x + \epsilon v) - f(x)}\epsilon \geq 0$$

Now the set of admissible directions of $$C$$ at $$x$$ is obviously a subset of
the full tangent space $$T_x(E)$$ of $$E$$ at $$x$$, and should restrict the set
of tangent vectors to the ones that somehow remain in $$C$$ (to the first order).

In the general case, the correct definition is a bit technical (the Bouligand
tangent cone) but in the case where $$C$$ is convex, it suffices to consider the
set of directions that intersect $$C$$ near $$x$$:

$$T_x(C) = \left\{v \in T_x\block{E} :\ \exists \epsilon > 0 :  x + \lambda v \in C\quad\forall \lambda \in [0, \epsilon] \right\}$$

This subset is obviously a cone, called the *tangent cone* to $$C$$ at
$$x$$. It can be easily checked that for convex $$C$$, the tangent
cone at $$x$$ is given by:

$$T_x(C) = \RR^+\block{C - x}$$

which makes the cone structure even more explicit. The optimality
condition above can be rewritten as:

$$\dd f(x).v \geq 0\quad \forall v \in T_x(C)$$

Or, using the gradient of $$f$$:

$$\inner{\nabla f(x), v} \geq 0\quad \forall v \in T_x(C)$$

which is to say that $$\nabla f(x)$$ should belong to the *dual* of the tangent
cone at $$x$$, called the (negative) *normal cone* $$N_x(C)$$:

$$\nabla f(x) \in \block{T_x(C)}^* \triangleq -N_x(C)$$

From our earlier expression for $$T_x(C)$$, we obtain its dual as:

$$-N_x(C) = \block{T_x(C)}^* = \left\{y \in E : \inner{y, z - x} \geq 0\quad  \forall z \in C \right\}$$

When $$C=\cone{K}$$ is a closed convex cone, this means that:

$$y \in -N_x(\cone{K}) \iff \inner{y, z - x} \geq 0 \quad \forall z \in \cone{K}$$

Taking $$z = 0$$ and $$z = 2 x$$ yields $$\inner{y, x} = 0$$ hence $$y \in
x^\bot$$, which itself implies that $$\inner{y, z} \geq 0$$ and we also get $$y
\in \cone{K}^*$$. The converse is easy to check and we obtain:

$$-N_x(\cone{K}) = \cone{K}^* \cap x^\bot$$

Putting everything together, this means that the optimality conditions for
minimizing $$f$$ over a closed convex cone are:

$$\cone{K} \ni x\ \bot\ \nabla f(x) \in \cone{K}^*$$


# Farkas' Lemma

If we wish to impose constraints of the form $$Ax \in \cone{K}^*$$ for
some dual cone $$\cone{K}^*$$, we'll need to compute the preimage of
cone by a linear map. One can immediately check that
$$A^{-1}\cone{K}^*$$ is a closed cone. Furthermore:

$$\begin{align}
x \in \inv{A}\cone{K}^* &\iff Ax \in \cone{K}^* \\
&\iff \forall y \in \cone{K}\quad \inner{Ax, y} \geq 0 \\
&\iff \forall y \in \cone{K}\quad \inner{x, A^Ty} \geq 0 \\
&\iff \forall z \in A^T\cone{K}\quad \inner{x, z} \geq 0 \\
&\iff x \in \block{A^T\cone{K}}^*
\end{align}$$

And we obtain $$\inv{A}\cone{K}^* = \block{A^T\cone{K}}^*$$, known as
the (generalized) Farkas lemma. Note that we've been carefully
avoiding adherence issues, in particular the image of a closed cone by
a linear map in not necessarily closed[^farkas-lemma], which explains
why the Farkas Lemma is sometimes given as:

$$\block{\inv{A}\cone{K}}^* = \bar{\block{A^T\cone{K}^*}}$$

The version above sidesteps the issue by taking duals, which are
always closed, but this is something to keep in mind.

- TODO alternative theorems

# KKT Conditions

$$\min\ f(x) \st c(x) \in \cone{K}$$

for some convex closed cone $$\cone{K}$$. The admissible directions at
$$x$$ must satisfy:

$$\dd c(x).\dd x \in T_{c(x)}(\cone{K})$$

In other words, tangent vectors by $$c$$ must be admissible in
$$T_{c(x)}\cone{K}$$. From the Farkas' lemma, we get 

$$\dd c(x)^{-1} T_{c(x)}(\cone{K}) = \block{\dd c(x)^T \block{T_{c(x)}\cone{K}}^*}^*$$

where the inverse is meant as a preimage. One generally asks that
$$\dd c(x)^T \cone{K}^*$$ to be closed (see the
[discussion](#farkas-lemma) above) via a *constraint
qualification* condition, so that the dual cone is:

$$\begin{align}
\block{\dd c(x)^{-1} T_{c(x)}(\cone{K})}^* &= \dd c(x)^T \block{T_{c(x)}\cone{K}}^*\\
&= \dd c(x)^T \block{\cone{K}^* \cap c(x)^\bot}
\end{align}$$

from the [discussion](#optimality-conditions) above on normal cones to
convex closed cones. We are now ready to state the optimality
conditions for the constrained problem:

$$\nabla f(x) \in \dd c(x)^T \block{\cone{K}^* \cap c(x)^\bot}$$

which expands to:

$$\begin{align}
\exists \lambda \in \cone{K}^*: \nabla f(x) &= \dd c(x)^T \lambda\\
\cone{K} \ni c(x) &\ \bot\ \lambda \in \cone{K}^* \\
\end{align}$$

which are known as the Karush, Kuhn & Tucker (KKT) conditions.

## Example: Quadratic Programming

Here we minimize a quadratic function:

$$f(x) = \frac{1}{2}x^T Q x + c^T x$$ 

subject to constraints: 

$$g(x) = Ax - b \geq 0$$

The positive orthant cone $$\RR^n_+$$ is self-dual, and the KKT
conditions are:

$$\begin{align}
\exists \lambda \geq 0: Qx + c &= A^T \lambda\\
0 \leq Ax - b &\ \bot\ \lambda \geq 0 \\
\end{align}$$


# Duality

- TODO lol

-----

# Projection on a Convex Set

Let $$C$$ be a closed convex set. the projection of a point $$x$$ onto $$C$$ minimizes
the Euclidean norm (squared):

$$\pi_C(x) = \argmin{y \in C}\ \norm{x - y}^2$$

## Existence

TODO (weierstrass)


## Unicity

Assume there are two distinct $$c_1, c_2$$ that both minimize the distance
$$d^\star$$ to $$C$$. The midpoint of $$c_1, c_2$$ belongs to $$C$$ and its
distance squared $$d$$ to $$C$$ is:

$$\begin{aligned}
d = \norm{x - \frac{c_1 + c_2}{2}}^2 &= \norm{x - c_1 + \frac{c_1 - c_2}{2}}^2 \\
&= \underbrace{\norm{x - c_1}^2}_{d^\star} + \block{x - c_1}^T\block{c_1 - c_2} + \frac{1}{4}\norm{c_1 - c_2}^2 \\
&= \underbrace{\norm{x - c_2}^2}_{d^\star} + \block{x - c_2}^T\block{c_2 - c_1} + \frac{1}{4}\norm{c_2 - c_1}^2 \\
\end{aligned}$$

Therefore we get $$2d = 2d^\star - \frac{1}{2}\norm{c_2 - c_1}^2$$ which
contradicts the fact that $$c_1, c_2$$ are minimizers.

## Characterization

From the convexity of $$C$$ one can derive a characterization of the projection
similar to the stationary conditions in optimization as follows. Let us call $$c
= \pi_C(x)$$, for any $$y \in C$$ the point $$(1 - \lambda) c + \lambda y$$
belongs to $$C$$ for $$\lambda \in [0, 1]$$, therefore:

$$\begin{aligned}
\norm{x - c}^2 &\leq \norm{x - (1 - \lambda) c + \lambda y}^2 \\
 &= \norm{x - c + \lambda\block{c - y}}^2 \\
 &= \norm{x - c}^2 + \lambda\block{x - c}^T\block{c - y} + \lambda^2 \norm{c - y}^2 \\
 \end{aligned}$$
 
This means that for any $$\lambda \in [0, 1]$$ we have:

$$\lambda \block{x - c}^T{y - c} \leq \lambda^2 \norm{y - c}$$

which for all $$\lambda \in ]0, 1]$$ gives: 

$$\block{x - c}^T{y - c} \leq \lambda \norm{y - c}$$ 
 
Therefore $$\block{x - c}^T{y - c} \leq 0$$. Conversely, if $$\block{x -
c}^T{y - c} \leq 0$$ we get

$$\begin{aligned}
\norm{x - y}^2 &= \norm{x - c + c - y}^2 \\
&= \norm{x - c}^2 + \underbrace{2 \block{x - c}^T\block{c - y}}_{\geq 0} + \underbrace{\norm{c - y}^2}_{\geq 0} \\
\end{aligned}$$
 
therefore $$\norm{x - y}^2 \geq \norm{x - c}^2$$ and $$c = \pi_C(x)$$.
 



# Notes & References

[^dual-cone]: actually, the notion of dual cone can be expressed using
    only the canonical pairing between $$E$$ and its dual $$E^*$$

[^farkas-lemma]: consider the Lorentz cone projected on the plane with
    normal $$\mathbb{1}$$: the projection is the union of slices of
    the cone by planes parallel to the projection cone. The
    intersection of each slice with the cone is the surface above a
    parabola passing through the origin, whose shape flattens as the
    slice gets further from the origin. The union of all these subsets
    is the *open* half-plane plus the origin, which is not closed.
