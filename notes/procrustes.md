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

Dropping the constant term $$\tr\block{X^TX}$$, we focus on the second term:

$$\begin{aligned}
\tr\block{X^TR^T\block{t1^T - Y}} &= \tr\block{X^TR^Tt1^T} - \tr\block{X^TR^TY} \\
&= \tr\block{1^TX^TR^Tt} - \tr\block{\block{YX^T} R}
\end{aligned}
$$

We notice that $$X 1 = \sum_i x_i$$ so if we choose coordinates so that $$X 1 =
0$$ (which is always possible), then the first term disappears and we're left
with a linear form in $$R$$.



