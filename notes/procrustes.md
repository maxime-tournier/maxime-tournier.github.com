---
title: Procrustes
categories: [math]
---

Quick notes about (semi-) rigid point registration.

# Problem 

Given two sets of $$n$$ 3-dimensional points $$\block{x_i}_i, \block{y_i}_i \in
\RR^3$$, we want to find the best rigid registration *i.e.* a rigid transform
$$\mat{R & t \\ 0 & 1} \in SE(3)$$ that minimizes the squared norm error:

$$\min_{R \in SO(3), t \in \RR^3} \quad \sum_i \norm{Rx_i + t - y_i}^2$$

# Analysis

If we let $$X, Y \in \mathcal{M}_{3, n}$$ be the matrices of column vectors
$$x_i, y_i$$, we get the following matrix form:

$$\min_{R \in SO(3), t \in \RR^3} \quad \underbrace{\norm{RX + t 1^T - Y}}_{E(R, t)}{}^2$$

Expanding the Frobenius norm, we get:

$$
\begin{aligned}
E(R, t) &= \tr\block{\block{RX + t1^T - Y}^T\block{RX + t1^T - Y}} \\
&= \tr\block{X^TX} + 2 \tr\block{X^TR^T\block{t1^T - Y}} + \tr\block{\block{t1^T - Y}^T\block{t1^T - Y}} \\
\end{aligned}
$$

Dropping the constant term $$\tr\block{X^TX}$$, let us focus on the second term:

$$\begin{aligned}
\tr\block{X^TR^T\block{t1^T - Y}} &= \tr\block{X^TR^Tt1^T} - \tr\block{X^TR^TY} \\
&= \tr\block{1^TX^TR^Tt} - \tr\block{\block{YX^T}^T R}
\end{aligned}
$$

We notice that $$X 1 = \sum_i x_i$$ so if we choose coordinates so that $$X 1 =
0$$ (which is always possible), then the first term disappears and we're left
with a linear form in $$R$$. The total energy is then reduced to the following
*separable* energy:

$$E(R, t) = -\tr\block{\block{YX^T}^T R} + \tr\block{\block{t1^T - Y}^T\block{t1^T - Y}}$$

# Solving

The translation term corresponds to the following linear least-squares in $$t$$:

$$\min_t \norm{1 t^T - Y^T}^2$$

whose solution is given by the usual normal equations:

$$1^T1t^T = 1^TY^T$$

That is: 

$$n t = \sum_i y_i$$

The rotation term is a linear form to be optimized on $$SO(3))$$:

$$\min_{R \in SO(3)} -\tr\block{\block{YX^T}^T R}$$

This is the same problem as the matrix projection onto $$SO(3)$$:

$$\min_{R \in SO(3)} \norm{F - R}^2 = \min_{R \in SO(3)} -\tr\block{F^TR}$$

with $$F = YX^T$$. Using the Singular Value Decomposition of $$F = USV^T$$
(flipping singular value signs to get both $$U, V \in SO(3)$$), we obtain the
following equivalent problem:

$$\min_{Q \in SO(3)} -\tr\block{S^TQ}$$

where $$Q = V^T R U$$. From there, two options:

- express the gradient on $$SO(3)$$ tangent vectors and make this gradient zero,
- introduce Lagrange multipliers for the constraint $$Q \in SO(3)$$ and solve the KKT system.

### First Option: $$SO(3)$$ tangent vectors

Using [left trivialization](lie-groups#translations) (body-fixed coordinates) we
get $$\dd Q = Q \omega$$ where $$\omega \in \alg{so(3)}$$ is skew-symmetric,
giving the following first-order conditions:

$$\forall \omega \in \alg{so(3)}: \tr\block{S^TQ\omega} = 0$$

which implies that $$Q^TS$$ must be symmetric (*i.e.* orthogonal to all
skew-symmetric matrices).

### Second Option: Lagrange multipliers

Introduce $$\lambda \in \mathcal{M}_{3,3}$$ and compute their pullback by the
constraint equation:

$$Q^T Q = I$$

Differentiating the contraint equation gives:

$$\dd Q^T Q + Q^T \dd Q = 0$$

Pullback the multipliers:

$$\begin{aligned}
\tr\block{\lambda^T \block{\dd Q^T Q + Q^T \dd Q}} &=
\tr\block{\lambda Q^T \dd Q} + \tr\block{\lambda^T Q^T \dd  Q} \\
&= \tr\block{\block{Q\block{\lambda + \lambda^T}}^T \dd Q} \\
\end{aligned}
$$

whose inner product left-hand side gives the multiplier pullback. The objective
function gradient is just $$S$$, therefore the first-order conditions are:

$$S = Q\block{\lambda + \lambda^T}$$

or, equivalently, that $$Q^TS$$ must be symmetric (as before).

### Solution

It turns out that the only rotation making $$Q^TS$$ symmetric is $$Q = I$$,
which means that $$R = VU^T$$. See also the page on
[quaternions](quaternions#point-registration) for an alternate algorithm that
finds the solution orientation directly as a quaternion.


# Summary

Assuming $$\sum_i x_i = 0$$, the problem becomes separable and its solution is
given by:

$$
\begin{aligned}
R &= VU^T \\
t &= \frac{1}{n} \sum_i y_i\\
\end{aligned}
$$

# TODO 

- introduce uniform scale?
- introduce non-uniform scale?
- introduce point equality constraints?




