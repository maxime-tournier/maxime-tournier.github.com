---
title: Modal Analysis
categories: [phys]
---

Consider a conservative $$n$$-dimensional linear mechanical system with mass
matrix $$M > 0$$ and stiffness matrix $$K \geq 0$$. Then $$\inv{M} K$$ is
self-adjoint[^1] for the inner-product $$M$$, which means there exists an
$$M$$-orthogonal basis $$B$$ of eigenvectors:

$$\inv{M}K = BS\inv{B}$$

for some diagonal matrix $$S$$. By definition of $$B$$, the mass matrix in this
basis is the identity:

$$B^T M B = I$$

In this basis, the stiffness matrix becomes diagonal:

$$B^T K B = \underbrace{B^T M B}_I S \underbrace{\inv{B} B}_I = S$$

and the linear system may be understood as a sum of *mechanically* independent
(that is, $$M$$-orthogonal) one-dimensional subsystems.

## TODO eigenfrequencies

## TODO mass-spring/mesh laplacian

## Computation 

From a Cholesky factorization $$M = LL^T$$ and the $$M$$-orthogonality of $$B$$
one can check that $$L^TB = U$$ for some orthogonal matrix $$U \in O(n)$$. To
compute either $$B$$ or $$U$$ and $$S$$ there are several options, ordered by
decrasing order of efficiency/numerical stability:

- solve the generalized eigenvalue problem $$M, K$$ to obtain $$B, S$$ and then $$U$$
- solve the regular eigenvalue problem on $$M^{-1}K$$
- notice that $$USU^T = UB^TKBU^T = L^{-1} K L^{-T}$$ since $$B^T = U^TL^{-1}$$
  and obtain $$U, S$$ by diagonalization of $$L^{-1} K L^{-T}$$

## Notes

[^1]: actually, positive semi-definite for $$M$$
