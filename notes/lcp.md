---
title: Linear Complementarity Problem
categories: [math]
---

A few notes on Linear Complementarity (LCP) algorithms for Symmetric Positive
Definite (SPD) matrices.

# Problem

Given an $$n\times n$$ SPD matrix $$M \succ 0$$ and a vector $$q \in \RR^n$$, the goal is
to find $$x \in \RR^n$$ such that:

$$0 \leq x \ \bot\  \underbrace{M x + q}_\lambda \geq 0$$

These are the [KKT conditions](convex-optimization#kkt-conditions) of the
following convex QP:

$$\min_{x \geq 0}\ \half x^TMx + q^T x$$

which may be a more suitable point of view for *e.g.* iterative
solvers. Assuming we're given a [Cholesky decomposition](cholesky)
of $$M = LL^T$$, and letting $$y = L^Tx$$ one can rewrite the above as:

$$\min_{L^{-T}y \geq 0} \norm{y - r}^2$$

where $$r = -L^{-1}q$$. The KKT conditions become:

$$\exists \lambda \geq 0: \quad y = r + L^{-1} \lambda$$ 

where $$0 \leq x \ \bot\ \lambda \geq 0$$. One may rewrite the cone condition
$$L^{-T}y \geq 0$$ as $$y \in L^T K$$ where $$K = \RR^{n+}$$ is the self-dual
positive orthant, and letting

$$\mu = L^{-1}\lambda \in L^{-1} K = \block{L^T K}^*$$

we obtain the [Moreau decomposition](convex-optimization#moreau-decomposition)
of $$r = y - \mu$$ along the cone $$L^T K$$ and its (negative)
dual. Equivalently, $$q = Mx - \lambda$$ is a Moreau decomposition of $$q$$
along $$-K$$ and its (negative) $$M^{-1}$$-dual.

# Projected Jacobi/Gauss-Seidel/SOR

- TODO jacobi/gauss-seidel + project negative variables
- simple/reasonably
- linear convergence
- see [iterative methods](iterative-methods) for the theory in the linear case

# Modulus

# Dantzig-Cottle

As usual, we use a set of complementary indices $$B, N$$ to designate *basic*
(the ones to solve for) and *non-basic* (the ones fixed to zero) variables. The
general idea of the Dantzig-Cottle algorithm is fairly simple:

- Start with $$x = 0, \lambda = q, B = \emptyset, N = \emptyset$$
- If there exist an index $$i$$ such that $$\lambda_i < 0$$, attempt to
  *continuously* bring $$\lambda_i$$ to $$0$$ while tracking sign changes in
  $$x, \lambda$$ coordinates. When this happen, pivot the system accordingly.
- Repeat

Of course, the heart of the algorithm is the procedure to drive a negative
$$\lambda_i$$ to $$0$$. At the start of any iteration, we have the following
invariants:

- $$$$ $$M x + q = \lambda$$ 
- $$$$ $$x_B > 0, \lambda_B = 0$$
- $$$$ $$x_N = 0, \lambda_N > 0$$

If we find an index $$i$$ such that $$\lambda_i < 0$$, then this index is
neither in $$B$$ nor $$N$$. We now include $$x_i$$ in the system variables and
add a new constraint to the system:

$$\mat{M_{BB} & M_{Bi} \\ M_{iB} & M_{ii}} \mat{x_B \\ x_i} + \mat{q_B \\ q_i}$$

From the first line of the system, we see that the solution of the constrained
system should satisfy:

$$x_B = M_{BB}^{-1} q_B + M_{BB}^{-1} M_{Bi} x_i$$

where the first term of the sum is the current value for $$x_B$$. So the idea of
Dantig-Cottle is increase $$x_i$$ continuously until either the constraint is
met, or there is a sign change in $$x_B, \lambda_N$$. More precisely, we'll move
from $$x_B$$ in direction $$M_{BB}^{-1} M_{Bi}$$ until one of the following
happens:

- some $$x_B$$ goes negative
- some $$\lambda_N = M_{NB} x_B + q_N$$ goes negative
- the maximum step length is reached, making $$i$$ a basic variable

At each sign change, the sets $$B, N$$ are updated and a new constraint
direction is computed.

## Implementation Notes

The main challenge is to implement subsystem factorization efficiently. A simple
yet effective approach is to maintain and update a Cholesky factorization of
$$M_{BB}$$ as the algorithm progresses. When some index enters/exits the basic
variables, only the rows of the Cholesky factor that come *after* it need to be
updated.

## TODO convergence theory

- no zero-size steps

# Lemke

# Projected Gradient

# Van Bokhoven

Perhaps surprisingly, there exists an explicit formula for solution of the
LCP. Of course, there is a catch: the formula uses an exponential number of
operations in the size of the problem $$n$$.

Let us split the problem using a $$n-1$$-dimensional problem:

$$\mat{M_{11} & M_{1\alpha} \\ M_{\alpha 1} & M_{\alpha \alpha}} \mat{x_1 \\
x_\alpha} + \mat{q_1 \\ q_\alpha} = \mat{\lambda_1 \\ \lambda_\alpha}$$

Now, let us assume we found a solution $$\block{\tilde{x}, \tilde{\lambda}}$$ of
the $$n-1$$-dimensional LCP $$\block{M_{\alpha\alpha}, q_\alpha}$$, we are faced
with the following two cases:

- either $$M_{1\alpha} \tilde{x} + q_1 \geq 0$$, in which case $$\block{0,
  \tilde{x}}$$ is a solution of the $$n$$-dimensional problem with $$\lambda_1 =
  M_{1\alpha} \tilde{x} + q_1$$
- or $$M_{1\alpha} \tilde{x} + q_1 < 0$$, in which case we must have $$x_1 >
  0$$. Otherwise $$x_1 = 0$$ and $$x_\alpha = \tilde{x}$$ which cannot be a
  solution if $$M_{1\alpha} \tilde{x} + q_1 < 0$$. Therefore $$x_1 > 0$$ or,
  equivalently, $$\lambda_1 = 0$$

We see that in any case, $$\lambda_1 = \block{M_{1\alpha} \tilde{x} +
q_1}^+$$. This means we can compute $$\lambda_1$$ solely from the solution of an
$$n-1$$-dimensional problem. This immediately gives rise to an (exponential)
algorithm: for each coordinate $$i$$, form and solve the corresponding $$n-1$$
problem obtained by removing coordinate $$i$$, then obtain $$\lambda_i$$ from
it. Once $$\lambda$$ is obtained, invert $$M$$ to get $$x$$. While completely
untractable for large $$n$$ this provides closed-form formula when $$n$$ is
small, for instance for $$n=2$$:

$$
\begin{aligned}
\lambda_1 &= \block{q_1 + M_{12} \block{-\frac{q_2}{M_{22}}}^{+}}^{+} \\
\lambda_2 &= \block{q_2 + M_{21} \block{-\frac{q_1}{M_{11}}}^{+}}^{+} \\
\end{aligned}
$$

For the general case, a less overkill algorithm can be derived: when
$$M_{1\alpha} \tilde{x} + q_1 < 0$$ we know that $$\lambda_1 = 0$$ which
provides a linear constraint between $$x_1$$ and $$x_\alpha$$. So we may simply
pivot $$x_1$$ away and solve a *single* additional $$n-1$$ dimensional LCP with
the Schur complement to get the solution:

$$
\begin{aligned}
x_1 &= -M_{11}^{-1}\block{q_1 + M_{1\alpha} x_\alpha} \\
M_{\alpha1} x_1 &+ M_{\alpha\alpha} x_\alpha + q_\alpha = \lambda_\alpha \\
\end{aligned}
$$

Substituting the first line into the second yields the following LCP:

$$\block{M_{\alpha\alpha} - M_{\alpha1} M_{11}^{-1} M_{1\alpha}} x_\alpha + q_\alpha - M_{\alpha1}M_{11}^{-1}q_1 = \lambda_\alpha$$

which can be solved recursively to get $$x_\alpha$$, from which $$x_1$$ can be
obtained. The runtime is still prohibitive, but this can work when then number
of active positivity constraints is known to be large.


# TODO P-matrices

- all principal minors positive
- includes positive definite matrices
- LCP has a unique solution iff M is a P-matrix
- properly closed by schur complements

# Lower-Hessenberg P-matrices

*(adapted from [https://arxiv.org/pdf/1112.0217](https://arxiv.org/pdf/1112.0217))*

When matrix $$M$$ is both lower Hessenberg and a P-matrix, the active set has a
special structure that can be exploited to speed up computations. As usual, we
partition the LCP variables $$x$$ into *basic* $$x_B$$ and non-basic $$x_N$$
variables, and introduce matrix $$\bar{M}$$ obtained by replacing non-basic
columns with that of $$-I$$. Modulo reordering, this gives the following system:

$$\underbrace{\mat{M_{BB} & 0 \\ M_{NB} & -I}}_{\bar{M}} \mat{z_B \\ z_N} + \mat{q_B \\ q_N} = 0$$

so that $$x = \block{z_B, 0}$$ is the primal solution and $$w = \block{0, z_N}$$
is the dual solution. Note that $$\bar{M}$$ is also lower-Hessenberg. Without
losing generality, we may rewrite $$B$$ as:

$$B = P \cup \underbrace{\left\{l + 2, l + 3, ... k\right\}}_S$$

where the prefix $$P \subseteq [1, \ldots, l] =: [l]$$, and $$-2 \leq l \leq k -
1$$. In other words, $$B$$ must end with a (possibly empty) set of *consecutive*
basic indices separated from the prefix $$P$$ by some non-basic index $$l +
1$$. Note that the bounds on $$l$$ correspond respectively to a full and empty
suffix. 

Now, since $$\bar{M}$$ is lower-Hessenberg and $$l+1$$ is non-basic, it is easy
to see that the top-right submatrix $$\bar{M}_{[l]S}$$ is zero. This means that:

- The solution prefix $$x_{[l]}$$ must be a solution of the (top-left) subsystem
  $$\block{\bar{M}_{[l][l]}, q_{[l]}}$$
- The prefix set must be the basic set of the top-left subsystem

Since the top-left subsystem is also lower-Hessenberg, this property carries
over recursively. Therefore if we can maintain for each index $$i$$:

- its prefix sub-system index $$l_i$$
- the solution suffix $$x_{S_i}$$

then we can chain solution prefixes and compute solution suffixes for
increasingly large suffixes until we find a solution. 

When the initial matrix is tridiagonal and symmetric (hence positive definite),
one can efficiently update [incremental
factorizations](cholesky#incremental-tridiagonal-factorization) of the suffix
system to obtain the solution suffix.

