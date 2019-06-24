---
title: Krylov Methods
categories: [math]
---

{% include toc.md %}


# Krylov Subspaces

$$\Krylov_k(A, b) = \Span{b, Ab, A^2b, \ldots, A^k b}$$

The dimension of $$\Krylov_k(A, b)$$ is related to the the minimum polynomial of
$$b$$ with respect to $$A$$, that is the lowest-degree non-zero polynomial $$p$$
such that $$p(A)v = 0$$.

# Tridiagonalization

Let us construct an orthogonal basis for the family $$\left\{b, Ab,
\ldots, A^k b\right\}$$ using the Graham-Schmidt process, and arrange
the basis vectors $$q_0, q_1, \dots, q_k$$ by column in a matrix
$$Q_k$$. If the matrix $$A$$ is symmetric, then $$Q_k$$
satisifies:

$$Q_k^T A Q_k = T_k$$

where $$T_k$$ is tridiagonal. To see this, consider $$T_{ij}$$ for
$$i>j + 1$$.  Clearly, $$T_{ij} = q_i^T A q_j$$ and $$Aq_j \in
\Krylov_{j+1}(A, b)$$, of which $$Q_{j+1}$$'s columns form an
orthogonal basis. But since $$Q_i$$'s columns are also orthogonal,
$$q_i$$ is orthogonal to every column of $$Q_{j+1}$$. Therefore
$$q_i^T A q_j = 0$$ and $$T$$ is lower-Hessenberg. But since $$T$$ is
also symmetric, $$T$$ is tridiagonal. Now, an orthogonal basis $$Q \in
O(n)$$ for the complete space $$\Krylov_n(A, b)$$ satisfies:

$$Q^T A Q = T$$

or, equivalently:

$$AQ = QT$$

By the same token, if $$M$$ is a non-standard inner product and $$A$$ is
auto-adjoint for $$M$$ (i.e. $$MA$$ is symmetric), then we have the following
tridiagonalization:

$$Q_k^T MA Q_k = T_k$$

where $$T_k$$ is tridiagonal. The complete factorization satisfies:

$$Q^T MA Q = T$$

where $$Q^T M Q = I$$, and we again obtain:

$$A Q = Q T$$

# The Lanczos Method

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

$$\beta_k q_{k+1} = A q_k - \alpha_k q_k - \beta_{k-1} q_{k-1}$$

where $$\beta_k$$ is chosen so that $$\norm{q_{k+1}} =
1$$. Likewise, using a non-standard inner product $$M$$ still yields:

$$\beta_k q_{k+1} = A q_k - \alpha_k q_k - \beta_{k-1} q_{k-1}$$

except this time:

$$\alpha_k = q_k^T MA q_k$$

and $$\beta_k$$ is chosen so that $$\norm{q_{k+1}}_M = 1$$.


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

while the MINRES[^minres] method satisfies:

$$x_k = \argmin{x \in \Krylov_k}\ \norm{A x - b}^2$$

## Conjugate Gradients

$$x_k = \argmin{x \in \Krylov_k}\ \half x^T A x - b^T x$$

Since we constructed an orthogonal basis for $$\Krylov_k$$, let us use
it by rewriting $$x_k = Q_k y_k$$. We obtain the following problem:

$$y_k = \argmin{y}\ \half y^T Q_k^T A Q_k y - b^T Q_k y$$

That is:

$$y_k = \argmin{y}\ \half y^T T_k y - \underbrace{b^T Q_k} y$$

or equivalently: $$T_k y_k = c_k$$. Now, $$Q_k b = \beta_1 e_1$$ and since
$$T_k$$ is tridiagonal, we can maintain an [incremental $$LDL^T$$
factorization](cholesky.html#incremental-tridiagonal-factorization) to
efficiently update $$y$$'s coordinates.



## MINRES

$$y_k = \argmin{y}\ \norm{Q_k^T A Q_k y - Q_k^Tb}^2$$

$$y_k = \argmin{y}\ \norm{T_k y - Q_k^Tb}^2$$

Here, an incremental QR factorization of $$T_k$$ is maintained. See
Choi's thesis[^choi06] for more details.

# Notes & References

[^minres]: C. C. Paige and M. A. Saunders (1975). [Solution of sparse indefinite systems of linear equations](https://web.stanford.edu/group/SOL/software/minres/), SIAM J. Numerical Analysis 12, 617-629.

[^choi06]: S.-C. Choi (2006). [Iterative Methods for Singular Linear Equations and Least-Squares Problems](https://web.stanford.edu/group/SOL/dissertations/sou-cheng-choi-thesis.pdf), PhD thesis, Stanford University.

