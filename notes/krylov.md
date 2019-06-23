---
title: Krylov Methods
categories: [math]
---

# Krylov Subspace

$$\Krylov_k(A, b) = \Span{b, Ab, \ldots, A^k b}$$

TODO dimension

# Tridiagonalization

Let us construct an orthogonal basis for the family $$\left\{b, Ab,
\ldots, A^k b\right\}$$ using the Graham-Schmidt process, and arrange
the basis vectors $$q_0, q_1, \dots, q_k$$ by column in a matrix
$$Q_k$$. If the matrix $$A$$ is positive definite, then $$Q_k$$
satisifies:

$$Q_k^T A Q_k = T_k$$

where $$T_k$$ is tridiagonal. To see this, consider $$T_{ij}$$ for
$$i>j + 1$$.  Clearly, $$T_{ij} = q_i^T A q_j$$ and $$Aq_j \in
\Krylov_{j+1}(A, b)$$, of which $$Q_{j+1}$$'s columns form an
orthogonal basis. But since $$Q_i$$'s columns are also orthogonal,
$$q_i$$ is orthogonal to every column of $$Q_{j+1}$$. Therefore
$$q_i^T A q_j = 0$$ and $$T$$ is tridiagonal. Now, an orthogonal basis
$$Q \in O(n)$$ for the complete space $$\Krylov_n(A, b)$$ satisfies:

$$Q^T A Q = T$$

or, equivalently:

$$AQ = QT$$

# Lanczos Method

The Lanczos Method is a simple iterative procedure to compute the
tridiagonal factorization columnwise. Let us rewrite $$T$$ as:

$$T = \mat{\alpha_1 & \beta_1 &     \\
           \beta_1 & \alpha_2 & \beta_2 \\
                 & \beta_2 &\alpha_3 & \beta_3 \\
                 & & \ddots  & \ddots & \ddots \\
                 & & & \beta_{n_1} & \alpha_n  }
$$

The $$k$$-th column satisfies:

$$Aq_k = Q t_k$$

in other words:

$$A q_k = \beta_{k-1} q_{k-1} + \alpha_k q_k + \beta_k q_{k+1}$$

where $$\alpha_k = q_k^T A q_k$$. This provides a way of computing
$$q_{k+1}$$ from $$q_k, q_{k-1}$$. Let us define $$\beta_0 q_0 = 0$$,
we obtain the following algorithm:

$$\beta_{k+1} q_{k+1} = A q_k - \alpha_k q_k - \beta_{k-1} q_{k-1}$$

where $$\beta_{k+1}$$ is chosen so that $$\norm{q_{k+1}} = 1$$.


## Gradient Methods

Iterative methods for solving linear systems $$Ax=b$$ usually minimize
some energy function $$E(x)$$ by taking steps along the gradient of
$$E$$. For $$E(x) = \half x^T A x - b^T x$$, the update rule is of the
form:

$$x_{k+1} = x_k - \alpha_k \block{A x_k - b}$$

One can easily check that $$x_k \in \Krylov_k \Rightarrow x_{k+1} \in
\Krylov_{k+1}$$, and Krylov methods generalize gradient methods by
projecting the solution onto nested Krylov subspaces. If one can build
a sufficiently nice basis for these subspaces, one can hope to find a
better approximation of the initial problem as we iterate. For
instance, the [Congugate Gradient](cg.html) method satisfies:

$$x_k = \argmin{x \in \Krylov_k}\ \half x^T A x - b^T x$$

while the Minimum Residual method satisfies:

$$x_k = \argmin{x \in \Krylov_k}\ \norm{A x - b}^2$$



