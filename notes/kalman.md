---
title: Kalman Filter
categories: [math]
---

A purely geometric take on Kalman filtering.

# Incremental Least Squares

Our problem is to solve the following least-squares problem: 

$$\argmin{x} \quad \norm{Ax - b}_M^2$$

where $$M$$ is positive definite. Assuming that $$A$$ has full row
rank, the normal equations for the above are:

$$ A^T M A x = A^T M b $$

and the solution is $$x = \inv{\block{A^T M A}} A^T M b$$. Now, let us
add extra equations to our (overconstrained) system:

$$A_{k+1} x = \mat{A_k \\ H_{k+1}} x = \mat{b_k \\ z_{k+1}} = b_{k+1}$$

The normal equations become:

$$ \block{A_k^T M A_k + H_{k+1}^T R_{k+1} H_{k+1}} x = A_k^T M_k b_k + H_{k+1}^T R_{k+1} z_{k+1} $$

where we assumed $$M_{k+1} = \mat{M_k & \\ & R_{k+1}}$$. Luckily, the
[Woodbury formula](https://en.wikipedia.org/wiki/Woodbury_matrix_identity)
gives us an easy way to update the inverse of $$K = A^T M A$$:

$$
\begin{align} 
C_{k+1} &= \inv{K_{k+1}} \\
	&= \inv{\block{K_k + H_{k+1}^T R_{k+1} H_{k+1}}} \\
	&= C_k - C_k H_{k+1}^T \inv{\block{ \inv{R_{k+1}} + H_{k+1} C_k H_{k+1}^T }} H_{k+1} C_k \\
\end{align}
$$

Let us denote $$S_{k+1} = \inv{R_{k+1}} + H_{k+1} C_k H_{k+1}^T$$ and $$y_k =
A_k^T M_k b_k$$, the whole update process becomes:

$$
\begin{align} 
y_{k+1} &= y_k + H_{k+1}^T R_{k+1} z_{k+1} \\
S_{k+1} &= \inv{R_{k+1}} + H_{k+1} C_k H_{k+1}^T \\
C_{k+1} &= C_k - C_k H_{k+1}^T \inv{S_{k+1}} H_{k+1} C_k \\
        &= \block{I - C_k H_{k+1}^T \inv{S_{k+1}} H_{k+1}} C_k \\
x_{k+1} &= C_{k+1} y_{k+1} \\		
\end{align}
$$		

The solution update can be rewritten as follows:

$$
\begin{align}
x_{k+1} &= C_{k+1} y_{k+1} \\
&= \block{ C_k - C_k H_{k+1}^T \inv{S_{k+1}} H_{k+1} C_k } \block{y_k + H_{k+1}^T R_{k+1} z_{k+1}} \\
&= C_k y_k - C_k H_{k+1}^T \inv{S_{k+1}} H_{k+1} C_k y_k + C_{k+1} H_{k+1}^T R_{k+1} z_{k+1} \\
&= x_k - C_k H_{k+1}^T \inv{S_{k+1}} H_{k+1} x_k + C_{k+1} H_{k+1}^T R_{k+1} z_{k+1} \\
&= x_k + C_k H_{k+1}^T \inv{S_{k+1}} \block{z_{k+1} -  H_{k+1} x_k}
\end{align}
$$

where the last line uses the fact that $$C_{k+1} H_{k+1}^T R_{k+1} = C_k
H_{k+1}^T \inv{S}_{k+1}$$ (see the appendix for a
derivation). Introducing the observation error $$w_{k+1} = z_{k+1} -
H_{k+1} x_k$$, we obtain the equations for a Kalman filter with
identity transition matrix and zero process noise (we are certain the
process is stationary), and observation noise covariance equal to
$$\inv{R}$$:

$$
\begin{align} 
w_{k+1} &= z_{k+1} - H_{k+1} x_k \\ 
S_{k+1} &= \inv{R_{k+1}} + H_{k+1} C_k H_{k+1}^T \\
C_{k+1} &= C_k - C_k H_{k+1}^T \inv{S_{k+1}} H_{k+1} C_k \\
        &= \block{I - C_k H_{k+1}^T \inv{S_{k+1}} H_{k+1}} C_k \\
x_{k+1} &= x_k + C_k H_{k+1}^T \inv{S_{k+1}} w_{k+1} \\		
\end{align}
$$

# Non-stationary Process

We now suppose that the state $$x$$ changes as follows:

$$ x_{k+1} = F_k x_k $$

for some invertible linear mapping $$F_k$$. The incremental problem
becomes:

$$ \argmin{x} \quad \norm{ \mat{A_k \inv{F}_k \\ H_{k+1}} x - \mat{b_k \\ z_{k+1}} }^2_{M_{k+1}} $$

*i.e.* we fit previous observations by taking one step backwards. The
normal equations become:

$$ \underbrace{\block{F_k^{-T} A_k^T M A_k \inv{F}_k + H_{k+1}^T R_{k+1} H_{k+1}}}_{K_{k+1}} x = F_k^{-T} A_k^T M_k b_k + H_{k+1}^T R_{k+1} z_{k+1} $$

We obtain easily that $$C_{k+1} = \inv{K}_{k+1} = D_k - D_k H_{k+1}^T \inv{S_{k+1}}
H_{k+1} D_k$$, where $$D_k = F_k C_k F_k^T$$ and $$S_{k+1} =
\inv{R_{k+1}} + H_{k+1} D_k H_{k+1}^T$$. The right-hand side is:

$$ y_{k+1} = F^{-T} y_k + H_{k+1}^TR_{k+1} z_{k+1} $$

The solution update is:

$$
\begin{align}
x_{k+1} &= D_k F^{-T} y_k - D_k H_{k+1}^T \inv{S_{k+1}} H_{k+1} D_k F^{-T} y_k + D_{k+1} H_{k+1}^T R_{k+1} z_{k+1} \\
&= F_k x_k - D_k H_{k+1}^T \inv{S_{k+1}} H_{k+1} F_k x_k + D_{k+1} H_{k+1}^T R_{k+1} z_{k+1} \\
&= F_k x_k + D_k H_{k+1}^T \inv{S_{k+1}} \block{ z_{k+1} - H_{k+1} F_k x_k } \\
\end{align}
$$

where we used the appendix for the last line. The final update rules
can be decomposed into two predition/correction phases:

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

S_{k+1} &= \inv{R_{k+1}} + H_{k+1} \tilde{C}_k H_{k+1}^T \\
C_{k+1} &= \tilde{C}_k - \tilde{C}_k H_{k+1}^T \inv{S_{k+1}} H_{k+1} \tilde{C}_k \\
        &= \block{I - \tilde{C}_k H_{k+1}^T \inv{S_{k+1}} H_{k+1}} \tilde{C}_k \\
x_{k+1} &= \tilde{x}_k + \tilde{C}_k H_{k+1}^T \inv{S_{k+1}} w_{k+1} \\		
\end{align}
$$

# TODO Process Noise


# Appendix: $$C_{k+1} H_{k+1}^T R_{k+1} = C_k H_{k+1}^T \inv{S}_{k+1}$$

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
