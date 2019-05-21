---
title: Iterative Methods
categories: [math]
---

Consider the following iteration for solving $$Mx + q = 0$$:

$$x_{k+1} = x_k - \inv{P}\block{Mx_k +q}$$

We look for conditions on $$P$$ such that the iteration matrix $$B =
I - \inv{P}M$$ is convergent, that is:

$$\rho(B) < 1$$

We start by splitting $$M = P - N$$ so that $$B = \inv{P}N$$, and let
$$\lambda$$ be an eigenvalue of $$B$$ with corresponding eigenvector
$$x$$, that is:

$$\inv{P}Nx = \lambda x$$

Equivalently:

$$\block{P - M}x = \lambda P x$$

We now look for sufficient conditions for:

$$|\lambda| < 1$$

## Case 1: $$M$$ is positive definite

In this case, $$x^*Mx >0$$ and we may normalize $$x$$ so that $$x^*Mx
= 1$$. $$x$$ verifies:

$$x^*\block{P - M}x = \lambda x^*P x$$

We let $$x^*Px = a + ib$$ and rewrite the above as:

$$a + ib - 1 = \lambda \block{a + ib}$$

Taking squared modulus on both sides gives:

$$(a - 1)^2 + b^2 = |\lambda|^2 \block{a^2 + b^2}$$

That is:

$$a^2 + b^2 + 1 - 2a = |\lambda|^2 \block{a^2 + b^2}$$

Our convergence condition reduces to $$1 - 2a < 0$$, which we rewrite
as:

$$x^*Mx < x^*Px + \bar{x^*Px} = x^*\block{P + P^T} x$$

A sufficient condition is then:

$$ P + P^T - M > 0 $$

## Jacobi Iteration

Given a diagonal preconditioner $$P$$ and a positive definite matrix
$$M$$, the above sufficient condition reduces to:

$$2P > M$$


## Gauss-Seidel Iteration

Given a positive definite matrix $$M = L + D + L^T$$, where $$D$$ is
diagonal, the Gauss-Seidel preconditioner is $$P = L + D$$ and the
sufficient convergence condition is:

$$L + 2D + L^T - M = D + M - M = D > 0$$

which is always true for $$M$$ positive definite.




