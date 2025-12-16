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

Let us construct an orthogonal basis for the family $$\left\{b, Ab, \ldots, A^k
b\right\}$$ using the Graham-Schmidt process, and arrange the basis vectors
$$q_0, q_1, \dots, q_k$$ by column in a matrix $$Q_k$$. We start with $$q_0 =
\frac{b}{\norm{b}}$$, then orthogonalize $$A q_0$$ against $$q_0$$ to get
$$q_1$$, then orthogonalize $$A q_1$$ against $$q_0, q_1$$ to get $$q_2$$, and
so on. It is easy to show by recurrence that the $$q_k$$ span $$\Krylov_k(A,
b)$$, to which the subsequent $$\block{q_{k+i}}_{i > 0}$$ are all orthogonal.

Let us consider two vectors $$q_i$$ and $$q_j$$ obtained by this process and
assume $$i > j$$, there are two cases:

- when $$i > j + 1$$, then $$A q_j \in \Krylov_{j+1}(A, b)$$ to which $$q_i$$ is
  orthogonal. Therefore $$q_i^T A q_j = 0$$
- when $$i = j + 1$$, then either the iteration stops due to $$A q_j \in
  \Krylov_j(A, b)$$, causing $$q_i = 0$$, or $$q_i^T A q_j$$ is non-zero

In matrix terms, this means that $$H_k = Q_k^TAQ_k$$ is (strictly)
lower-Hessenberg. Now since $$A$$ is symmetric, $$H_k$$ must also be symmetric,
which means it is also upper-Hessenberg, therefore $$Q_k^TAQ_k = T_k$$ is
tridiagonal. This means that we just need to orthogonalize $$Aq_i$$ against the
*two* last $$q_i, q_{i-1}$$ instead of the complete sequence of predecessors,
which is a huge computational win (assuming exact arithmetic).

# The Lanczos Method

The Lanczos Method is a simple iterative procedure to compute the above
tridiagonal factorization columnwise. Let us rewrite $$T_k$$ as:

$$T_k = \mat{\alpha_1 & \beta_2 &     \\
           \beta_2 & \alpha_2 & \beta_3 \\
                 & \beta_3 &\alpha_3 & \beta_4 \\
                 & & \ddots  & \ddots & \ddots \\
                 & & & \beta_{k} & \alpha_k  }
$$

As we've seen, the orthogonalization process only requires the last two vectors:

$$\gamma_{k + 1} q_{k+1} = A q_k - \underbrace{q_k^TAq_k}_{\alpha_k} q_k -
\underbrace{q_k^TA q_{k-1}}_{\beta_k} q_{k - 1}$$

where $$\gamma_{k + 1}$$ normalizes $$q_{k+1}$$. Taking the dot product with
$$q_{k+1}$$ shows that $$\gamma_{k + 1} = \beta_{k + 1}$$ so that the iteration
can be rewritten as:

$$\beta_{k + 1} q_{k+1} = A q_k - \alpha_k q_k - \beta_k q_{k - 1}$$

where $$\beta_{k + 1}$$ normalizes $$q_{k+1}$$ and $$\alpha_k = q_k^TAq_k$$. The
iteration starts with $$\beta_1 q_1 = b$$ and $$q_0 = 0$$.

## Gradient Methods

Iterative methods for solving linear systems $$Ax=b$$ usually minimize
some energy function $$E(x)$$ by taking steps along the gradient of
$$E$$. For $$E(x) = \half x^T A x - b^T x$$, the update rule is of the
form:

$$x_{k+1} = x_k - \alpha_k \block{A x_k - b}$$

One can easily check that $$x_k \in \Krylov_k \Rightarrow x_{k+1} \in
\Krylov_{k+1}$$, and Krylov methods seek to accelerate gradient methods by
directly projecting the solution onto nested Krylov subspaces, using Lanczos
method to construct an orthonormal basis. For instance, the [Conjugate
Gradient](cg.html) method satisfies:

$$x_k = \argmin{x \in \Krylov_k}\ \half x^T A x - b^T x$$

while the MINRES[^minres] method satisfies:

$$x_k = \argmin{x \in \Krylov_k}\ \norm{A x - b}^2$$

## Conjugate Gradient

$$x_k = \argmin{x \in \Krylov_k}\ \half x^T A x - b^T x$$

Since we constructed an orthogonal basis for $$\Krylov_k$$, let us use
it by rewriting $$x_k = Q_k y_k$$. We obtain the following problem:

$$y_k = \argmin{y}\ \half y^T Q_k^T A Q_k y - b^T Q_k y$$

That is:

$$y_k = \argmin{y}\ \half y^T T_k y - \underbrace{b^T Q_k}_{c_k^T} y$$

or equivalently: $$T_k y_k = c_k$$. Now, $$Q_k b = \beta_1 e_1$$ and since
$$T_k$$ is tridiagonal, we can maintain an [incremental $$LDL^T$$
factorization](cholesky.html#incremental-tridiagonal-factorization) to
efficiently compute $$y_k$$'s coordinates, then update the solution vector
$$x$$. More precisely, $$T_k$$ factors as $$T_k = L_k D_k L_k^T$$ where $$D_k =
\diag\block{d_k}$$ is diagonal and

$$L_k = \mat{
1 & & & &  \\
l_2 & 1 &  & \\
  & l_3 & 1  & \\ 
  &  & \ddots & \ddots \\
& &  & l_{k} & 1
}
$$

is lower-diagonal with only the first sub-diagonal filled. It is possible to
update $$\block{l_k, d_k}$$ from $$\block{l_{k-1}, d_{k-1}}$$ and
$$\block{\alpha_k, \beta_l}$$, then update both the projected solution $$T_k y_k
= c_k$$ and the unprojected one $$x_k = Q_k y_k$$ efficiently (in $$O(n)$$
operations per iteration). One can show that this approach is indeed equivalent
to the usual presentation of the CG algorithm[^choi06].

## MINRES

$$y_k = \argmin{y}\ \norm{Q_k^T A Q_k y - Q_k^Tb}^2$$

which reduces to:

$$y_k = \argmin{y}\ \norm{T_k y - \beta_1 e_1}^2$$

The MINRES algorithm maintains an incremental QR factorization of $$T_k$$ as:

$$T_k = U_k R_k$$

so that we may solve:

$$y_k = \argmin{y}\ \norm{R_k y - U_k^T \beta_1 e_1}^2$$

by maintaining $$t_k = U_k^T \beta_1 e_1$$ then solving the normal equations

$$R_k^TR_k y_k = R_k^T t_k$$

which reduce to simply

$$R_k y_k = t_k$$

Since $$T_k$$ is almost triangular except for one sub-diagonal, we may use a
sequence of Givens rotations to cancel the successive subdiagonal terms
$$\beta_k$$, which produces a sparse $$R_k$$ with only two non-zero
super-diagonals. More precisely, $$T_k$$ is applied $$U_k^T = \Pi_i^{k - 1}
V_{k - i}$$ so that $$U_k T_k = R_k$$ is upper triangular and $$T_k = U_k
R_k$$. The first iteration brings

$$T_2 = \mat{\alpha_1 & \beta_2 \\ \beta_2 & \alpha_2}$$

to the form:

$$V_1 T_2 = \mat{\delta_1 & \gamma_2 \\ 0 & \eta_2}$$

Then we transform

$$T_3 = \mat{\alpha_1 & \beta_2 & \\ \beta_2 & \alpha_2 & \beta_3 \\ & \beta_3 & \alpha_3}$$

to the following successive forms:

$$V_1 T_3 = \mat{\delta_1 & \gamma_2 & \nu_2 \\ & \eta_2 & \mu_2 \\ & \beta_3 & \alpha_3}$$

$$V_2 V_1 T_3 = \mat{\delta_1 & \gamma_2 & \nu_2 \\ & \delta_2 & \gamma_3 \\ &  & \eta_3}$$

Note how applying $$V_1$$ to $$T_2$$ changes $$\beta_3$$ to $$\mu_2$$ and
introduces fill-in with a new non-zero term $$\nu_2$$. Similarly, one can show
by recurrence that once pivoted by the first $$k - 1$$ rotations, $$T_k$$ is
brought to the following form (showing only the bottom-right corner):

$$
V_{k-1} \ldots V_1 T_k = \mat{
    \delta_{k-1} & \gamma_k & \nu_k \\
    0 & \eta_k & \mu_k \\
    & \beta_{k+1} & \alpha_{k + 1} \\
}
$$

We apply a Givens rotation $$V_k$$ to cancel $$\beta_{k+1}$$:

$$V_k = \mat{c_k & -s_k \\ s_k & c_k} \mat{\eta_k \\ \beta_{k+1}} = \mat{\delta_k \\ 0}$$ 

where $$c_k^2 + s_k^2 = 1$$ and we obtain

$$\mat{\gamma_{k+1} \\ \eta_{k+1}} = V_k \mat{\mu_k \\ \alpha_{k+1}}$$

For next iteration we'll also need to compute:

$$\mat{\nu_{k+1} \\ \mu_{k+1}} = V_k \mat{0 \\ \beta_{k+1}}$$

from current rotation $$V_k$$. We still need to update $$t_k$$ and update
$$y_k$$ by one incremental triangular solve similar to the CG case. See Choi's
thesis[^choi06] for more details.

## MINRES-QLP

When $$A$$ is rank-deficient $$T_k$$ may become singular and MINRES may not able
to solve $$R_k y_k = t_k$$ due to $$\delta_k$$ being $$0$$. In such situations,
it is possible to obtain a *minimum-length* solution by maintaining a QLP
factorization of $$T_k$$ instead of a QR factorization. Again, see Choi's
thesis[^choi06] for details.

# Eigenvalue Estimation

The Lanczos method can be used to estimate the extremal eigenvalues of $$A$$ by
computing that of $$T_k$$, called the *Ritz values*. Given any
tridiagonalization $$A=QTQ^T$$, let $$x$$ be an eigenvector with eigenvalue
$$\lambda$$, we see that

$$Ax = \lambda x \iff TQx = \lambda Qx$$

In other words, $$x$$ is an eigenvector of $$A$$ for eigenvalue $$\lambda$$ if
and only if $$Qx$$ is an eigenvector of $$T$$ for the same eigenvalue. If we can
compute an eigendecomposition of $$T_k$$ efficiently, it becomes possible to
estimate $$A$$'s eigenvalues/vectors though that of $$T_k$$ as we build an
increasingly precise approximation of $$A$$ over nested Krylov subspaces. In
practice though, there's no reason the Ritz values should be ordered the same
way as $$A$$'s except for the extremal ones (TODO ref).


# Notes & References

[^minres]: C. C. Paige and M. A. Saunders (1975). [Solution of sparse indefinite systems of linear equations](https://web.stanford.edu/group/SOL/software/minres/), SIAM J. Numerical Analysis 12, 617-629.

[^choi06]: S.-C. Choi (2006). [Iterative Methods for Singular Linear Equations and Least-Squares Problems](https://web.stanford.edu/group/SOL/dissertations/sou-cheng-choi-thesis.pdf), PhD thesis, Stanford University.

