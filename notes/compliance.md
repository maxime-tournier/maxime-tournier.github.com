---
title: Compliance
categories: [phys]
---

{% include toc.md %}

# Equations of Motion

Consider a conservative mechanical system described by the following
Lagrangian:

$$\LL\block{q, \dot{q}} = \half \dot{q}^T M \dot{q}$$

Introduce $$\lambda$$ variables, a positive semidefinite compliance
matrix $$C \geq 0$$ and a kinematic mapping $$f(q)$$:

$$\LL\block{q, \lambda, \dot{q}} = \half \dot{q}^T M \dot{q} + f(q)^T\lambda + \half \lambda^T C \lambda$$

Partial derivatives:

$$\begin{align}
\ddd{\LL}{\dot{q}} &= \mat{M\dot{q} \\ 0} \\
\ddd{\LL}{q, \lambda} &= \mat{J(q)^T \lambda \\ f(q) + C \lambda} \\
\end{align}$$

The Euler-Lagrange equations are:

$$\frac\dd{\dd t}\ddd{\LL}{\dot{q}} = \mat{M \ddot{q} \\ 0} = \mat{J(q)^T \lambda \\ f(q) + C \lambda}$$

which are the exact same equations of motions as the following
mechanical system:

$$\LL\block{q, \dot{q}} = \half \dot{q}^T M \dot{q} - \half\norm{f(q)}^2_K$$

with stiffness matrix $$K = \inv{C}$$ when it makes sense. Notice that
the limit case $$C = 0$$ gives the (holonomic) constrained system
equations.

# Time Discretization

Let us fix a time step $$h > 0$$ and discretize the equations of
motion at time $$k$$:

$$\mat{M \ddot{q}_k \\ 0} = \mat{J\block{q_k}^T \lambda_k \\ f\block{q_k} + C \lambda_k}$$

Backward differences for accelerations: 

$$\ddot{q}_{k+1} \approx \frac{\dot{q}_{k+1} - \dot{q}_k}{h}$$

which yields:

$$\mat{M \dot{q}_{k+1} \\ 0} = \mat{M\dot{q}_k + hJ\block{q_{k+1}}^T \lambda_{k+1} \\ f\block{q_{k+1}} + C \lambda_{k+1}}$$

First-order approximations:

$$
\begin{align}
J\block{q_{k+1}}^T \lambda_{k+1} &\approx J\block{q_k}^T \lambda_k + \block{ \block{\dd J\block{q_k}.\dd q_{k+1}}^T \lambda_k + J^T\block{q_k}.\dd \lambda_{k+1} } \\
f\block{q_{k+1}} &\approx f\block{q_k} + J\block{q_k}.\dd q_{k+1} \\
\end{align}
$$

In our case: 

$$\dd q_{k+1} = h.\dot{q}_{k+1}$$

Letting $$v_{k+1} = \dot q_{k+1}$$, we end up with:

$$
\mat{M v_{k+1} \\ 0} = \mat{M v_k + h \block{ \block{h.\dd J\block{q_k}.v_{k+1}}^T \lambda_k + J^T\block{q_k}.\lambda_{k+1}} \\ f\block{q_k} + h.J\block{q_k}.v_{k+1} + C \lambda_{k+1}}
$$

Let us introduce the *Geometric Stiffness* $$G_k = \ddd{}{q_k}\dd
J\block{q_k}^T \lambda_k$$ of mapping $$f$$ at $$\block{q_k,
\lambda_k}$$. We may rewrite the above system as:

$$
\begin{align}
\block{M - h^2 G_k} v_{k+1} - h J^T\block{q_k} \lambda_{k+1} &= M v_k  \\
-h J\block{q_k} v_{k+1} - C \lambda_{k+1} &= f\block{q_k}\\
\end{align}
$$

Alternatively, in matrix form:

$$\mat{M - h^2 G_k & -hJ_k^T \\ -hJ_k & -C} \mat{v_{k+1} \\ \lambda_{k+1}} = \mat{M v_k \\ f_k}$$

In other words: a *bona fide* saddle point system. Notice how the case $$K
\to +\infty$$ naturally degrades to the holonomic constraint case $$C
\to 0$$. It is possible to rewrite this system using only
*first-order* quantities by introducing the momentum $$\mu =
h.\lambda$$ to obtain:

$$\mat{M - h^2 G_k & -J_k^T \\ -J_k & -\frac{C}{h^2}} \mat{v_{k+1} \\ \mu_{k+1}} = \mat{M v_k \\ \frac{f_k}{h}}$$

# Composing Geometric Stiffnesses

The Geometric Stiffness being a second derivative, it follows the
associated composition rules. More precisely, let us now consider a
composed kinematic mapping $$f \circ g$$. The Jacobian matrix is given
by:

$$J_{f \circ g}\block{q} = \dd f\block{g\block{q}}.\dd g\block{q}$$

The Geometric Stiffness is a bit more involved:

$$\begin{align}
G_{f \circ g} &= \ddd{}{q} J^T\lambda\\
    &= \ddd{}{q}\dd g\block{q}^T.\dd f\block{g\block{q}}^T\lambda \\
    &= \block{\ddd{}{q} \dd g\block{q}^T }\dd f\block{g\block{q}}^T\lambda\  + \ \dd g\block{q}^T.\ddd{}{q}\block{\dd f\block{g\block{q}}^T\lambda}\\
\end{align}
$$

Now, if we denote by $$\gamma = J_f^T \lambda$$ the pullback of
$$\lambda$$ by $$f$$, we recognize the left-hand side as the geometric
stiffness of $$g$$ at $$\block{q, \gamma}$$:

$$\block{\ddd{}{q} \dd g\block{q}^T }\dd f\block{g\block{q}}^T\lambda = G_g\block{q, \gamma}$$

The right part requires more care:

$$\begin{align}
\dd g\block{q}^T.\ddd{}{q}\block{\dd f\block{g\block{q}}^T\lambda}
&= \dd g\block{q}^T.\block{\ddd{}{q}\dd f\block{g\block{q}}^T\lambda}.\dd g\block{q} \\
&= J_g\block{q}^T G_f\block{g\block{q}, \lambda} J_g\block{q} \\
\end{align}$$

Finally, we obtain:

$$G_{f \circ g}\block{q, \lambda} = G_g\block{q, \gamma} + J_g\block{q}^T G_f\block{g\block{q}, \lambda} J_g\block{q}$$

where $$\gamma = J_f\block{g\block{q}}^T\lambda$$. This gives a
general algorithm for computing geometric stiffnesses (or their
product with a vector):

1. push 
   - compute $$g\block{q}, J_g\block{q}$$
   - compute $$f\block{g\block{q}}, J_f\block{g\block{q}}$$, ...
2. pull
   - compute $$\gamma = J_f\block{g\block{q}}^T\lambda$$
   - compute $$G_f\block{g\block{q}, \lambda}$$
   - compute $$J_g\block{q}^T G_f\block{g\block{q}, \lambda} J_g\block{q}$$, then add $$G_g\block{q, \gamma}$$, ...


# Lie Groups

TODO

# Damping

Consider the following primal first-order time-discretization:

$$\alpha Mv + J^T\block{\beta D + \gamma K}J v = \delta p + \eta J^T K f$$

where $$\alpha, \beta, \gamma, \delta, \eta$$ are constants depending
on the integration method[^1]. Letting $$C = \inv{K}$$, the
compliance system for the above is obtained as follows:

$$\begin{align}
\alpha Mv - J^T \lambda &= \delta p \\
-\lambda &= \block{\beta D + \gamma K} J v - \eta K f \\
&= K\block{\beta CD + \gamma I} J v - \eta K f \\
-\block{\beta CD + \gamma I}^{-1}C\lambda &= Jv - \eta \block{\beta CD + \gamma I}^{-1} f \\
Jv + \underbrace{\block{\beta CD + \gamma I}^{-1}C}_W\lambda &= \eta \block{\beta CD + \gamma I}^{-1} f \\
\end{align}
$$

Note that matrix $$W$$ is always symmetric, positive semi-definite
and remains well-defined as $$K \to +\infty$$. This formulation,
combined with an LCP solver, provides unilateral constraints with
arbitrary stiffness/damping.

## composing 

$$L = \half v^T M v - \norm{f(q)}_K^2 + g\block{f(q)}^T \lambda + \half \lambda^T C \lambda$$


$$\ddd{L}{v} = Mv$$

$$\ddd{L}{q, \lambda} = \mat{-J_f^T(q)^TKf(q) + J_f^TJ_g^T\lambda\\ g(f(q)) + C\lambda}$$

$$\ddd{L}{q, \lambda} = \mat{J_f^T(q)^T\block{ -Kf(q) + J_g^T\lambda}\\ g(f(q)) + C\lambda}$$


# Notes

[^1]: For Implicit Euler: $$\alpha = 1, \beta = h, \gamma = h^2,
    \delta = 1, \eta = h$$ where $$h$$ is the time step.
