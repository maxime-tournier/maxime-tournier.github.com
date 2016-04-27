---
title: Principal Component Analysis
categories: [math]
---

A geometric take on PCA.

# First Component 

We consider a $$n$$-dimensional Euclidean space $$E$$ with inner
product matrix $$M$$, and $$m$$ samples $$x_i \in E$$, arranged in
rows in matrix $$X$$. The first principal component $$w_1$$ maximizes
the *projected* variance of the data:

$$
\begin{align}
w_1 &= \argmax{\norm{w}_M = 1} \quad \sum_i \block{x_i^TMw}^2 \\
&= \argmax{\norm{w}_M = 1} \quad \norm{XMw}^2 \\
&= \argmax{\norm{w}_M = 1} \quad w^T MX^TXM w \\
\end{align}
$$

The first-order stationary conditions give:

$$ \exists \lambda \in \RR,\quad MX^TXMw = \lambda Mw, \quad \norm{w}_M = 1$$

In other words, $$w$$ is an eigenvector of $$X^TXM$$ and since $$w^T
MX^TXM w = \lambda w^T M w = \lambda$$ is maximized, $$w$$ is the
eigenvector corresponding to the largest eigenvalue.


# Eigen Decomposition

More precisely, let $$M = L L^T$$ be the Cholesky decomposition of
$$M$$, and $$L^T X^T X L = USU^T$$ the eigendecomposition of $$L^T
X^TX L$$, then we have the following diagonalization:

$$X^TXM = X^T X L L^T = L^{-T}USU^TL^T = P S \inv{P}$$

for an eigenvector basis $$P = L^{-T}U$$. Note that the basis $$P$$
is $$M$$-orthonormal:

$$ P^T M P = I $$

so that its inverse is the $$M$$-orthogonal projection on $$P$$:

$$\inv{P} = P^T M$$

Finally, the covariance in basis $$P$$ is diagonal:

$$ \inv{P} X^T X P^{-T} = U^T L^T X^T X L U = S $$

# Induction

Letting $$X_1 = X$$, subsequent components are found by projecting out
the current component:

$$X_k = X_{k-1} - X_{k-1}Mw_{k-1} w_{k-1}^T$$

$$w_k = \argmax{\norm{w}_M = 1} \quad \quad w^T MX_{k}^TX_{k}M w$$

For $$w_2$$, simple derivations show that:

$$X_2^T X_2 M = X X^T M - s_1 p_1 p_1^T M$$

where $$p_1 = w_1$$ is the eigenvector associated to the largest
eigenvalue $$s_1$$. One can also show that:

$$s_1 p_1 p_1^T M = P \mat{s_1 & \\ & 0 } \inv{P}$$

so that $$X_2^T X_2 M = P S_2 \inv{P}$$, where $$S_2$$ replaces the
largest eigenvalues in $$S$$ by zero. Hence, the second principal
component is the eigenvector associated with the second largest
eigenvalue.

A similar derivation shows by induction that the principal components
are the eigenvectors corresponding to decreasing eigenvalues.

# Summary

PCA computes an $$M$$-orthogonal matrix $$P$$ in which the samples
have a diagonal covariance matrix:

$$ \inv{P} X^T X P^{-T} = S $$

It is found by diagonalizing $$X^T X M = P S \inv{P}$$, or using $$P =
L^{-T}U$$, where $$M = L L^T$$ and $$L^T X^T X L = USU^T$$.

