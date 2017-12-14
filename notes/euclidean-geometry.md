---
title: Euclidean Geometry
categories: [math, lie group]
---

A couple of elementary (and useful) results in Euclidean geometry.

{% include toc.md %}


# TODO Orthogonal Projection


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
\ddd{\LL}{n} &= -n^T \underbrace{\block{p - \Sigma_i p_i}^T\block{p - \Sigma_i p_i}}_W + 2\lambda n^T \\

\ddd{\LL}{\lambda} &= \norm{n}^2 - 1 \\
\end{align}
$$

These conditions exactly state that $$n$$ must be a normalized
eigenvector of the positive semidefinite matrix $$W$$. Our
optimization problem is thus reduced to:

$$\argmin{Wn = \lambda n, \norm{n} = 1} \quad -n^TWn = -\lambda$$

whose solution is clearly the (normalized) eigenvector of $$W$$
corresponding to the largest eigenvalue.
