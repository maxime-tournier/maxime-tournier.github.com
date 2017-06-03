---
title: Conic Optimization
categories: [math, optimization]
---


{% include toc.md %}


# Cones

$$x\in K \Rightarrow \lambda x \in K, \quad \forall \lambda \geq 0$$

# Examples

Positive orthant: $$x \geq 0$$

Second-order cone: $$(x, t) : \norm{x} \leq t$$

Positive definite matrices: $$M \succeq 0$$



# Dual Cone

$$K^* = \left\{ y \in E, \quad x^Ty \geq 0 \quad \forall x \in K\right\}$$

# Polar Cone
$$K^- = -K^*$$

# Moreau Decomposition

$$ z = x + y, \quad x \in K \  \bot \  y \in K^- \iff x = \pi_K(z),\ y = \pi_{K^-}(z)$$

# Convex Conic Programs

$$\argmin{x \in K} \quad \half x^T M x + q^T x \quad \iff \quad x \in K \ \bot \  Mx + q \in K^*$$ 

As a fixed point: 

$$x = \pi_K\block{x - (Mx + q)}$$

# Projection on the Second Order Cone

Given the second-order cone $$K = \left\{x: \norm{x_T} \leq x_N
\right\}$$ with normal/tangent subspaces $$N, T$$ we consider the
orthogonal projection problem:

$$x = \pi_K(p)$$

More precisely, we look for a solution to:

$$ \argmin{\norm{x_N}^2 \geq \norm{x_T}^2, \ x_N \geq 0}  \norm{x - p}^2 $$

which is a convex program. The gradient for constraint
$$\norm{x_N}^2 - \norm{x_T}^2 \geq 0$$ is $$x_N - x_T$$, and KKT conditions are:

$$ 
\begin{align}
x_N &= p_N + \lambda x_N + \mu \\
x_T &= p_T - \lambda x_N  \\
0 \leq \lambda \ &\bot\  \norm{x_N}^2 - \norm{x_T}^2 \geq 0 \\
0 \leq \mu \ &\bot \  x_N \geq 0
\end{align}
$$

Since $$\pi_K(x) = x$$ if $$x \in K$$ and testing whether $$x \in K$$
is easy, we will focus on the case where the first constraint is
active (which implies $$\norm{p_N}^2 \neq \norm{p_T}^2$$) and the
second is not ($$\mu = 0$$) and obtain the following conditions:

$$ 
\begin{align}
(1 - \lambda) x_N &= p_N \\
(1 + \lambda) x_T &= p_T   \\
\norm{x_N}^2 - \norm{x_T}^2 &= 0 \\
0 \leq \mu \ &\bot \  x_N \geq 0
\end{align}
$$

Let us assume for now that $$\norm{\lambda} \neq 1$$ and obtain:

$$ 
\begin{align}
x_N &= \frac{p_N}{1 - \lambda} \\
x_T &= \frac{p_T}{1 + \lambda}  \\
\norm{x_N}^2 - \norm{x_T}^2 &= 0 \\
\end{align}
$$

Subtitution gives:

$$(1 - \lambda)^2 \norm{p_T}^2 = (1 + \lambda)^2 \norm{p_N}^2$$

And finally:

$$\block{1 + \lambda^2}\block{ \norm{p_N}^2 - \norm{p_T}^2} + 2
\lambda \block{\norm{p_N}^2 + \norm{p_T}^2} = 0$$

which under our assumptions has always a solution given by:

$$\lambda = \frac{\norm{p}^2 \pm 2 \norm{p_T}\norm{p_N}}{\norm{p_T}^2 - \norm{p_N}^2}$$

Now if we are to use this formula for solving the original orthogonal
projection problem, we need to consider the sign of $$x_N$$
corresponding to $$\lambda$$ in order to rule out critical
points. More precisely, we have: $$\sign{x_N} = \sign{p_N}\sign{1 -
\lambda}$$

where:

$$1-\lambda = \frac{ 2 \norm{p_N}^2 \pm 2 \norm{p_T}\norm{p_N} } {\norm{p_T}^2 - \norm{p_N}^2}$$

Depending on the sign of $$\norm{p_T}^2 - \norm{p_N}^2$$ and that of
$$p_N$$, each $$\lambda$$ pair will result in either two positive, two
negative, or exactly one positive and one negative values for
$$x_N$$. One can verify that:

- $$\x_{N1} \geq 0, \x_{N2} \geq 0 \iff p \in K$$, in which case $$\pi_K(x) = x$$ 
- $$\x_{N1} \leq 0, \x_{N2} \leq 0 \iff p \in K^-$$, in which case $$\pi_K(x) = 0$$ 

In other cases, the solution is obtained from the root resulting in a
positive $$\x_N$$. The final projection procedure is the following:

- if $$\norm{p_N} = \norm{p_T}$$, $$x = p$$ or $$x = 0$$ based on $$\sign{p_N}$$
- else 
  1. Compute $$\lambda = \frac{\norm{p}^2 \pm 2 \norm{p_T}\norm{p_N}}{\norm{p_T}^2 - \norm{p_N}^2}$$
  2. Compute both values for $$x_N = \frac{p_n}{1 - \lambda}$$ and keep the one that is positive
  3. Compute $$x_T = \frac{p_T}{1 + \lambda}$$
  
It is easy to check that the two degenerate cases are handled
correctly by the above procedure:

- $$\lambda = 1$$ corresponds to $$p_N = 0$$
- $$\lambda = -1$$ corresponds to $$p_T = 0$$


