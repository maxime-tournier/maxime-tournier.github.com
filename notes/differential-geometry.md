---
title: Differential Geometry
categories: [math]
---

{% include toc.md %}

# Implicit Function Theorem

Consider a smooth function $$g: \RR^m \to \RR^n$$ and the preimage of $$0 \in
\RR^n$$ by $$g$$:

$$X = g^{-1}(0)$$

For instance, let us start with the case when $$g$$ is linear $$X =
\ker(g)$$. Calling $$G$$ the matrix of $$g$$, then $$X$$ is defined as:

$$X = {x \in \RR^n, \quad Gx = 0}$$

When $$m > n$$ and $$G$$ is full-rank (in this case: $$n$$), then all $$n \times
n$$ submatrices of $$G$$ are invertible. In particular, we may reorder and
partition coordinates so that

$$G = \mat{L & R}$$ 

where $$R \in GL(n)$$ is invertible and $$L$$ is $$n \times (m - n)$$. In this
case we obtain that:

$$\begin{align}
G x = 0 &\iff L x_L + R x_R = 0 \\
    &\iff x_R = \inv{R}L x_L \\
\end{align}
$$

In other words, $$x_R$$ can be seen as a (linear) function of $$x_L$$. The
*implicit function theorem* is an extension of this to the non-linear case.


Coming back to the non-linear case, we will now assume that $$g$$ is smooth and
that is differential $$\dd g$$ is full-rank (again: $$n$$) at some *interior*
point $$x \in \mathrm(X)$$ of $$X$$. As a consequence, there exists some open
set $$U \subset X$$ containing $$x$$ such that $$\dd g$$ is everywhere full-rank
on $$U$$. Since $$g$$ is constant on $$X$$, it is also constant on $$U$$, so
for every tangent vector $$\dd u$$, we get:

$$\dd g(u).\dd u = 0$$

Again, since $$\dd g(u)$$ is everywhere full-rank over $$U$$, we may
reorder/partition coordinates globally on $$U$$ such that

$$\dd g(u) = \mat{L(u) & R(u)}$$

with $$L(u), R(u)$$ as in the linear case. In particular, we also have:

$$
\begin{align}
\dd g(u).\dd u = 0 &\iff L(u).\dd u_L + R(u).\dd u_R = 0 \\
&\iff \dd u_R = \inv{R}(u)L(u).\dd u_L \\
\end{align}
$$

So that $$\dd u_R$$ can be seen as a function of $$\dd u_L$$. The key is that
$$L, R$$ are both smooth functions of $$u$$, and inverting $$R$$ is a smooth
operation as well (see [Lie groups](lie-groups)) so that $$\dd u_R$$ is a
*smooth* function of $$\dd u_L$$. If we fix a tangent vector $$\dd u_L$$ and an
initial condition $$u = \block{u_L, u_R}$$, then we obtain the following
ordinary differential equation for $$u_R$$:

$$\dot{u}_R = \inv{R}\block{u_L, u_R} L\block{u_L, u_R}.\dd u_L$$

(to be continued)

# Cheat Sheet

## Chain Rule

$$h(x) = f(g(x))$$

$$\dd h(x).\dd x = \dd f(g(x)).\dd g(x).\dd x$$

$$\dd^2 h(x).\dd x_1.\dd x_2 = \dd^2 f(g(x)).\dd g(x).\dd x_2.\dd g(x).\dd x_1 + \dd f(g(x)).\dd^2 g(x).\dd x_1.\dd x_2$$



## Inner Product

$$f(x, y) = x^T y$$

$$\dd f(x, y).\dd x, \dd y = \dd x^Ty + x^T \dd y$$

$$\nabla f(x, y) = \mat{y \\ x}$$ 

$$\dd^2 f(x, y).\dd x_1, \dd y_1.\dd x_2, \dd y_2 = \dd x_1^T\dd y_2 + \dd x_2^T \dd y_1$$

$$\nabla^2 f = \mat{0 & I \\ I & 0}$$

## Squared Norm

$$f(x, y) = \norm{x}^2 = x^T x$$

$$\dd f(x).\dd x = 2x^T \dd x$$

$$\nabla f(x) = 2x$$

$$\dd^2 f(x).\dd x_1.\dd x_2 = 2\dd x_2^T \dd x_1$$

$$\nabla^2 f(x) = 2I$$

## Squared Root

$$f(x) = \sqrt{x}$$

$$\dd f(x) = \frac{1}{2 \sqrt{x}}$$

$$\dd^2 f(x) = -\frac{1}{4x\sqrt{x}}$$

## Norm

$$f(x) = \norm{x} = \sqrt{\norm{x}^2}$$

$$\dd f(x).\dd x = \frac{x^T}{\norm{x}} \dd x$$

$$\nabla f(x) = \frac{x}{\norm{x}}$$

$$\dd^2 f(x).\dd x_1.\dd x_2 = \dd x_2^T \frac{1}{\norm{x}}\block{I - \frac{x}{\norm{x}}\frac{x^T}{\norm{x}}} \dd x_1$$

$$\nabla^2 f(x) = \frac{1}{\norm{x}} \block{I - \frac{x}{\norm{x}}\frac{x^T}{\norm{x}}}$$

## Inverse

$$f(x) = \frac{1}{x}$$

$$\dd f(x) = -\frac{1}{x^2}$$

$$\dd^2 f(x) = \frac{2}{x^3}$$

## Normalization

$$f(x) = \frac{x}{\norm{x}}$$

$$
\begin{align}
\dd f(x).\dd x &= \frac{1}{\norm{x}}\dd x + x\block{-\frac{1}{\norm{x}^2}\frac{1}{\norm{x}} x^T \dd x}\\
&= \frac{1}{\norm{x}}\block{I - \frac{x}{\norm{x}}\frac{x^T}{\norm{x}}}.\dd x \\
\end{align}
$$

## Cross Product

$$f(x, y) = x \times y$$

$$\begin{align}
\dd f(x, y).\dd x.\dd y &= \dd x \times y + x \times \dd y \\
&= \mat{-\hat{y} & \hat{x}} \mat{\dd x \\ \dd y}
\end{align}
$$

$$\begin{align}
\lambda^T\dd^2 f(x, y) &= \lambda^T \dd x_1 \times \dd y_2 + \lambda^T \dd x_2 \times \dd y_1 \\
&= -\dd x_1^T \hat{\lambda} \dd y_2 - \dd x_2^T \hat{\lambda} \dd y_1 \\
&= \dd y_2^T \hat{\lambda} \dd x_1 - \dd x_2^T \hat{\lambda} \dd y_1 \\
&= \mat{\dd x_2^T & \dd y_2^T} \mat{0 & -\hat{\lambda} \\ \hat{\lambda} & 0} \mat{\dd x_1 \\ \dd y_1} \\
\end{align}
$$

