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



