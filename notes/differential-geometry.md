---
title: Differential Geometry
categories: [math]
---

{% include toc.md %}


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

$$\dd f(x).\dd x = \frac{1}{2 \sqrt{x}}.\dd x$$

## Norm

$$f(x) = \norm{x} = \sqrt{\norm{x}^2}$$

$$\dd f(x).\dd x = \frac{x^T}{\norm{x}} \dd x$$

$$\nabla f(x) = \frac{x}{\norm{x}}$$

$$\dd^2 f(x).\dd x_1.\dd x_2 = \dd x_2^T \frac{1}{\norm{x}}\block{I - \frac{x}{\norm{x}}\frac{x^T}{\norm{x}}} \dd x_1$$

$$\nabla^2 f(x) = \frac{1}{\norm{x}} \block{I - \frac{x}{\norm{x}}\frac{x^T}{\norm{x}}}$$

## Inverse

$$f(x) = \frac{1}{x}$$

$$\dd f(x) = -\frac{1}{x^2}\dd x$$

## Normalization

$$f(x) = \frac{x}{\norm{x}}$$

$$
\begin{align}
\dd f(x).\dd x &= \frac{1}{\norm{x}}\dd x + x\block{-\frac{1}{\norm{x}^2}\frac{1}{\norm{x}} x^T \dd x}\\
&= \frac{1}{\norm{x}}\block{I - \frac{x}{\norm{x}}\frac{x^T}{\norm{x}}}.\dd x \\
\end{align}
$$
