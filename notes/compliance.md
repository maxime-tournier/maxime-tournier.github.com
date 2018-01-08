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

Let us introduce $$G_k = \ddd{}{q_k}\dd J\block{q_k}^T \lambda_k$$ the
*Geometric Stiffness* of mapping $$f$$ at $$\block{q_k,
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

$$J_k = J_{f \circ g}\block{q_k} = \dd f\block{g\block{q_k}}.\dd g\block{q_k}$$

The Geometric Stiffness is a bit more involved:

$$\begin{align}
G_k &= \ddd{}{q_k} J_k^T\lambda_k\\
    &= \ddd{}{q_k}\dd g\block{q_k}^T.\dd f\block{g\block{q_k}}^T\lambda_k \\
    &= \block{\ddd{}{q_k} \dd g\block{q_k}^T }\dd f\block{g\block{q_k}}^T\lambda_k + \dd g\block{q_k}^T.\ddd{}{q_k}\dd f\block{g\block{q_k}}^T\lambda_k\\
\end{align}
$$

Now, if we note $$\gamma = J_f^T \lambda$$ the pullback of $$\lambda$$
by $$f$$, the left part is simply the geometric stiffness of $$g$$ at
$$\block{q_k, \gamma_k}$$:

$$\block{\ddd{}{q_k} \dd g\block{q_k}^T }\dd f\block{g\block{q_k}}^T\lambda_k = G_g\block{q_k, \gamma_k}$$

The right part requires more care:

$$\begin{align}
\dd g\block{q_k}^T.\ddd{}{q_k}\dd f\block{g\block{q_k}}^T\lambda_k 
&= \dd g\block{q_k}^T.\block{\ddd{}{q_k}\dd f\block{g\block{q_k}}^T\lambda_k}.\dd g\block{q_k} \\
&= J_g\block{q_k}^T G_f\block{g\block{q_k}, \lambda_k} J_g\block{q_k} \\
\end{align}$$

Finally, we obtain:

$$G_{f \circ g}\block{q_k, \lambda_k} = G_g\block{q_k, \gamma_k} + J_g\block{q_k}^T G_f\block{g\block{q_k}, \lambda_k} J_g\block{q_k}$$

where $$\gamma_k = J_f\block{g\block{q_k}}^T\lambda_k$$. This gives a
general algorithm for computing geometric stiffnesses (or their
product with a vector):

1. push 
   - compute $$g\block{q_k}, J_g\block{q_k}$$
   - compute $$f\block{g\block{q_k}}, J_f\block{g\block{q_k}}$$, ...
2. pull
   - compute $$\gamma_k = J_f\block{g\block{q_k}}^T\lambda_k$$
   - compute $$G_f\block{g\block{q_k}, \lambda_k}$$
   - compute $$J_g\block{q_k}^T G_f\block{g\block{q_k}, \lambda_k} J_g\block{q_k}$$, then add $$G_g\block{q_k, \gamma_k}$$, ...


# Lie Groups

