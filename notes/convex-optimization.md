---
title: Convex Optimization
categories: [math]
---

{% include toc.md %}


# Convexity

TODO

# Projection on a Convex Set

Let $$C$$ be a closed convex set. the projection of a point $$x$$ onto $$C$$ minimizes
the Euclidean norm (squared):

$$\pi_C(x) = \argmin{y \in C}\ \norm{x - y}^2$$

## Existence

TODO (weierstrass)


## Unicity

Assume there are two distinct $$c_1, c_2$$ that both minimize the distance
$$d^\star$$ to $$C$$. The midpoint of $$c_1, c_2$$ belongs to $$C$$ and its
distance squared $$d$$ to $$C$$ is:

$$\begin{aligned}
d = \norm{x - \frac{c_1 + c_2}{2}}^2 &= \norm{x - c_1 + \frac{c_1 - c_2}{2}}^2 \\
&= \underbrace{\norm{x - c_1}^2}_{d^\star} + \block{x - c_1}^T\block{c_1 - c_2} + \frac{1}{4}\norm{c_1 - c_2}^2 \\
&= \underbrace{\norm{x - c_2}^2}_{d^\star} + \block{x - c_2}^T\block{c_2 - c_1} + \frac{1}{4}\norm{c_2 - c_1}^2 \\
\end{aligned}$$

Therefore we get $$2d = 2d^\star - \frac{1}{2}\norm{c_2 - c_1}^2$$ which
contradicts the fact that $$c_1, c_2$$ are minimizers.

## Characterization

From the convexity of $$C$$ one can derive a characterization of the projection
similar to the stationary conditions in optimization as follows. Let us call $$c
= \pi_C(x)$$, for any $$y \in C$$ the point $$(1 - \lambda) c + \lambda y$$
belongs to $$C$$ for $$\lambda \in [0, 1]$$, therefore:

$$\begin{aligned}
\norm{x - c}^2 &\leq \norm{x - (1 - \lambda) c + \lambda y}^2 \\
 &= \norm{x - c + \lambda\block{c - y}}^2 \\
 &= \norm{x - c}^2 + \lambda\block{x - c}^T\block{c - y} + \lambda^2 \norm{c - y}^2 \\
 \end{aligned}$$
 
This means that for any $$\lambda \in [0, 1]$$ we have:

$$\lambda \block{x - c}^T{y - c} \leq \lambda^2 \norm{y - c}$$

which for all $$\lambda \in ]0, 1]$$ gives: 

$$\block{x - c}^T{y - c} \leq \lambda \norm{y - c}$$ 
 
Therefore $$\block{x - c}^T{y - c} \leq 0$$. Conversely, if $$\block{x -
c}^T{y - c} \leq 0$$ we get

$$\begin{aligned}
\norm{x - y}^2 &= \norm{x - c + c - y}^2 \\
&= \norm{x - c}^2 + \underbrace{2 \block{x - c}^T\block{c - y}}_{\geq 0} + \underbrace{\norm{c - y}^2}_{\geq 0} \\
\end{aligned}$$
 
therefore $$\norm{x - y}^2 \geq \norm{x - c}^2$$ and $$c = \pi_C(x)$$.
 

# Convex Cones

## Moreau Decomposition


