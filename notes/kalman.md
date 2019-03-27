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

and the solution is $$x = \block{A^T M A}^{-1} A^T M b$$. Now, let us
add extra rows to our (overconstrained) system:

$$A_{k+1} x_{k+1} = \mat{A_k \\ H_{k+1}} x_{k+1} = \mat{b_k \\ z_{k+1}} = b_{k+1}$$

At this point, it is convenient to rewrite the incremental system in terms of
the solution update $$\delta_{k+1}$$ defined such that $$x_{k+1} = x_k +
\delta_{k+1}$$, in order to express the current normal equations in terms of the
previous ones:

$$\delta_{k+1} = \argmin{\delta} \quad \norm{ \mat{A_k \\ H_{k+1}} \block{x_k + \delta} - \mat{b_k \\ z_{k+1}} }_{M_{k+1}}^2$$

where we assumed $$M_{k+1} = \mat{M_k & \\ & R_{k+1}}$$. Equivalently:

$$\delta_{k+1} = \argmin{\delta} \quad \norm{ \mat{A_k \\ H_{k+1}} \delta - \mat{ b_k - A_k x_k \\ z_{k+1} - H_{k+1} x_k} }_{M_{k+1}}^2$$

The normal equations become:

$$ \underbrace{\block{A_k^T M_k A_k + H_{k+1}^T R_{k+1} H_{k+1}}}_{K_{k+1}} \delta = \underbrace{A_k^T M_k\block{b_k - A_k x_k}}_0 + H_{k+1}^T R_{k+1} \underbrace{\block{z_{k+1} - H_{k+1}x_k}}_{w_{k+1}}$$

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

We now suppose that the state $$x$$ changes linearly as follows:

$$ x_k \overset{F_k}{\longmapsto} x_{k+1} $$

for some invertible linear mapping $$F_k$$. Think of it as a change of
coordinates at each step. The incremental problem becomes:

$$ \argmin{x} \quad \norm{ \mat{A_k \inv{F}_k \\ H_{k+1}} x - \mat{b_k \\ z_{k+1}} }^2_{M_{k+1}} $$

*i.e.* we fit previous observations by reverting to the previous
coordinate system. The normal equations become:

$$ \underbrace{\block{F_k^{-T} A_k^T M A_k \inv{F}_k + H_{k+1}^T R_{k+1} H_{k+1}}}_{K_{k+1}} x = F_k^{-T} A_k^T M_k b_k + H_{k+1}^T R_{k+1} z_{k+1} $$

which in terms of the solution update $$\delta = x - F_k x_k$$ gives:

$$ K_{k+1} \delta = F_k^{-T} \underbrace{A_k^T M_k \block{b_k - A_k x_k}}_0 + H_{k+1}^T R_{k+1} \underbrace{\block{z_{k+1} - H_{k+1}F_kx_k}}_{w_{k+1}} $$

since once again, $$x_k$$ solves the problem at step $$k$$. Using the
Woodbury formula as before, we obtain $$C_{k+1} = K_{k+1}^{-1} = D_k -
D_k H_{k+1}^T S_{k+1}^{-1} H_{k+1} D_k$$, where $$D_k = F_k C_k
F_k^T$$ and $$S_{k+1} = R_{k+1}^{-1} + H_{k+1} D_k H_{k+1}^T$$. The
solution update is traditionally decomposed into two
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
x_{k+1} &= \tilde{x}_k + \tilde{C}_{k+1} H_{k+1}^T R_{k+1} w_{k+1} \\
 &= \tilde{x}_k + \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} w_{k+1} \\
\end{align}
$$

## Affine update

We now suppose that the state $$x$$ changes *affinely* as follows:

$$ x_k \longmapsto F_k x_{k+1} + u_k $$

The incremental problem becomes:

$$ \argmin{x} \quad \norm{ \mat{A_k \inv{F}_k \\ H_{k+1}} x - \mat{b_k + A_k \inv{F}_k u_k \\ z_{k+1}} }^2_{M_{k+1}} $$

Terms once again cancel each other in the normal equations, for the
solution update $$\delta = x - \block{F_k x_k + u_k}$$:

$$K_{k+1} \delta = H_{k+1}^T R_{k+1} \underbrace{\block{z_{k+1} - H_{k+1}\block{F_k x_k + u_k}}}_{w_{k+1}}$$

So the prediction phase simply changes to:

$$
\tilde{x}_k = F_k x_k + u_k
$$

and all the rest remains the same.

# TODO Process Noise

Not quite sure how to obtain this one, looks like some kind of dual
regularization.

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
w_{k+1} &= y_{k+1} - f_{k+1}\block{x_k} \\
\end{align}
$$

and the rest is the same as before. A similar linearization for
non-stationary processes can be obtained, again by linearizing the
non-linear transition function at the most up-to-date estimate for the
state. See
[wikipedia](https://en.wikipedia.org/wiki/Extended_Kalman_filter#Discrete-time_predict_and_update_equations)
for details.

## Variant

Alternatively, one can consider the linearized least-squares problem expressed
$$purely$$ in terms of the solution update $$\dd x = x - x_k$$, which will help
us derive the equations for the Lie group case:

$$\dd x_{k+1} = \argmin{\dd x} \ \sum_{i=0}^k \norm{f_{i+1}\block{x_i} + \dd f_{i+1}\block{x_i}.\block{\dd x + x_k - x_i} - y_{i+1}}^2_{R_{i+1}}$$

In this version, the state at each iteration is the *displacement* $$\dd x$$,
which will be predicted/corrected. The underlying position $$x_k$$ is maintained
separately only to update matrices/vectors accordingly. Each change of
linearization from point $$k-1$$ to $$k$$ is affine:

$$\dd x \mapsto \dd x - (x_k - x_{k-1})$$

so we can exploit the affine prediction update as seen before:

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

Now consider an incremental non-linear least-squares problem on a Lie
group $$G$$:

$$\argmin{g \in G} \ \sum_k \norm{f_k(g) - y_k}^2_{R_k}$$

where $$f: G \to E$$ maps to an Euclidean space $$E$$. Again, each
term is linearized using the most up-to-date estimate for the state,
and the group exponential map:

$$g_{k+1} = \argmin{g} \ \sum_k \norm{f_{k+1}\block{g_k} + \db f_{k+1}\block{g_k}.\log\block{g_k^{-1}g} - y_{k+1}}^2_{R_{k+1}}$$

where $$\db f$$ is the body-fixed differential of $$f$$. Let $$v
= \log\block{g_k^{-1} g}$$ be the state update viewed from state
$$k$$, then the state update viewed from state $$k-1$$ is:

$$
\begin{align}
\log\block{g_{k-1}^{-1} g} &= \log\block{g_{k-1}^{-1}g_k g_k^{-1} g} \\
&= \log\block{ \exp\block{v_k} \exp\block{v} } \\
&\approx v_k + \db \log{\block{\exp\block{v_k}}}.v \\
&= v_k + \block{\db \exp\block{v_k}}^{-1}.v \\

\end{align}
$$

where we linearized at $$v = 0$$. The above is an affine
coordinate change whose inverse is the following:

$$ v \mapsto \db \exp\block{v_k}.v - \underbrace{\db
\exp\block{v_k}.v_k}_{=v_k} $$

The linearized problem can be rewritten as a linear Kalman filter in
$$v$$ using the following update rules:

$$
\begin{align}
F_k &= \db \exp\block{v_k} \\
\tilde{v}_k &= 0 \\
\tilde{C}_k &= F_k C_k F_k^T \\
\\
w_{k+1} &= y_{k+1} - f_{k+1}\block{g_k} \\ 
H_{k+1} &= \db f_{k+1}\block{g_k} \\
S_{k+1} &= R_{k+1}^{-1} + H_{k+1} \tilde{C}_k H_{k+1}^T \\
C_{k+1} &= \tilde{C}_k - \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} H_{k+1} \tilde{C}_k \\
        &= \block{I - \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} H_{k+1}} \tilde{C}_k \\
v_{k+1} &=  \tilde{C}_{k+1} H_{k+1}^T R_{k+1} w_{k+1} \\
 &=  \tilde{C}_k H_{k+1}^T S_{k+1}^{-1} w_{k+1} \\
 g_{k+1} &= g_k \exp\block{v_{k+1} } \\
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





