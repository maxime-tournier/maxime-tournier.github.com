---
title: Convex Optimization
categories: [math]
tags: [draft]
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


## First-Order Conditions

When $$f$$ is smooth, convexity translates to special properties on the
differential $$\dd f$$. For any $$0 < \lambda \leq 1$$ we have:

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

## Second-Order Conditions

Similarly, $$\dd f$$ being smooth and monotone implies special properties on the
Hessian $$\nabla^2 f$$. When $$f$$ is twice continuously differentiable, the
monotonicity of $$\dd f$$ implies the following, for $$\lambda > 0$$:

$$\frac{\dd f\block{x + \lambda (y - x)} - \dd f(x)}{\lambda}.(y - x) \geq 0$$

Taking limits as $$\lambda \downarrow 0$$, we obtain by the defintion
of the second derivative:

$$\dd^2 f(x)(y - x, y - x) \geq 0$$

Alternatively, using the Hessian matrix $$\nabla^2 f$$: 

$$(y - x)^T \nabla^2 f(x) (y - x) \geq 0$$

In other words, the Hessian of $$f$$ is positive semi-definite.


## Strict/Strong Convexity

Recall the definition of convexity for a function $$f$$:

$$f\block{x + \lambda(y - x)} \leq f(x) + \lambda \block{f(y) - f(x)}$$

$$f$$ is said to be *strictly* convex when the above inequality is strict
everywhere except at endpoints $$\lambda = 0$$ and $$\lambda = 1$$. As we'll see
below, strict convexity helps establishing uniqueness of solutions in
mimimization problems. Intuitively, a strictly convex function is never "flat".

In practice however, a strictly convex function is allowed to be arbitrarily
close to being flat somewhere. In contrast, a *strongly* convex function is
never flatter than a given quadratic function:

$$f\block{\lambda x + \block{1- \lambda} y} \leq \lambda f(x) + (1 - \lambda) f(y) - \frac{m}{2} \lambda(1 - \lambda) \norm{x - y}^2$$

Note that $$m = 0$$ recovers the definition of convexity. As
[before](#first-order-conditions), we get:

$$\lim_{\lambda \downarrow 0} \frac{f\block{x + \lambda\block{y - x}} - f(x)}{\lambda} \leq f(y) - f(x) - \frac{m}{2}(1 - \lambda) \norm{x - y}^2$$

so that $$f$$ being smooth and strongly convex implies:

$$f(x) + \dd f(x).(y - x) + \frac{m}{2} \norm{x - y}^2 \leq f(y)$$

When $$f$$ is twice differentiable, one can show that strong convexity implies
that $$\nabla^2 f(x) \succeq m I$$. In fact, both first and second order
conditions are equivalent to $$f$$ being strongly convex, with proofs similar to
the ones for simple convexity.

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

The Moreau decomposition generalizes the direct sum decomposition between a
linear subspace and its orthogonal complement to a convex cone and its
(negative) dual. Given a cone $$\cone{K}$$, any $$x \in E$$ decomposes
*uniquely* as:

$$x = x_\cone{K} - x_{\cone{K}^*}$$

with $$\inner{x_\cone{K}, x_{\cone{K}^*}} = 0$$. Equivalently:

- $$x_\cone{K} = P_\cone{K}(x)$$
- $$-x_{\cone{K}^*} = P_{-\cone{K}^*}(x)$$

(TODO proof)

# Optimality Conditions

Suppose we want to minimize a function $$f: E \to \RR$$ over some set $$C$$. For
a given $$x \in C$$ to be a local minimizer means that $$f$$ can only increase
locally around $$x$$ in $$C$$. If $$f$$ is differentiable, this means that the
derivative of $$f$$ in any *admissible* direction $$v$$ should be positive:

$$\dd f(x).v = \lim_{\epsilon \downarrow 0}\ \frac{f(x + \epsilon v) - f(x)}\epsilon \geq 0$$

Now the set of admissible directions of $$C$$ at $$x$$ is obviously a subset of
the full tangent space $$T_x(E)$$ of $$E$$ at $$x$$, and should restrict the set
of tangent vectors to the ones that somehow remain in $$C$$ (to the first order).

### Tangent Cone
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

### Normal Cone to a Convex Set

- TODO $$N_x(C) = -\block{C - x}^*$$ when $$C$$ is convex

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


# Cone Indicator Function

Assuming we know how to minimize a convex function $$f$$ over a closed convex
cone $$\cone{K}$$, we could in theory solve a minimization problem under the
constraint that some other smooth function $$g(x) \in \cone{K}^*$$ by
adding a penalty term $$p(x)$$ such that:

- $$p(x) = 0$$ when $$g(x) \in \cone{K}^*$$
- $$p(x) = +\infty$$ when $$g(x) \notin \cone{K}^*$$

Such a penalty function is usually known as the *indicator function* $$\iota_C$$
over the set $$C = g^{-1}\block{\cone{K}^*}$$. Such an indicator function is
convex when $$C$$ is convex and so in principle one could just minimize $$f$$
over $$C$$ when $$C$$ is a cone and we're done.

In practice however, it's not always clear how to compute $$C$$ from $$g$$ and
$$\cone{K}$$, or that $$C$$ even is a cone to begin with. Despite all this,
there is a simple trick to express $$\iota_C$$ in terms of $$g$$ by adding extra
variables known as the *Lagrange multipliers*:

$$p(x) = \max_{\lambda \in \cone{K}} \ -\inner{\lambda^T, g(x)}$$

By the very definition of $$\cone{K}^*$$, we get that $$\inner{\lambda, g(x)}
\geq 0$$ for all $$\lambda \in \cone{K}$$, so that 

$$p(x) = \max_{\lambda \in \cone{K}} \ -\inner{\lambda, g(x)} = 0$$ 

when $$g(x) \in \cone{K}^*$$. Conversely, when $$g(x) \notin \cone{K}^*$$, it
has a non-zero [Moreau](#moreau-decomposition) component $$\mu$$ over
$$-\cone{K}$$, for which $$\inner{\mu, g(x)} = \mu^2 > 0$$. So there exists
$$\lambda = -\mu \in \cone{K}$$ such that $$-\inner{\lambda, g(x)} > 0$$, which
can grow arbitrary large as $$\lambda$$ scales in $$\cone{K}$$. In other words:

$$p(x) = \max_{\lambda \in \cone{K}} \ -\inner{\lambda, g(x)} = +\infty$$ 

when $$g(x) \notin \cone{K}^*$$. Our initial problem becomes:

$$\min_x\ \max_{\lambda \in \cone{K}} \ f(x) - \inner{\lambda, g(x)}$$

The function $$\LL(x, \lambda) = f(x) - \inner{\lambda, g(x)}$$ is
called the *Lagrangian* of the original constrained problem, which is
reduced to solving the following (unconstrained) $$\min-\max$$
problem:

$$\min_x\ \max_{\lambda \in \cone{K}} \ \LL(x, \lambda)$$

# Duality

What happens when the order of $$\min-\max$$ is reversed in the above? The
general max-min inequality applies:

$$\sup_\lambda \inf_x \LL(x, \lambda) \leq \inf_x \sup_\lambda \LL(x, \lambda)$$

Indeed, let us call $$d(\lambda) = \inf_x \LL(x, \lambda)$$ and $$p(x) =
\sup_\lambda \LL(x, \lambda)$$, we see that:

$$\forall x: d(\lambda) \leq \LL(x, \lambda) \leq p(x)$$

so that any $$p(x)$$ is an upper bound of $$d(\lambda)$$, which implies
$$\sup_\lambda d(\lambda) \leq p(x)$$ for all $$x$$, by definition of
$$\sup$$. In turn, $$\sup_\lambda d(\lambda)$$ is a lower bound of any $$p(x)$$,
therefore $$\sup_\lambda d(\lambda) \leq \inf_x p(x)$$ and the result
follows. Since all sets involved are assumed to be closed and convex, we even
get $$\max$$ for $$\sup$$ and $$\min$$ for $$\inf$$. In particular:

$$d^\star = \max_\lambda d(\lambda) \leq \min_x p(x) = p^\star$$

We see that minimizing the *primal* function $$p(x)$$ (our original problem) is
related to maximizing the *dual* function $$d(\lambda)$$ in the sense that
solving the dual problem provides a lower bound on the solution of the primal
problem. This is called *weak duality*, and the difference $$p^\star - d^\star$$
is the *duality gap*. There are situations in which solving either problem is
equivalent to solving the other, in which case the duality gap is $$0$$: this is
called *strong duality*. For instance, the existence of a *saddle point*
$$\block{x^\star, \lambda^\star}$$ such that

$$\LL\block{x^\star, \lambda} \leq \LL\block{x^\star, \lambda^\star} \leq \LL\block{x, \lambda^\star}$$

for all $$x, \lambda$$. Indeed, from $$\LL\block{x^\star, \lambda^\star} \leq
\LL\block{x, \lambda^\star}$$ we get 

$$\LL\block{x^\star, \lambda^\star} = \min_x \LL\block{x, \lambda^\star} = d\block{\lambda^\star} \leq d^\star$$ 

and similarly from $$\LL\block{x^\star, \lambda} \leq \LL\block{x^\star, \lambda^\star}$$ we obtain

$$\LL\block{x^\star, \lambda^\star} = \max_\lambda \LL\block{x^\star, \lambda} = p\block{x^\star} \geq p^\star$$ 

which gives $$d^\star \geq p^\star$$ and the duality gap is 0. 

We note that whenever $$g(x) \in \cone{K}^*$$ and $$\lambda \in
\cone{K}$$, the Lagrangian $$\LL(x, \lambda)$$ is a lower bound on the
primal objective function $$f(x)$$, therefore the dual problem can be
seen as maximizing this lower bound.

## Examples

### Linear Programs (LP)

Let us consider the following *linear program*:

$$\argmin x \quad c^T x \quad \st \quad  Ax \geq b$$

With the above notation, the constraint function is $$g(x) = Ax - b$$
and the constraint cone is the (self-dual) positive orthant
$$\RR^{m+}$$, so the Lagrangian is the following:

$$\LL(x, \lambda) = c^T x - (Ax - b)^T \lambda$$

with $$\lambda \geq 0$$. Expanding the Lagrangian

$$\LL(x, \lambda) = c^T x - (Ax - b)^T \lambda = b^T \lambda -\block{A^T \lambda - c}^T x$$

we see that this Lagrangian could as well be involved in optimizing
$$b^T \lambda$$ under the constraint that $$ A^T \lambda = c$$ and
indeed, the primal and dual functions as defined above make this
connection explicit:

$$p(x) = \max_{\lambda \geq 0} c^T x - (Ax - b)^T \lambda$$

exactly encodes our original LP as expected, and the dual

$$d(\lambda) = \min_{x} c^T x - (Ax - b)^T \lambda = b^T \lambda - \block{A^T \lambda - c}^T x$$

is unbounded by below when $$A^T \lambda \neq c$$, and equal to
$$b^T\lambda$$ otherwise, so the dual problem is

$$\max_{\lambda \geq 0} b^T \lambda \quad \st \quad A^T \lambda = c$$


### Quadratic Programs (QP)

Let us consider the following *quadratic program*:

$$\min x \quad \half x^T Q x + c^T x \quad \st \quad  Ax \geq b$$

The Lagrangian is:

$$\LL(x, \lambda) = \half x^T Q x + c^T x - \lambda^T\block{Ax - b}$$

for $$\lambda \geq 0$$. The dual function is therefore:

$$
\begin{aligned}
d(\lambda) &= \min_x \quad \half x^T Q x + c^T x - \lambda^T\block{Ax - b}\\
&= \min_x \quad \half x^T Q x + x^T\block{c - A^T\lambda} + b^T \lambda
\end{aligned}
$$

When $$Q$$ is positive-definite, this minimum is obtained for:

$$x^\star(\lambda) = Q^{-1}\block{A^T \lambda - c}$$

and the dual problem is:

$$
\begin{aligned}
\max_{\lambda \geq 0} \LL\block{x^\star(\lambda), \lambda} 
&= \max_{\lambda \geq 0} \quad \half \block{A^T \lambda - c} Q^{-1} \block{A^T \lambda - c} + \block{A^T \lambda - c}^T\block{c - A^T\lambda} + b^T \lambda \\
&= \max_{\lambda \geq 0} \quad  -\half \block{A^T \lambda - c} Q^{-1} \block{A^T \lambda - c} + b^T \lambda \\
\end{aligned}
$$

whose solution $$\lambda^\star$$ can be found by solving the following QP (dropping constant terms):

$$\min_{\lambda \geq 0}\quad \half \lambda^T AQ^{-1}A^T \lambda + \lambda^T\block{b + Ac}$$

# ADMM

## Dual Ascent 

Let us consider the following problem:

$$\min_x \quad f(x) \quad \st A x = b$$

Again, the Lagrangian is:

$$\LL(x, \lambda) = f(x) - \lambda^T\block{Ax - b}$$

with dual function:

$$d(\lambda) = \min_x \LL(x, \lambda) = f(x) - \lambda^T\block{Ax - b}$$

Assuming that $$f$$ is smooth and that we can compute the dual function its
gradient is trivial to compute:

$$\nabla d(\lambda) = Ax^\star(\lambda) - b$$

where $$x^\star(\lambda) = \argmin{x}\quad \LL(x, \lambda) = f(x) - \lambda^T\block{Ax -
b}$$. Indeed, by definition of the dual function, we have:

$$d(\lambda) = \LL\block{x^\star(\lambda), \lambda}$$

But since $$x^\star(\lambda)$$ minimizes $$\LL\block{\cdot, \lambda}$$, the
derivative along $$x$$ at the optimum is zero:

$$\ddd{\LL}{x}\block{x^\star(\lambda), \lambda} = 0$$

so that only $$\ddd{\LL}{\lambda}\block{x^\star(\lambda), \lambda} =
\block{Ax^\star - b}^T$$ appears in the differential of $$d(\lambda)$$. We now
have a smooth[^dual-ascent] function and its gradient, we can simply use gradient ascent with
step sizes $$\alpha_k$$ to solve the dual problem of maximizing $$d(\lambda)$$:

1. initialize $$\lambda_0 = 0$$
2. solve $$x_k = \argmin{x}\quad  f(x) - \lambda_k^T\block{Ax - b}$$
3. update $$\lambda_{k+1} = \lambda_k - \alpha_k \block{A x_k - b}$$
4. goto 2 until sufficient precision is achieved (more on this below)

Note that when $$f$$ is not smooth, the above procedure provides a subgradient
at each iteration and the method is known as *dual subgradient ascent*.

## Method of Multipliers

As usual, there are conditions to be met for the simple gradient ascent to
converge. Unfortunately, it is not trivial to get Lipschitz constants for the
dual function, therefore it might be difficult to converge robustly in practice.

In order to improve convergence, the *Method of Multipliers* replaces
the initial problem with the following, equivalent one:

$$\min_x \quad f(x) + \rho \norm{Ax - b}^2 \quad \st A x = b$$

whose Lagrangian is usually called the *Augmented Lagrangian*. Clearly the added
penalty is zero on the feasible set, so the two problems are equivalent. In a
sense, doing so regularizes the function $$f$$ outside the feasible set by
driving solutions towards the feasible set, over which the regularization
vanishes.

Applying dual ascent to the regularized problem yields the following optimality
conditions *(dual feasibility)* after solving the optimization problem at each
iteration:

$$\nabla f\block{x_k} + \underbrace{\rho A^T\block{Ax_k - b} - A^T\lambda_k}_{-A^T\block{\lambda_k - \rho\block{Ax_k - b}}} = 0$$

This suggests that picking step size $$\alpha_k = \rho$$ such that
$$\lambda_{k+1} = \lambda_k - \rho\block{Ax_k - b}$$ will have the following
benefits:

- dual feasibility of the regularized problem at $$\lambda_k$$ is equivalent to
  dual feasibility *of the original problem* at $$\lambda_{k+1}$$: $$\nabla
  f\block{x_k} -A^T\lambda_{k+1} = 0$$
- this corresponds to *implicit integration* of the gradient flow of the dual
  function of the original problem: the gradient found by solving the
  regularized problem is that of the original problem at $$\lambda_{k+1}$$
  instead of $$\lambda_k$$

and we might expect the nice properties of implicit integration to somehow
ensure convergence. More precisely, convergence can be shown by considering the
dual error $$V_k = \norm{\lambda_{k+1} - \lambda^\star}^2$$ where
$$\lambda^\star$$ is the solution:

$$
\begin{aligned}
V_{k+1} - V_k &= \norm{\lambda_{k+1} - \lambda_k + \lambda_k - \lambda^\star}^2 - \norm{\lambda_k - \lambda^\star}^2\\
&= \norm{\lambda_{k+1} - \lambda_k}^2 + \norm{\lambda_k - \lambda^\star}^2 + 2\block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_k - \lambda^\star} - \norm{\lambda_k - \lambda^\star}^2 \\
&= \norm{\lambda_{k+1} - \lambda_k}^2 + 2\block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_k - \lambda^\star} \\
&= \norm{\lambda_{k+1} - \lambda_k}^2 + 2\block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_k - \lambda_{k+1} + \lambda_{k+1} - \lambda^\star} \\
&= - \norm{\lambda_{k+1} - \lambda_k}^2 + 2 \block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_{k+1} - \lambda^\star} \\
\end{aligned}
$$

The second term can be rewritten in terms of $$x$$:

$$
\begin{aligned}
\block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_{k+1} - \lambda^\star} &=
    -\rho\block{A x_k - b}^T \block{\lambda_{k+1} - \lambda^\star} \\
    &= -\rho\block{x_k - x^\star}^TA^T\block{\lambda_{k+1} - \lambda^\star} \\
    &= -\rho\block{x_k - x^\star}^T\block{\nabla f\block{x_k} - \nabla f\block{x^\star}} \\ 
    &\leq 0
\end{aligned}
$$

due to $$\nabla f$$ being monotone since $$f$$ is convex. This means the method
strictly converges while $$x_k$$ is not primal feasible, and terminates with a
result that is both primal and dual feasible. One can also show that the Method
of Multipliers corresponds to the proximal point algorithm applied to the dual
function (dual proximal point method).

## Alternating Direction Method of Multipliers

So far, so good: we solved the problem of choosing step sizes
$$\alpha_k$$ and still get convergence, which is nice. One practical
issue is that the penalty term $$\norm{Ax - b}^2$$ introduces coupling
between variables that may not appear in function $$f$$: while dual
ascent could optimize a separable function $$f(x) = g(y) + h(z)$$
well, separately (possibly using dedicated, optimized solvers), this
is no longer possible with the Method of Multipliers.

The *Alternating Direction Method of Multipliers* improves the situation by
working around the coupling introduced by constraint matrix. Let us introduce
some notation first: we consider the following problem of minimizing a separable
function under affine constraints:

$$\min_{x, z}\quad f(x) + g(z)\quad\st\ Ax + Bz = c$$

Instead of minimizing *jointly* over both $$x, z$$ like the Method of
Multipliers would, the minimization is split into two subproblems: minimizing
along $$x$$ alone first with $$z$$ constant, then along $$z$$ alone with $$x$$
constant, in a Gauss-Seidel fashion:

1. initialize $$\lambda_0 = 0, z_0 = 0$$
2. solve $$x_k = \argmin{x}\quad f(x) - \lambda_k^T\block{Ax + Bz_{k-1} - c} + \rho\norm{Ax + Bz_{k-1} - c}^2$$
2. solve $$z_k = \argmin{z}\quad g(z) - \lambda_k^T\block{Ax_k + Bz - c} + \rho\norm{Ax_k + Bz - c}^2$$
3. update $$\lambda_{k+1} = \lambda_k - \rho \block{A x_k + Bz_k - b}$$
4. goto 2 until sufficient precision is achieved (more on this below)

This is equivalent to alternating two Method of Multiplier solves in
the $$x, z$$ directions with varying constraint values, hence the
name. Crucially, the matrices $$A, B$$ remain constant so one can
usually do some preprocessing so that inner solves for $$x, z$$ are as
efficient as possible. The method is still not parallel, but it
becomes possible to employ dedicated, optimized solvers for each
subproblems, establishing consensus as the iteration converges.

### Scaled Form


## Stopping Criterion


## TODO

- convex conjugate, relation to dual function/problem


# Subgradients, Subdifferential

As we saw [above](#first-order-conditions), $$f$$ being smooth and convex
implies it always stays *above* its first-order approximation:

$$\inner{\nabla f(x), y - x} \leq f(y) - f(x)$$

When $$f$$ is *not* smooth at $$x$$, there may still exist a set of vectors
$$g$$ such that the above holds. We call such vectors *subgradients* of $$f$$ at
$$x$$. The set of all subgradients at $$x$$ is called the *subdifferential* of
$$f$$ at $$x$$, denoted by:

$$\partial f(x) = \left\{g: \inner{g, y - x} \leq f(y) - f(x) \right\}$$

TODO examples (norm)

The subdifferential of $$f$$ can be seen as a multivalued function, mapping a
set of subgradients to every point of the domain. It is easy to see that this
set is convex. It is also closed, but it may be empty (TODO unbounded
operators). As one can expect, when $$f$$ is smooth at $$x$$, we get 

$$\partial f(x) = \left\{\nabla f(x)\right\}$$

by showing that $$\inner{u - \nabla f(x), v} \leq 0$$ for all $$v$$. We saw
[earlier](#first-order-conditions) that when $$f$$ is smooth and convex, its
gradient is *monotone*:

$$\inner{\nabla f(y) - \nabla f(x), y - x} \geq 0$$

The subdifferential is also monotone in the following sense:

$$\forall u \in \partial f(x), v \in \partial f(y): \inner{v - u, y - x} \geq 0$$

Indeed, for all $$x, y$$ we get:


$$\begin{aligned}
\inner{u, y - x} &\leq f(y) - f(x) \\
\inner{v, x - y} &\leq f(x) - f(y) \\
\end{aligned}$$

for any $$u\in \partial f(x), v \in \partial f(y)$$, therefore:

$$\begin{aligned}
\inner{v, y - x} &\geq f(y) - f(x)\\
\inner{-u, y - x} &\geq f(x) - f(y)\\
\end{aligned}$$

and the result follows by summation. Obviously, $$f$$ being minimal at
$$x^\star$$ is equivalent to having $$0 \in \partial f(x)$$, which since
$$\partial f$$ is monotone is called a *monotone inclusion problem*.

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

[^dual-ascent]: Assuming strict convexity for $$f$$
