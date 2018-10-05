---
title: Principal Component Analysis
categories: [math]
---

A geometric take on [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis).

{% include toc.md %}

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

PCA computes an $$M$$-orthonormal basis $$P$$ in which the samples
have a diagonal covariance matrix:

$$ \inv{P} X^T X P^{-T} = S $$

It is found by diagonalizing $$X^T X M = P S \inv{P}$$, or using $$P =
L^{-T}U$$, where $$M = L L^T$$ and $$L^T X^T X L = USU^T$$.

# Symmetries

We now examine how symmetries in the data set influence the PCA.

## Reflections

Let us assume our data set $$\Omega$$ has a plane symmetry, *i.e.*
$$\Omega$$ is invariant by a plane reflection:

$$\Omega = G(\Omega)$$

More precisely, $$G$$ implements the reflection by some unit normal
vector $$n$$ by flipping the normal component:

$$G = \underbrace{\block{I - nn^T}}_{\text{tangent}} - \underbrace{n n^T}_{\text{normal}} = \block{I - 2nn^T}$$

Some immediate properties of $$G$$:

- $$G^T = G$$,
- $$G n = -n$$.

Since the data set is invariant by $$G$$, the covariance matrix $$C =
X^T X$$ is left unchanged when applying our reflection $$G$$ to the
data (we just process the same points in a different order when
computing $$C = \sum_i x_i^T x_i$$), so that the transformed
covariance satisfies:

$$G C G^T = C$$

We immediately get:

$$ Cn = G C G^T n = G C G n = - G C n$$

from which we obtain, replacing $$G$$ with its definition:

$$ Cn = 2 nn^TCn - Cn$$

or, equivalently:

$$ Cn = n (n^T C n)$$

and $$n$$ is an eigenvector of the covariance matrix, with eigenvalue
equal to the projected variance on $$n$$. 

## Rotations

Now, let us assume $$\Omega$$ has a rotation symmetry: $$R(\Omega) =
\Omega$$ for some rotation matrix $$R$$. Then the rotated covariance
satisfies:

$$RCR^T = C$$

Let $$u$$ be an eigenvector with eigenvalue $$\lambda \geq 0$$, *i.e.* $$Cu =
\lambda u$$, then $$CRv = \lambda Rv$$ for $$R v = u$$, which means:

$$RCR^T R v = \lambda Rv$$

and $$v = R^T u$$ is also an eigenvector with eigenvalue
$$\lambda$$. In other words: the stable subspaces associated with a
given eigenvalue are also stable by $$R$$. This yields the following
requirements:

- *(if the dimension is odd)* one eigenvector of $$C$$ is the rotation
  axis of $$R$$
- $$C$$ has (real) eigenvalues of multiplicity 2 whose eigenspaces are
  the 2-dimensional stable subspaces of $$R$$ corresponding to its
  complex conjugate eigenvalues
  
## Non-standard Metric

The above results naturally extend to a non-standard metric, provided
we consider $$M$$-orthogonal rotations and reflections. (TODO show it)

# Pulling back Principal Components

Let us now suppose that we have a set of samples $$X$$ in some output space
$$F$$ and a linear mapping $$P: w \mapsto x \in F$$ from some input space
$$E$$. We now look for the direction in the input space $$E$$ that maximizes the
projected variance in the output space $$F$$:

$$\argmax{\norm{w}=1} \quad \sum_i \block{x_i^T Pw}^2 = \norm{XPw}^2 = w^T P^T X^T X P w$$

We see that this amounts to diagonalizing the covariance matrix $$P^T X^T X P$$,
which is the pullback of output covariance $$X^T X$$ by $$P$$.

# Dual Metric

So far we only considered a metric $$M$$ in the *feature* space, *i.e.* the
metric used to compare features for two given individuals. However, it is also
natural consider the dual problem of comparing individuals for two given
features, for instance in order to give more weight to some individuals than to
others or even give different weights to arbitraty linear subspaces of
individuals. Let us recall the projection equation:

$$\argmax{\norm{w}=1} \quad \sum_i \block{x_i^T w}^2 = \norm{Xw}^2$$

Here we see that the canonical Euclidean norm is used in the right-hand side
term, but any Euclidean norm could be used instead: the projection of each
sample $$x_i$$ onto feature $$w$$ gives each individual a *score* (how similar
to feature $$w$$ the individual is), and we may choose to measure scores using a
different metric $$W$$:

$$\argmax{\norm{w}=1} \quad \sum_i \norm{Xw}_W^2$$

Using such a dual metric yields a correlation matrix $$X^TWX$$. In particular,
choosing $$W=XX^T$$ does not change the eigenvector basis (but it does square
the eigenvalues), thus producing the same principal components. More precisely,
the metric $$W=XX^T$$ takes a score, then produces a feature by weighting all
individuals by their score, and finally measures the resulting feature using the
(here implicit) feature metric.

But let's say we also have another set of features $$Y$$ over the same
individuals: we can then use the metric $$YY^T$$ over the same individuals to
weight scores when looking for principal components. This means that a
$$X$$-feature must produce a score that agrees with $$Y$$-features to be
selected as a principal component. This allows us to *correlate* $$X$$-features
to $$Y$$-features: the obtained principal components in $$X$$ will have to be
representative of both $$X$$ and $$Y$$.

## Partial Least Squares

Some good introductory material can be found in [^0][^3]. The whole field feels
like a mess, with at least three main versions competing with each other: 

- Bookstein PLS: obtained by an SVD of $$X^TY$$, as suggested by the above
  analysis
- PLS1, PLS2: start with first eigenvector of $$\mathrm{cov}(X^TY, X^TY)$$, then
  various deflation schemes (removing contributions of principal components)
- Consistency issues[^1][^2], use bidiagonalization formulation of PLSR
  instead

The SciKit page
on
[cross decomposition](http://scikit-learn.org/stable/modules/cross_decomposition.html#cross-decomposition) is
also a good start.

# Notes & References

[^0]: http://vision.cse.psu.edu/seminars/talks/PLSpresentation.pdf
[^1]: https://onlinelibrary.wiley.com/doi/pdf/10.1002/cem.1067
[^2]: https://onlinelibrary.wiley.com/doi/abs/10.1002/cem.1181
[^3]: http://users.cecs.anu.edu.au/~kee/pls.pdf







