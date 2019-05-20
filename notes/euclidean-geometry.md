---
title: Euclidean Geometry
categories: [math]
---

A couple of elementary (and useful) results in Euclidean geometry.

{% include toc.md %}

# Inner Product

We consider a finite-dimensional real vector space $$E$$ of dimension
$$n \in \NN$$. An *inner product* on $$E$$ is a bilinear form
$$\inner{.,.}$$ that is:

- symmetric: $$\inner{x, y} = \inner{y, x}$$
- positive: $$\inner{x, x} \geq 0$$
- definite: $$\inner{x, x} = 0 \iff x = 0$$

One can show that an inner product uniquely corresponds to a matrix
$$M$$ that is symmetric, positive definite so that:

$$\inner{x, y} = x^T M y$$

We will happily abuse notations and use $$M$$ to designate the
corresponding inner product and conversely.

# Euclidean Norm

One can verify that the Euclidean norm, defined by:

$$\norm{x}_M^2 = \inner{x, x}$$

is actually a norm:

- positive: $$\norm{x} \geq 0$$
- separation: $$\norm{x} = 0 \Rightarrow x = 0$$
- multiplicative: $$\norm{\lambda x} = \lvert \lambda \rvert \norm{x}$$
- triangle inequality: $$\norm{x + y} \leq \norm{x} + \norm{y}$$

(TODO) triangle inequality

# Orthogonal Group

Given an inner product $$M$$ on $$E$$, it is natural to ask which
linear applications preserve it. That is, we look for (non-degenerate)
applications $$U$$ such that:

$$\inner{Ux, Uy} = \inner{x, y}$$

By choosing $$x = e_i, y = e_j$$, one can see that $$U$$ satisfies:

$$U^TMU = M$$

The above set forms a group under matrix multiplication, called the
*orthogonal group*. It depends on the inner product $$M$$, even though
one is generally interested in the standard Euclidean norm on $$E$$,
corresponding to $$M = I$$, in which case:

$$U^T U = I$$

# TODO Orthogonal Basis

Linear subspaces have a basis, and it is always possible to compute an
orthogonal basis from an arbitraty basis.

## TODO Graham-Schmidt Orthogonalization


# TODO Orthogonal Projection

The Euclidean metric can be used to find the closest point to a given linear
subspace: let us consider a linear subspace $$V$$ which we identify with its
orthogonal basis $$\block{V_i}_i$$. We may complete this basis into a basis of
the full Euclidean space, which we can again orthogonalize using the
Graham-Schmidt process: 

$$\block{U_i}_i=\block{\block{V_i}_{i\leq n}, \block{W_i}_{i\leq m}} $$ 

In particular, any vector in $$W_i$$ is orthogonal to all $$V_i$$ vectors and
conversely. We call $$W$$ the orthogonal complement of $$V$$.


# Line Fitting

We consider the problem of finding the *best* affine line
approximation for a given set of samples $$\block{p_i}_{i\leq
k}$$. More precisely, we look for the line origin $$x$$ and direction
$$n$$, with $$\norm{n}=1$$, that solves the following minimization
problem:

$$\argmin{D} \quad \half\sum_{i=0}^k \norm{p_i - \pi_D\block{p_i}}^2$$

where $$\pi_D(y)$$ is the orthogonal projection of $$y$$ on our line
$$D = (x, n)$$. In other words, we seek to minimize the squared norm
of the residuals. A bit of algebra quickly shows that the projection
can be expressed as:

$$\pi_D(y) = x + nn^T(y - x)$$

Moreover, the projection being orthogonal, the residual squared norm
satisfies:

$$\norm{y - \pi_D(y)}^2 = \norm{y - x}^2 + \norm{nn^T(y - x)}^2$$

by Pythagore's theorem. We may rewrite our minimization problem as:

$$\argmin{x, \norm{n}^2 = 1} \quad \half\sum_{i=0}^k \norm{x -
p_i}^2 - \block{x - p_i}n^Tn \block{x - p_i}$$

At this point it is worth noting that our line parametrization is
redundant since translating $$x$$ by any amount of $$n$$ yields the
same line. This reflects on our cost function, meaning that we should
provide an extra condition on $$x^T n$$ if we desire a unique
solution.

With this in mind, we examine the first-order optimality conditions of
the Lagrangian:

$$\LL(x, n, \lambda) = \half\sum_{i=0}^k \block{\norm{x - p_i}^2 - \block{x - p_i}n^Tn \block{x - p_i}} + \lambda^T\block{\norm{n}^2 - 1}$$

whose partial derivatives are:

$$
\begin{align}
\ddd{\LL}{x} &= \sum_i \block{x - p_i}^T + \block{x - p_i}^Tnn^T \\
    &= \block{kx - \Sigma_ip_i}^T\block{I - nn^T} \\ 

\ddd{\LL}{n} &= -n^T \block{x - \Sigma_i p_i}^T\block{x - \Sigma_i p_i} + 2\lambda n^T \\

\ddd{\LL}{\lambda} &= \norm{n}^2 - 1 \\
\end{align}
$$

The optimality condition $$\ddd{\LL}{x}=0$$ tells us that the
projection of $$x$$ on $$n^\bot$$ is that of the mean $$p =
\frac{1}{k}\Sigma_i p_i$$. As noted before, we may also force its
projection on $$n$$ without losing generality. We conveniently chose
$$x^T n = p^T n$$, and obtain that a solution must have $$x = p$$. In
other words, we just reduced our inital problem to the simpler,
following one:

$$\argmin{\norm{n}^2 = 1} \quad \half\sum_{i=0}^k - \block{p - p_i}n^Tn \block{p - p_i}$$

whose Lagrangian partial derivatives are:

$$
\begin{align}
\ddd{\LL}{n} &= -n^T \underbrace{\sum_i\block{p - p_i}^T\block{p - p_i}}_W + 2\lambda n^T \\

\ddd{\LL}{\lambda} &= \norm{n}^2 - 1 \\
\end{align}
$$

These conditions exactly state that $$n$$ must be a normalized
eigenvector of the positive semidefinite dispertion matrix $$W$$. Our
optimization problem is thus reduced to:

$$\argmin{Wn = \lambda n, \norm{n} = 1} \quad -n^TWn = -\lambda$$

whose solution is clearly the (normalized) eigenvector of $$W$$
corresponding to the largest eigenvalue.
