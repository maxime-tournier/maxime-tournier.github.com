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
*above* the graph of $$f$$. Equivalently:

$$f\block{x + \lambda(y - x)} \leq f(x) + \lambda \block{f(y) - f(x)}$$



### First-Order Conditions

For any $$0 < \lambda \leq 1$$ we have:

$$\frac{f\block{x + \lambda (y - x)} - f(x)}{\lambda} \leq f(y) - f(x)$$

Taking limits when $$\lambda$$ goes to zero gives:

$$\lim_{\lambda \downarrow 0} \frac{f\block{x + \lambda (y - x)} -
f(x)}{\lambda} \leq f(y) - f(x)$$

Therefore, if $$f$$ is differentiable at $$x$$, we obtain:

$$\dd f(x).(y - x) \leq f(y) - f(x)$$

Or, equivalently:

$$\inner{\nabla f(x), y - x} \leq f(y) - f(x)$$

In other words, $$f$$ always stays *above* its tangent space at
$$x$$. This condition is actually sufficient for convexity. Letting
$$z = (1 - \lambda) x + \lambda y$$, we obtain:

$$
\begin{align}
(1 - \lambda) f(x) &\geq (1 - \lambda)\block{f(z) + \dd f(z).(x - z)} \\
\lambda f(y) &\geq \lambda \block{f(z) + \dd f(z).(y - z)}\\
\end{align}
$$

and summing both equations provides the convexity of $$f$$:

$$(1 - \lambda) f(x) + \lambda f(y) \geq f(z) + \dd f(z).\underbrace{\block{(1 - \lambda) x + \lambda y - z}}_{0}$$

Besides, one can easily check that $$\dd f$$ is *monotone*, that is:

$$\block{\dd f(y) - \dd f(x)}(y - x) \geq 0$$

As it turns out, this condition is also sufficient for the convexity of
$$f$$. Let $$g(\lambda) = f\block{x + \lambda(y - x)}$$, we have:

$$\begin{align}
g(0) &= f(x) \\
g(1) &= f(y) \\
\end{align}$$

and by the mean value theorem, there exists some $$0 < \lambda < 1$$
such that:

$$\underbrace{g'(\lambda)}_{\dd f(z).(y - x)} = f(y) - f(x)$$

where once again $$z = (1 - \lambda) x + \lambda y$$. Assuming
monotonicity of $$\dd f$$ we obtain:

$$\block{\dd f(z) - \dd f(x)}\underbrace{(z - x)}_{\lambda\block{y - x}} \geq 0$$

and since $$\lambda > 0$$ this implies $$\block{\dd f(z) - \dd
f(x)}(y - x) \geq 0$$. Putting everything together:

$$f(y) - f(x) \geq \dd f(z).(y - x) \geq \dd f(x).(y - x)$$

hence by the previous argument $$f$$ is convex. 

### Second-Order Conditions

When $$f$$ is twice continuously differentiable, the monotonicity of
$$\dd f$$ implies the following, for $$\lambda > 0$$:

$$\frac{\dd f\block{x + \lambda (y - x)} - \dd f(x)}{\lambda}.(y - x) \geq 0$$

Taking limits as $$\lambda \downarrow 0$$, we obtain by the defintion
of the second derivative:

$$\dd^2 f(x)(y - x, y - x) \geq 0$$

Alternatively, using the Hessian matrix $$\nabla^2 f$$: 

$$(y - x)^T \nabla^2 f(x) (y - x) \geq 0$$

In other words, the Hessian of $$f$$ is positive semi-definite.

- TODO more properties
- TODO examples
- TODO strict convexity
- TODO strong convexity

# Optimization

- TODO proper convex functions
- TODO $$\min_{x \in C} f(x)$$ where $$f$$ is proper convex


## Unicity

Let us assume $$x_1, x_2 \in C$$ both minimize a *strict* convex function $$f$$
over $$C$$ with $$x_1 \neq x_2$$. In particular, we have $$f\block{x_1} =
f\block{x_2} = f^\star$$. The strict convexity of $$f$$ gives, for any $$0 <
\lambda < 1$$:

$$f(\underbrace{(1- \lambda) x_1 + \lambda x_2}_{\neq x_1, \neq x_2}) < (1 - \lambda) f\block{x_1} + \lambda f\block{x_2} = f^\star$$

which contradicts the fact that $$x_1, x_2$$ minimize $$f$$. Therefore the
minimizer, should it exist, must be unique. A similar argument can show that
when $$f$$ is merely convex, every point on the segment $$[x_1, x_2]$$ must also
minimize $$f$$.


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

$$\dd f(x).v = \lim_{\epsilon \downarrow 0}\ \frac{f(x + \epsilon v) - f(x)}\epsilon \geq 0$$

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

Putting everything together, this means that the optimality conditions
for minimizing $$f$$ over a closed convex cone $$\cone{K}$$ are the
following:

$$\cone{K} \ni x\ \bot\ \nabla f(x) \in \cone{K}^*$$

## Example: Linear Complementarity Problem

We consider the problem of minimizing a quadratic function over some closed
convex cone $$\cone{K}$$:

$$\min_{x \in \cone{K}} \ \frac{1}{2}x^TMx + q^Tx$$

The optimality conditions are the following (linear) *Cone Complementarity
Problem*:

$$\begin{align}
\quad Mx + q &= \lambda \\
\cone{K} \ni x&\ \bot\ \lambda \in \cone{K}^*
\end{align}$$

When $$\cone{K}=\RR^n_+$$ is the positive orthant (self-dual), these conditions
are known as a Linear Complementarity Problem (LCP):

$$\begin{align}
\quad Mx + q &= \lambda \\
0 \leq x&\ \bot\ \lambda \geq 0
\end{align}$$




# Farkas' Lemma

If we wish to impose constraints of the form $$Ax \in \cone{K}^*$$ for
some dual cone $$\cone{K}^*$$, we'll need to compute the preimage of
cone by a linear map. One can immediately check that the preimage
$$A^{-1}\cone{K}^*$$ is a closed cone. Furthermore:

$$\begin{align}
x \in \inv{A}\cone{K}^* &\iff Ax \in \cone{K}^* \\
&\iff \forall y \in \cone{K}\quad \inner{Ax, y} \geq 0 \\
&\iff \forall y \in \cone{K}\quad \inner{x, A^Ty} \geq 0 \\
&\iff \forall z \in A^T\cone{K}\quad \inner{x, z} \geq 0 \\
&\iff x \in \block{A^T\cone{K}}^*
\end{align}$$

And we obtain $$\inv{A}\cone{K}^* = \block{A^T\cone{K}}^*$$, a result
known as the (generalized) Farkas lemma. Note that we've been
carefully avoiding adherence issues, in particular the image of a
closed cone by a linear map in not necessarily closed[^farkas-lemma],
which explains why the Farkas Lemma is sometimes given as:

$$\block{\inv{A}\cone{K}}^* = \bar{\block{A^T\cone{K}^*}}$$

The version above sidesteps the issue by taking duals, which are
always closed, but this is something to keep in mind.

- TODO alternative theorems

# KKT Conditions

We now consider the following optimization problem:

$$\min_{x \in E} \ f(x)\ \st \ c(x) \in \cone{K}$$

for some convex closed cone $$\cone{K}$$. The admissible directions at
$$x$$ must satisfy:

$$\dd c(x).\dd x \in T_{c(x)}(\cone{K})$$

In other words, tangent vectors outputted by $$c$$ must be admissible
in $$\cone{K}$$, that is: belong to $$T_{c(x)}\cone{K}$$. From Farkas'
lemma, we get:

$$\dd c(x)^{-1}\block{T_{c(x)}(\cone{K})} = \block{\dd c(x)^T \block{T_{c(x)}\cone{K}}^*}^*$$

where the inverse is understood as a preimage. One generally asks that
$$\dd c(x)^T \cone{K}^*$$ be closed (see the
[discussion](#farkas-lemma) above) via a *constraint qualification*
condition, so that the dual cone is:

$$\begin{align}
\block{\dd c(x)^{-1} \block{T_{c(x)}(\cone{K})}}^* &= \dd c(x)^T \block{T_{c(x)}\cone{K}}^*\\
&= \dd c(x)^T \block{\cone{K}^* \cap c(x)^\bot}
\end{align}$$

(see the [discussion](#optimality-conditions) above on normal cones to
convex closed cones). We are now ready to state the optimality
conditions for the constrained problem:

$$\nabla f(x) \in \dd c(x)^T \block{\cone{K}^* \cap c(x)^\bot}$$

which expands to:

$$\begin{align}
\nabla f(x) &= \dd c(x)^T \lambda\\
\cone{K} \ni c(x) &\ \bot\ \lambda \in \cone{K}^* \\
\end{align}$$

These are known as the Karush, Kuhn & Tucker (KKT) conditions.

## Example: Quadratic Programming

Here we minimize a quadratic function:

$$f(x) = \frac{1}{2}x^T Q x + c^T x$$ 

subject to constraints: 

$$g(x) = Ax - b \geq 0$$

The positive orthant cone $$\RR^n_+$$ is self-dual, and the KKT
conditions are:

$$\begin{align}
Qx + c &= A^T \lambda\\
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

[^dual-cone]: Actually, the notion of dual cone can be expressed using
    only the canonical pairing between $$E$$ and its dual $$E^*$$

[^farkas-lemma]: Consider the Lorentz cone projected on the plane of
    normal $$(1, 1, \ldots, 1)^T$$: the projection is the union of
    slices of the cone by planes parallel to the projection
    plane. Each slice is delimited by a parabola passing through
    the origin of the slicing plane, and whose shape flattens as the
    slicing plane gets further from the origin. The union of all these
    slices is the *open* half-plane plus the origin, which is not
    closed.
