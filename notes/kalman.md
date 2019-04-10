---
title: Kalman Filter
categories: [math]
---

A geometric take on
[Kalman filtering](https://en.wikipedia.org/wiki/Kalman_filter). In
the absence of process noise, Kalman filtering simply boils down to
the Recursive Least Squares algorithm.

{% include toc.md %}


# Recursive Least Squares

Our goal is to solve the following least-squares problem: 

$$\argmin{x} \quad \norm{Ax - b}_M^2$$

where $$M$$ is positive definite, and where the size of the system will grow
over time. Assuming that $$A$$ has full row rank, the normal equations for the
above are:

$$ \underbrace{A^T M A}_K \ x = A^T M b $$

and the solution is $$x = \block{A^T M A}^{-1} A^T M b$$. Assuming we computed
the solution $$x_k$$ at step $$k$$, we now add extra rows to our
(overconstrained) system:

$$A_{k+1} x_{k+1} = \mat{A_k \\ H_{k+1}} x_{k+1}$$

$$b_{k+1} = \mat{b_k \\ z_{k+1}}$$

At this point, it is convenient to rewrite the current system in terms of the
solution update $$\delta_{k+1}$$ such that $$x_{k+1} = x_k + \delta_{k+1}$$ in
order to express the current normal equations in terms of the previous ones:

$$\delta_{k+1} = \argmin{\delta} \quad \norm{ \mat{A_k \\ H_{k+1}} \block{x_k + \delta} - \mat{b_k \\ z_{k+1}} }_{M_{k+1}}^2$$

where we assumed $$M_{k+1} = \mat{M_k & \\ & R_{k+1}}$$. Equivalently:

$$\delta_{k+1} = \argmin{\delta} \quad \norm{ \mat{A_k \\ H_{k+1}} \delta - \mat{ b_k - A_k x_k \\ z_{k+1} - H_{k+1} x_k} }_{M_{k+1}}^2$$

The normal equations become:

$$ \underbrace{\block{A_k^T M_k A_k + H_{k+1}^T R_{k+1} H_{k+1}}}_{K_{k+1}} \delta = \underbrace{A_k^T M_k\block{b_k - A_k x_k}}_{= 0} + H_{k+1}^T R_{k+1} \underbrace{\block{z_{k+1} - H_{k+1}x_k}}_{w_{k+1}}$$

where the first part in the right-hand side is zero since $$x_k$$ solves the
problem at step $$k$$. The [Woodbury
formula](https://en.wikipedia.org/wiki/Woodbury_matrix_identity) provides a
practical way to update the inverse $$C_{k+1}$$ of $$K_{k+1}$$ from the
previously computed $$C_k$$ as follows:

$$
\begin{align} 
C_{k+1} &= K_{k+1}^{-1} \\
	&= \inv{\block{K_k + H_{k+1}^T R_{k+1} H_{k+1}}} \\
	&= C_k - C_k H_{k+1}^T {\underbrace{\block{ R_{k+1}^{-1} + H_{k+1} C_k H_{k+1}^T }}_{S_{k+1}}}^{-1} H_{k+1} C_k \\
\end{align}
$$

Let us denote $$S_{k+1} = R_{k+1}^{-1} + H_{k+1} C_k H_{k+1}^T$$, now the whole
update process becomes:

$$
\begin{align} 
w_{k+1} &= z_{k+1} - H_{k+1} x_k \\ 
S_{k+1} &= R_{k+1}^{-1} + H_{k+1} C_k H_{k+1}^T \\
C_{k+1} &= C_k - C_k H_{k+1}^T S_{k+1}^{-1} H_{k+1} C_k \\
        &= \block{I - C_k H_{k+1}^T S_{k+1}^{-1} H_{k+1}} C_k \\
x_{k+1} &= x_k + C_{k+1} H_{k+1}^T R_{k+1} w_{k+1} \\		
\end{align}
$$

Alternatively, the [appendix](#appendix) shows that $$C_{k+1} H_{k+1}^T R_{k+1} =
C_k H_{k+1} S_{k+1}^{-1}$$, so that the solution update can also be
obtained as:

$$x_{k+1} = x_k + C_k H_{k+1}^T S_{k+1}^{-1} w_{k+1}$$

which may be more convenient to use in practice.

## Forgetting Factor

One can easily incorporate a geometrically decreasing weight for
previous measurements by scaling $$K_k$$ by $$0 \leq \lambda < 1$$,
which corresponds to scaling $$C_k$$ by $$\frac{1}{\lambda}$$ before
computing the next iterate.


# Non-stationary Process

Let us now assume that the state $$x$$ changes between steps according to a
linear map, for instance as a result of some dynamic process:

$$ x^{(k)} \overset{F_k}{\longmapsto} x^{(k+1)} $$

for some invertible linear mapping $$F_k$$. One can also think of $$F_k$$ as a
change of coordinates occurring after each step, and we need to express the
previous system in terms of the new coordinates $$x^{(k+1)}$$. The incremental
problem becomes:

$$ \argmin{x} \quad \norm{ \mat{A_k \inv{F}_k \\ H_{k+1}} x - \mat{b_k \\
z_{k+1}} }^2_{M_{k+1}}$$

*i.e.* we fit previous observations by reverting to the previous
coordinate system. The normal equations become:

$$ \underbrace{\block{F_k^{-T} A_k^T M A_k \inv{F}_k + H_{k+1}^T R_{k+1} H_{k+1}}}_{K_{k+1}} x = F_k^{-T} A_k^T M_k b_k + H_{k+1}^T R_{k+1} z_{k+1} $$

which in terms of the solution update $$\delta = x - F_k x_k$$ gives:

$$ K_{k+1} \delta = F_k^{-T} \underbrace{A_k^T M_k \block{b_k - A_k x_k}}_{=0} + H_{k+1}^T R_{k+1} \underbrace{\block{z_{k+1} - H_{k+1}F_kx_k}}_{w_{k+1}} $$

since once again, $$x_k$$ solves the problem at step $$k$$. Using the
Woodbury formula as before, we obtain:

$$C_{k+1} = K_{k+1}^{-1} = D_k - D_k H_{k+1}^T S_{k+1}^{-1} H_{k+1} D_k$$

where $$D_k = F_k C_k F_k^T$$ and $$S_{k+1} = R_{k+1}^{-1} + H_{k+1} D_k
H_{k+1}^T$$. The solution update is traditionally decomposed into two
prediction/update phases:

### Prediction

$$
\begin{align} 
\tilde{x}_k &= F_k x_k \\
\tilde{C}_k &= F_k C_k F_k^T
\end{align}
$$

### Update

$$
\begin{align}
w_{k+1} &= z_{k+1} - H_{k+1} \tilde{x}_k \\
S_{k+1} &= R_{k+1}^{-1} + H_{k+1} \tilde{C}_k H_{k+1}^T \\
C_{k+1} &= \tilde{C}_k - \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} H_{k+1} \tilde{C}_k \\
        &= \block{I - \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} H_{k+1}} \tilde{C}_k \\
x_{k+1} &= \tilde{x}_k + C_{k+1} H_{k+1}^T R_{k+1} w_{k+1} \\
 &= \tilde{x}_k + \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} w_{k+1} \\
\end{align}
$$

## Affine update

We now suppose that the state $$x$$ changes *affinely* as follows:

$$ x_k \longmapsto F_k x_{k+1} + u_k $$

The incremental problem becomes:

$$ \argmin{x} \quad \norm{ \mat{A_k \inv{F}_k \\ H_{k+1}} x - \mat{b_k + A_k \inv{F}_k u_k \\ z_{k+1}} }^2_{M_{k+1}} $$

Terms once again cancel each other in the normal equations, this time for the
solution update $$\delta = x - \block{F_k x_k + u_k}$$:

$$K_{k+1} \delta = H_{k+1}^T R_{k+1} \underbrace{\block{z_{k+1} - H_{k+1}\block{F_k x_k + u_k}}}_{w_{k+1}}$$

So the prediction phase simply changes to:

$$
\tilde{x}_k = F_k x_k + u_k
$$

and all the rest remains the same.

# TODO Process Noise

Not quite sure how to obtain this one, looks like some kind of dual
regularization: 

$$\argmin_{x, y} \norm{Ax - b}^2_M + \norm{Fx - y}_{Q^{-1}}^2 + \norm{Hy - z}_R^2$$

with limit case where prediction is certain (compliance $$Q=0$$).

# Extended Kalman Filter

Consider the following incremental *non-linear* least-squares problem:

$$x_{k+1} = \argmin{x} \ \sum_k \norm{f_k(x) - y_k}^2_{R_k}$$

Starting from an initial estimate $$x_0$$, we linearize each new measurement
$$f_{k+1}$$ at current estimate $$x_k$$ to obtain:

$$x_{k+1} = \argmin{x} \ \sum_k \norm{f_{k+1}\block{x_k} + \dd
f_{k+1}\block{x_k}.\block{x - x_k} - y_{k+1}}^2_{R_{k+1}}$$

A straightforward adaptation of the linear Kalman filter to the linearized
problem gives:

$$
\begin{align}
H_{k+1} &= \dd f_{k+1} \block{x_k} \\
z_{k+1} &= y_{k+1} + \dd f_{k+1}\block{x_k}.x_k - f_{k+1}\block{x_k} \\
\end{align}
$$

In particular, we obtain the following update for $$w$$:

$$w_{k+1} = y_{k+1} - f_{k+1}\block{x_k}$$

and the rest is the same as before. A similar linearization for non-stationary
processes can be obtained, again by linearizing the non-linear transition
function at the most up-to-date estimate for the state. See
[wikipedia](https://en.wikipedia.org/wiki/Extended_Kalman_filter#Discrete-time_predict_and_update_equations)
for details.

### Prediction

$$
\begin{align} 
\tilde{x}_k &= F_k x_k + u_k\\
\tilde{C}_k &= F_k C_k F_k^T
\end{align}
$$

### Update

$$
\begin{align}
H_{k+1} &= \dd f_{k+1} \block{x_k} \\
z_{k+1} &= y_{k+1} + H_{k+1}x_k - f_{k+1}\block{x_k} \\
w_{k+1} &= z_{k+1} - H_{k+1} \tilde{x}_k \\
S_{k+1} &= R_{k+1}^{-1} + H_{k+1} \tilde{C}_k H_{k+1}^T \\
C_{k+1} &= \tilde{C}_k - \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} H_{k+1} \tilde{C}_k \\
        &= \block{I - \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} H_{k+1}} \tilde{C}_k \\
x_{k+1} &= \tilde{x}_k + C_{k+1} H_{k+1}^T R_{k+1} w_{k+1} \\
 &= \tilde{x}_k + \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} w_{k+1} \\
\end{align}
$$


## Variant

Alternatively, one can consider the linearized least-squares problem expressed
$$purely$$ in terms of the solution update $$\delta x = x - x_k$$, which will
help us derive the equations for the Lie group case. Assuming $$x_k$$ is our
lastest estimate, we get $$x - x_i = \delta _x + x_k - x_i$$, which we plug into
the Extended Kalman Filter equation above to obtain:

$$\delta x_{k+1} = \argmin{\delta} \ \sum_{i=0}^k \norm{f_{i+1}\block{x_i} + \dd
f_{i+1}\block{x_i}.\block{\delta x + x_k - x_i} - y_{i+1}}^2_{R_{i+1}}$$

In this version, the state at each iteration is the *displacement* $$\delta x$$,
which will be predicted/corrected. The underlying position $$x_k$$ is maintained
separately only to update matrices/vectors accordingly. Each change of
linearization point can be seen as a coordinate change on the solution update
$$\delta x$$:

$$x = \delta x^{(k)} + x_k = \delta x^{(k-1)} + x_{k-1}$$

where the same $$x$$ is expressed in two coordinate systems $$\delta x^{(k)}$$
and $$\delta x^{(k-1)}$$, related by:

$$\delta x^{(k)} = \delta x^{(k-1)} - \block{x_k - x_{k-1}}$$

and now we can exploit the affine prediction update as seen before.

### Prediction

$$
\begin{align} 
\tilde{\dd x}_k &= \dd x_k - \block{x_k - x_{k-1}} \\
\tilde{C}_k &= F_k C_k F_k^T
\end{align}
$$

### Update

$$
\begin{align}
H_{k+1} &= \dd f_{k+1} \block{x_k} \\
z_{k+1} &= y_{k+1} - f_{k+1}\block{x_k} \\
w_{k+1} &= z_{k+1} - H_{k+1} \tilde{\dd x}_k \\

S_{k+1} &= R_{k+1}^{-1} + H_{k+1} \tilde{C}_k H_{k+1}^T \\
C_{k+1} &= \tilde{C}_k - \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} H_{k+1} \tilde{C}_k \\
        &= \block{I - \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} H_{k+1}} \tilde{C}_k \\
\dd x_{k+1} &= \tilde{\dd x}_k + \tilde{C}_{k+1} H_{k+1}^T R_{k+1} w_{k+1} \\
 &= \tilde{\dd x}_k + \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} w_{k+1} \\
x_{k+1} &= x_k + \dd x_k
\end{align}
$$

The first prediction line seems a bit silly as it appears to always
evaluate to zero, but it comes useful when incorporating state
constraints into the filter. In this case, $$x_{k+1} = x_k + \dd x_k +
\ldots$$ therefore the predicted $$\dd x$$ is not always zero.


# Lie Groups

We now consider an incremental non-linear least-squares problem on a Lie
group $$G$$:

$$\argmin{g \in G} \ \sum_k \norm{f_k(g) - y_k}^2_{R_k}$$

where $$f: G \to E$$ maps to an Euclidean space $$E$$. Instead of solving for
$$g$$ directly (which may involve non-linear constraints), we express it as a
body-fixed update from current estimate $$g_k$$ using the group exponential:

$$g = g_k.\exp\block{\delta g^{(k)}}$$

and reformulate our problem in terms of the solution update:

$$\delta g_{k+1} = \argmin{\delta g^{(k)} \in \alg{g}} \ \sum_k \norm{f_{k+1}\block{g_k.\exp\block{\delta g^{(k)}}} - y_{k+1}}_{R_{k+1}}^2$$

As before, we linearize the above at current estimate $$g_k$$ to obtain:

$$
\delta g_{k+1} = \argmin{\delta g^{(k)} \in \alg{g}} \ \sum_k \norm{f_{k+1}\block{g_k} +
\db f_{k+1} \block{g_k}.\delta g^{(k)} - y_{k+1}}_{R_{k+1}}^2$$

Coordinates at steps $$k-1$$ and $$k$$ are related by:

$$g = g_k.\exp\block{\delta g^{(k)}} = g_{k-1}.\exp\block{\delta g^{(k-1)}}$$

Therefore, the coordinate change is:

$$\delta g^{(k)} = \log\block{g_k^{-1} g_{k-1}.\exp\block{\delta g^{(k-1)}}}$$

which we linearize at $$\delta g^{(k-1)} = 0$$ to obtain:
 
$$
\begin{align}
\delta g^{(k)} &\approx \underbrace{-\delta g_{k-1}}_{u_k} + \underbrace{\db \log\block{g_k^{-1} g_{k-1}}}_{F_k}.\delta g^{(k-1)}\\
\end{align}
$$

Another convenient alternative is to linearize the coordinate change at $$\delta
g_{k-1}$$, *i.e.* the previous computed solution update:
 
$$
\begin{align}
\delta g^{(k)} &\approx \log\block{I} + \db \log\block{I}.\db \exp\block{\delta g_{k-1}}.\block{\delta g^{(k-1)} - \delta g_{k-1}}\\
&= \underbrace{\db \exp\block{\delta g_{k-1}}}_{F_k}.\delta g^{(k-1)} \ \underbrace{\ -\delta g_{k-1}}_{u_k}\\
\end{align}
$$
 



# Appendix

Consider the following linear system:

$$ \block{\inv{C} + H^T R H} x = H^T R z $$

One can verify that the solution $$x$$ for the above system is also
that of the following two augmented KKT systems:

1. $$ \mat{ \inv{C} & H^T \\ H & -R^{-1} } \mat{x \\ \lambda} = \mat{H^T R z \\ 0 }$$
2. $$ \mat{ \inv{C} & H^T \\ H & -R^{-1} } \mat{x \\ \lambda} = \mat{0 \\ z}$$

Now the solution for the first one is $$x = \block{C -
CH^T\inv{S}HC}H^TRz$$, where $$S = HCH^T + \inv{R}$$ is the Schur
complement, and the solution for the second is $$x = CH^T\inv{S}z$$.





