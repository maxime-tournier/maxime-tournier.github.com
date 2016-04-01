---
title: Kalman Filter
categories: [math]
---

A purely geometric take on [Kalman filtering](https://en.wikipedia.org/wiki/Kalman_filter).

# Incremental Least Squares

Our problem is to solve the following least-squares problem: 

$$\argmin{x} \quad \norm{Ax - b}_M^2$$

where $$M$$ is positive definite. Assuming that $$A$$ has full row
rank, the normal equations for the above are:

$$ \underbrace{A^T M A}_K \ x = A^T M b $$

and the solution is $$x = \block{A^T M A}^{-1} A^T M b$$. Now, let us
add extra equations to our (overconstrained) system:

$$A_{k+1} x_{k+1} = \mat{A_k \\ H_{k+1}} x_{k+1} = \mat{b_k \\ z_{k+1}} = b_{k+1}$$

At this point, it is convenient to rewrite the incremental system in
terms of the solution update $$\delta_{k+1}$$ defined such that
$$x_{k+1} = x_k + \delta_{k+1}$$:

$$\delta_{k+1} = \argmin{\delta} \quad \norm{ \mat{A_k \\ H_{k+1}} \block{x_k + \delta} - \mat{b_k \\ z_{k+1}} }_{M^2_{k+1}}$$

where we assumed $$M_{k+1} = \mat{M_k & \\ & R_{k+1}}$$. Equivalently:

$$\delta_{k+1} = \argmin{\delta} \quad \norm{ \mat{A_k \\ H_{k+1}} \delta - \mat{ b_k - A_k x_k \\ z_{k+1} - H_{k+1} x_k} }_{M^2_{k+1}}$$

The normal equations become:

$$ \underbrace{\block{A_k^T M A_k + H_{k+1}^T R_{k+1} H_{k+1}}}_{K_{k+1}} \delta = \underbrace{A_k^T M_k\block{b_k - A_k x_k}}_0 + H_{k+1}^T R_{k+1} \underbrace{\block{z_{k+1} - H_{k+1}x_k}}_{w_{k+1}}$$

since $$x_k$$ solves the problem at step $$k$$. Luckily, the
[Woodbury formula](https://en.wikipedia.org/wiki/Woodbury_matrix_identity)
provides a simple way to update the inverse $$C_{k+1}$$ of $$K_{k+1}$$ as follows:

$$
\begin{align} 
C_{k+1} &= K_{k+1}^{-1} \\
	&= \inv{\block{K_k + H_{k+1}^T R_{k+1} H_{k+1}}} \\
	&= C_k - C_k H_{k+1}^T \block{ R_{k+1}^{-1} + H_{k+1} C_k H_{k+1}^T }^{-1} H_{k+1} C_k \\
\end{align}
$$

Let us denote $$S_{k+1} = R_{k+1}^{-1} + H_{k+1} C_k H_{k+1}^T$$, the
whole update process becomes:

$$
\begin{align} 
w_{k+1} &= z_{k+1} - H_{k+1} x_k \\ 
S_{k+1} &= R_{k+1}^{-1} + H_{k+1} C_k H_{k+1}^T \\
C_{k+1} &= C_k - C_k H_{k+1}^T S_{k+1}^{-1} H_{k+1} C_k \\
        &= \block{I - C_k H_{k+1}^T S_{k+1}^{-1} H_{k+1}} C_k \\
x_{k+1} &= x_k + C_{k+1} H_{k+1}^T R_{k+1} w_{k+1} \\		
\end{align}
$$

Alternatively, the appendix shows that $$C_{k+1} H_{k+1}^T R_{k+1} =
C_k H_{k+1} S_{k+1}^{-1}$$, so that the solution update can also be
obtained as:

$$x_{k+1} = x_k + C_k H_{k+1}^T S_{k+1}^{-1} w_{k+1}$$

which might be more convenient to use in practice.

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
prediction/correction phases:

### Prediction

$$
\begin{align} 
\tilde{x}_k &= F_k x_k \\
\tilde{C}_k &= F_k C_k F_k^T
\end{align}
$$

### Correction

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

# TODO Process Noise

Not quite sure how to obtain this one, looks like some kind of dual
regularization.

# TODO Non-linear Kalman Filter

A linearization of the problem is is solved at each step on tangent
vectors.

# TODO Lie Groups

Start from the non-linear Kalman filter, trivialize tangent vectors +
express solution update using group exponential. The group adjoint
must account for change of (body/spatial) coordinates between steps
according to trivialization.

# Appendix

Let us consider the following linear system:

$$ \block{\inv{C} + H^T R H} x = H^T R z $$

It is easy to see that the solution $$x$$ for the above system is also
that of the following two augmented KKT systems:

1. $$ \mat{ \inv{C} & H^T \\ H & -R } \mat{x \\ \lambda} = \mat{H^T R z \\ 0 }$$
2. $$ \mat{ \inv{C} & H^T \\ H & -R } \mat{x \\ \lambda} = \mat{0 \\ z}$$

Now the solution for the first one is $$x = \block{C -
CH^T\inv{S}HC}H^TRz$$, where $$S = HCH^T + \inv{R}$$ is the Schur
complement, and the solution for the second is $$x = CH^T\inv{S}z$$.
Both solutions are equal since both KKT systems are equivalent to the
initial system and since $$z$$ is arbitrary, the proof is complete.
