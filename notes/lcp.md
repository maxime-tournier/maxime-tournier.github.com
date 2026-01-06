---
title: Linear Complementarity Problem
categories: [math]
---

A few notes on Linear Complementarity (LCP) algorithms for Symmetric Positive
Definite (SPD) matrices.

# Problem

Given an $$n\times n$$ SPD matrix $$M \succ 0$$ and a vector $$q \in \RR^n$$, the goal is
to find $$x \in \RR^n$$ such that:

$$0 \leq x \ \bot\  M x + q \geq 0$$

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

# Modulus

# Dantzig-Cottle

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
it.
