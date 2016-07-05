---
title: Lower-Diagonal Solves
categories: [math]
---

Let $$M = L + D + L^T$$ be a symmetric matrix with positive diagonal
$$D$$. We look at the various in-place lower-diagonal system solves
found *e.g.* in Gauss-Seidel method.

## Solving $$\block{L + D}x = y$$ 

This gives:

$$
\sum_{j < i} M_{ij} x_j + M_{ii} x_i = y_i
$$

Thus, solving incrementally for $$x_i$$ gives:

$$ x_i = \frac{1}{M_{ii}} \block{y_i - \sum_{j < i} M_{ij} x_j} $$

which can also be performed in-place on vector $$y$$:

$$ y_i := \frac{1}{M_{ii}} \block{y_i - \sum_{j < i} M_{ij} y_j}$$

## Solving $$\block{L + D}x = -L^T y$$ 

This gives:

$$
\sum_{j < i} M_{ij} x_j + M_{ii} x_i = -\sum_{j > i} M_{ij} y_j
$$

Thus, solving incrementally for $$x_i$$ gives:

$$ x_i = \frac{1}{M_{ii}} \block{- \sum_{j > i} M_{ij} y_j - \sum_{j < i} M_{ij} x_j} $$

which can also be performed in-place on vector $$y$$:

$$ y_i := \frac{1}{M_{ii}} \block{- \sum_{j > i} M_{ij} y_j - \sum_{j < i} M_{ij} y_j}$$

Alternatively:

$$ y_i := y_i - \frac{1}{M_{ii}} \sum_j M_{ij} y_j $$

## Solving $$\block{L + D}x = -L^T y - z$$ 

Mixing the above gives, for all $$i$$:

$$
\sum_{j < i} M_{ij} x_j + M_{ii} x_i = -\sum_{j > i} M_{ij} y_j - z_i
$$

Thus, solving incrementally for $$x_i$$ gives:

$$ x_i = \frac{1}{M_{ii}} \block{- \sum_{j > i} M_{ij} y_j - \sum_{j < i} M_{ij} x_j - z_i} $$

which can also be performed in-place on vector $$y$$:

$$ y_i := \frac{1}{M_{ii}} \block{- \sum_{j > i} M_{ij} y_j - \sum_{j < i} M_{ij} y_j - z_i}$$

Alternatively:

$$ y_i := y_i - \frac{1}{M_{ii}} \block{z_i + \sum_j M_{ij} y_j} $$

This one is used in Gauss-Seidel method:

$$
\begin{align}
x_{k+1} &= x_k - \block{L + D}^{-1}\block{Mx_k + q} \\
	&= - \block{L + D}^{-1} \block{L^T x_k + q} \\
\end{align}
$$


