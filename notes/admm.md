---
title: Alternating Directions Method of Minimizers
categories: [math]
tags: [draft]
---

{% include toc.md %}

Some notes on ADMM based on Boyd's resources [^boyd].

# Dual Ascent 

Let us consider the following problem:

$$\min_x \quad f(x) \quad \st A x = b$$

Again, the Lagrangian is:

$$\LL(x, \lambda) = f(x) - \lambda^T\block{Ax - b}$$

with dual function:

$$d(\lambda) = \min_x \LL(x, \lambda) = f(x) - \lambda^T\block{Ax - b}$$

Assuming that $$f$$ is smooth and that we can compute the dual function its
gradient is trivial to compute:

$$\nabla d(\lambda) = Ax^\star(\lambda) - b$$

where $$x^\star(\lambda) = \argmin{x}\quad \LL(x, \lambda) = f(x) - \lambda^T\block{Ax -
b}$$. Indeed, by definition of the dual function, we have:

$$d(\lambda) = \LL\block{x^\star(\lambda), \lambda}$$

But since $$x^\star(\lambda)$$ minimizes $$\LL\block{\cdot, \lambda}$$, the
derivative along $$x$$ at the optimum is zero:

$$\ddd{\LL}{x}\block{x^\star(\lambda), \lambda} = 0$$

so that only $$\ddd{\LL}{\lambda}\block{x^\star(\lambda), \lambda} =
\block{Ax^\star - b}^T$$ appears in the differential of $$d(\lambda)$$. We now
have a smooth[^dual-ascent] function and its gradient, we can simply use gradient ascent with
step sizes $$\alpha_k$$ to solve the dual problem of maximizing $$d(\lambda)$$:

1. initialize $$\lambda_0 = 0$$
2. solve $$x_k = \argmin{x}\quad  f(x) - \lambda_k^T\block{Ax - b}$$
3. update $$\lambda_{k+1} = \lambda_k - \alpha_k \block{A x_k - b}$$
4. goto 2 until sufficient precision is achieved (more on this below)

Note that when $$f$$ is not smooth, the above procedure provides a subgradient
at each iteration and the method is known as *dual subgradient ascent*.

# Method of Multipliers

As usual, there are conditions to be met for the simple gradient ascent to
converge. Unfortunately, it is not trivial to get Lipschitz constants for the
dual function, therefore it might be difficult to converge robustly in practice.

In order to improve convergence, the *Method of Multipliers* replaces
the initial problem with the following equivalent one:

$$\min_x \quad f(x) + \rho \norm{Ax - b}^2 \quad \st A x = b$$

whose Lagrangian is called the *Augmented Lagrangian*. Clearly the
added penalty is zero on the feasible set, so the two problems are
equivalent. In a sense, doing so regularizes the function $$f$$
outside the feasible set by driving solutions towards the feasible
set, over which the regularization vanishes.

Applying dual ascent to the regularized problem yields the following
optimality conditions *(dual feasibility)* for the optimization
problem at each iteration:

$$\nabla f\block{x_k} + \underbrace{\rho A^T\block{Ax_k - b} - A^T\lambda_k}_{-A^T\block{\lambda_k - \rho\block{Ax_k - b}}} = 0$$

This suggests that picking step size $$\alpha_k = \rho$$ such that
$$\lambda_{k+1} = \lambda_k - \rho\block{Ax_k - b}$$ will have the following
benefits:

- dual feasibility of the regularized problem at $$\lambda_k$$ is
  equivalent to dual feasibility *of the original problem* at
  $$\lambda_{k+1}$$: $$\nabla f\block{x_k} -A^T\lambda_{k+1} = 0$$
- this corresponds to *implicit integration* of the gradient flow of
  the dual function for the original problem: the gradient found by
  solving the regularized problem is that of the original problem at
  $$\lambda_{k+1}$$ instead of $$\lambda_k$$

Therefore we might expect the nice properties of implicit integration
to somehow ensure convergence. More rigorously, convergence can be
shown by considering the dual error $$V_k = \norm{\lambda_{k+1} -
\lambda^\star}^2$$ where $$\lambda^\star$$ is the solution:

$$
\begin{aligned}
V_{k+1} - V_k &= \norm{\lambda_{k+1} - \lambda_k + \lambda_k - \lambda^\star}^2 - \norm{\lambda_k - \lambda^\star}^2\\
&= \norm{\lambda_{k+1} - \lambda_k}^2 + \norm{\lambda_k - \lambda^\star}^2 + 2\block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_k - \lambda^\star} - \norm{\lambda_k - \lambda^\star}^2 \\
&= \norm{\lambda_{k+1} - \lambda_k}^2 + 2\block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_k - \lambda^\star} \\
&= \norm{\lambda_{k+1} - \lambda_k}^2 + 2\block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_k - \lambda_{k+1} + \lambda_{k+1} - \lambda^\star} \\
&= - \norm{\lambda_{k+1} - \lambda_k}^2 + 2 \block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_{k+1} - \lambda^\star} \\
\end{aligned}
$$

The second term can be rewritten in terms of $$x$$:

$$
\begin{aligned}
\block{\lambda_{k+1} - \lambda_k}^T\block{\lambda_{k+1} - \lambda^\star} &=
    -\rho\block{A x_k - b}^T \block{\lambda_{k+1} - \lambda^\star} \\
    &= -\rho\block{x_k - x^\star}^TA^T\block{\lambda_{k+1} - \lambda^\star} \\
    &= -\rho\block{x_k - x^\star}^T\block{\nabla f\block{x_k} - \nabla f\block{x^\star}} \\ 
    &\leq 0
\end{aligned}
$$

due to $$\nabla f$$ being monotone since $$f$$ is convex. This means
the method strictly converges while $$x_k$$ is not primal feasible,
and terminates with a result that is both primal and dual
feasible. One can also show that the Method of Multipliers corresponds
to the proximal point algorithm applied to the dual function *(dual
proximal point method)*, which is more direct in the non-smooth case.

The Method of Multipliers is also known as the *Augmented Lagrangian
Method* (ALM).

## Scaled Form

At every iteration, $$x$$ is solved as:

$$x_k = \argmin{x}\ f(x) - \lambda_k^T\block{Ax - b} + \frac{\rho}{2}\norm{Ax - b}^2$$

The above can be simplified by completing the square:

$$\frac{\rho}2\norm{Ax - b - \frac{\lambda_k}{\rho}}^2 =  \frac{\rho}{2}\norm{Ax - b}^2 - \lambda_k^T\block{Ax - b} + \frac{1}{2\rho}\norm{\lambda_k}^2$$

Therefore, we can introduce $$u_k = \frac{\lambda_k}{\rho}$$ and the ALM
simplifies to the following:

1. initialize $$u_0 = 0$$
2. solve $$x_k = \argmin{x}\ f(x) + \frac{\rho}{2}\norm{Ax - b - u_k}^2$$
3. update $$u_{k+1} = u_k - \block{A x_k - b}$$
4. goto 2 until sufficient precision is achieved (more on this below)

which is a bit more convenient in practice.

# Alternating Direction Method of Multipliers

So far, so good: we solved the problem of choosing step sizes
$$\alpha_k$$ and still get convergence. One practical issue is that
the penalty term $$\norm{Ax - b}^2$$ introduces coupling between
variables that may not appear in function $$f$$: while dual ascent
could optimize a separable function $$f(x) = g(y) + h(z)$$ well,
separately (possibly using dedicated, optimized solvers), this is no
longer possible with the Method of Multipliers.

The *Alternating Direction Method of Multipliers* (ADMM) improves the
situation by working around the coupling introduced by constraint
matrix. Let us introduce some notation first: we consider the
following problem of minimizing a separable function under affine
constraints:

$$\min_{x, z}\quad f(x) + g(z)\quad\st\ Ax + Bz = c$$

Instead of minimizing *jointly* over both $$x, z$$ like the Method of
Multipliers would, the minimization is split into two subproblems: minimizing
along $$x$$ alone first with $$z$$ constant, then along $$z$$ alone with $$x$$
constant, in a Gauss-Seidel fashion:

1. initialize $$\lambda_0 = 0, z_0 = 0$$
2. solve $$x_k = \argmin{x}\quad f(x) - \lambda_k^T\block{Ax + B z_k - c} + \frac{\rho}{2}\norm{Ax + B z_k - c}^2$$
3. solve $$z_{k+1} = \argmin{z}\quad g(z) - \lambda_k^T\block{Ax_k + Bz - c} + \frac{\rho}{2}\norm{Ax_k + Bz - c}^2$$
4. update $$\lambda_{k+1} = \lambda_k - \rho \block{A x_k + Bz_{k+1} - c}$$
5. goto 2 until sufficient precision is achieved (more on this below)

This is equivalent to alternating two Method of Multiplier solves in the $$x,
z$$ directions with varying constraint values, hence the name. Crucially, the
matrices $$A, B$$ remain constant, which enables preprocessing so that inner
solves for $$x, z$$ are as efficient as possible. Unlike the Method of
Multipliers, it becomes possible to employ dedicated, optimized solvers for each
subproblems, establishing consensus as the iteration converges. 

Note that the role played by $$x,z$$ is *not* symmetric. Dual feasibility for $$x_k$$ gives:

$$\nabla f\block{x_k} - A^T \lambda_k + \rho A^T\block{Ax_k + Bz_k - c} = 0$$

while dual feasibility for $$z_{k+1}$$ gives:

$$\underbrace{\nabla g\block{z_{k+1}} - B^T \lambda_k + \rho B^T\block{Ax_k + Bz_{k+1} - c}}_{\nabla g\block{z_{k+1}} - B^T \lambda_{k+1}} = 0$$

Therefore, $$z_{k+1}, \lambda_{k+1}$$ is automatically dual-feasible for the
original problem, while $$x_k, \lambda_{k+1}$$ is not:

$$\begin{aligned}
\nabla f\block{x_k} &- A^T \lambda_k + \rho A^T\block{Ax_k + Bz_k - c} \\
= \nabla f\block{x_k} &- A^T \block{\lambda_k - \rho \block{Ax_k + Bz_k - c}} \\
= \nabla f\block{x_k} &- A^T\block{\lambda_{k+1} - \rho Bz_{k+1} + \rho Bz_k} \\
= \nabla f\block{x_k} &- A^T\lambda_{k+1} - \rho A^TB\block{z_{k+1} - z_k} \\
\end{aligned}$$

This suggests that convergence checks should not only consider the primal
residual $$\norm{Ax_k + Bz_{k+1} - c}$$ (consensus), but also the dual residual
$$\rho\norm{A^TB\block{z_{k+1} - z_k}}$$, as described below.

For proving convergence, we consider the following energy:

$$W_k = \frac{1}{\rho} {\underbrace{\norm{\lambda_k - \lambda^\star}}_{V_k}^{}}^2 + \rho {\underbrace{\norm{B\block{z_k - z^\star}}}_{U_k}^{}}^2$$


## Scaled Form

As [before](#method-of-multipliers), introducing $$u_k =
\frac{\lambda_k}{\rho}$$ yields slightly more convenient equations in practice:

1. initialize $$u_0 = 0, z_0 = 0$$
2. solve $$x_k = \argmin{x}\ f(x) + \frac{\rho}{2}\norm{Ax + B z_k - c - u_k}^2$$
3. solve $$z_{k+1} = \argmin{z}\ g(z) + \frac{\rho}{2}\norm{Ax_k + Bz - c - u_k}^2$$
4. update $$u_{k+1} = u_k - \block{A x_k + Bz_{k+1} - c}$$
5. goto 2 until sufficient precision is achieved (more on this below)

Expanding quadratic terms gives

$$\begin{aligned}
x_k &= \argmin{x}\ f(x) + \frac{\rho}{2}x^TA^TAx + \rho x^TA^T\block{B z_k - c - u_k} \\
z_{k+1} &= \argmin{z}\ g(z) + \frac{\rho}{2}z^TB^TBz + \rho z^T\block{Ax_k - c - u_k} \\
\end{aligned}
$$

In particular, for the *consensus* problem $$A=I, B=-I, c = 0$$ this gives:

$$\begin{aligned}
x_k &= \argmin{x}\ f(x) + \frac{\rho}{2}\norm{x}^2 - \rho x^T\block{u_k + z_k} \\
z_{k+1} &= \argmin{z}\ g(z) + \frac{\rho}{2}\norm{z}^2 - \rho z^T\block{u_k - x_k} \\
\end{aligned}
$$


## Stopping Criterion



# References 

[^boyd]: See [https://web.stanford.edu/~boyd/admm.html](https://web.stanford.edu/~boyd/admm.html)

