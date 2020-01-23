---
title: Rigid-Scales
categories: [math]
---

Quick notes on Rigid-Scale kinematics.

{% include toc.md %}


# Affine Transformation

$$f: \RR^3 \times \RR^3 \times \RR^3 \to \mathrm{GA}(3)$$

## Mapping

$$f(\omega, s, t) = \block{\exp(\omega)RS, t}$$

where $$S = \diag(s)$$.

## Jacobian

$$\dd f = \block{\dd \exp(\omega).\dd \omega\ RS + \exp(\omega)R\dd S, \dd t}$$

$$\omega = 0: \quad \dd f = \block{\dd \omega\ RS + R\dd S, \dd t}$$

## Hessian

$$\dd^2 f = \block{\dd^2 \exp(\omega).\dd \omega_2.\dd \omega_1\ RS + \dd \exp(\omega).\dd \omega_1 R \dd S_2 + \dd \exp(\omega).\dd \omega_2 R\dd S_1, 0}$$

$$\newcommand{\ddexp}[2]{\frac{\hat{#1} \hat{#2} + \hat{#2} \hat{#1}}2}$$

$$\omega = 0: \quad \dd^2 f = \block{\ddexp{\dd \omega_1}{\dd \omega_2}RS + \dd \omega_1 R \dd S_2 + \dd \omega_2 R \dd S_1, 0}$$

## Geometric Stiffness

$$
\trace{\block{\lambda, \lambda_t}^T \dd^2 f}_{\omega=0} = \trace{\lambda^T \ddexp{\dd \omega_1}{\dd \omega_2}RS} + \trace{\lambda^T\dd \omega_1 R \dd S_2} + \trace{\lambda^T\dd \omega_2 R \dd S_1}
$$

Since: 

$$\ddexp{\dd \omega_1}{\dd \omega_2} = \frac{\dd \omega_1 \dd \omega_2^T + \dd \omega_2 \dd \omega_1^T}{2} - \dd \omega_1^T \dd \omega_2I$$

We get:

$$
\begin{align}
\trace{\lambda^T \ddexp{\dd \omega_1}{\dd \omega_2}RS} &= 
\trace{\underbrace{RS\lambda^T}_{K^T} \ddexp{\dd \omega_1}{\dd \omega_2}} \\
&= \trace{K^T  \frac{\dd \omega_1 \dd \omega_2^T + \dd \omega_2 \dd \omega_1^T}{2}} - \trace{K^T\dd \omega_1^T \dd \omega_2I} \\
&= \dd \omega_2^T \frac{K + K^T}{2} \dd \omega_1 - \dd \omega_2^T \trace{K} \dd \omega_1 \\
\end{align}
$$

Finally:

$$
\begin{align}
\trace{\lambda^T\dd \omega_1 R \dd S_2} &= \trace{\lambda^TRR^T\dd \hat{\omega}_1 R \dd S_2} \\
&=\trace{\lambda^T R \hat{R^T \dd \omega_1}\dd S_2} \\
&= \sum_i e_i^T\block{\lambda^T R \hat{R^T \dd \omega_1}\dd S_2}e_i\\
&= \sum_i e_i^T\block{\lambda^T R \hat{R^T \dd \omega_1}e_i \dd S_{2_i}}\\
&= -\sum_i e_i^T\block{\lambda^T R \hat{e_i}R^T \dd \omega_1 \dd S_{2_i}}\\
&= -\sum_i e_i^T\block{\lambda^T \hat{Re_i} \dd \omega_1 \dd S_{2_i}}\\
&= -\block{\sum_i \dd S_{2_i}e_i^T\block{\lambda^T \hat{Re_i}}} \dd \omega_1\\
&= \block{\sum_i \dd S_{2_i}\block{\lambda_i^T \times R_i}^T} \dd \omega_1\\
&= \dd s_2^T \mat{\block{\lambda_i^T \times R_i}^T\\ \vdots} \dd \omega_1\\
\end{align}
$$

(phhew!)

# Deformation Field

$$g: \RR^3 \times \RR^3 \times \RR^3 \times \RR^3 \to \RR^3$$

## Mapping 

$$g(\omega, s, t, x) = \exp(\omega)RSx + t = f(\omega, s, t)\mat{x\\ 1}$$

## Jacobian

$$\begin{align}
\dd g &= \dd f \mat{x\\ 1} + f(\omega, s, t) \mat{\dd x\\ 0} \\
&=\block{\dd \omega\ RS + R\dd S}x + \dd t + RS \dd x \\
&= -\hat{RSx}\ \dd \omega + R\diag(x)\ \dd s + \dd t + RS\ \dd x \\
\end{align}$$

## Hessian

$$
\dd^2 g = \dd^2 f \mat{x\\ 1} + \dd f_1 \mat{\dd x_2\\ 0} + \dd f_2 \mat{\dd x_1\\ 0} 
$$

## Geometric Stiffness

$$\begin{align}
\trace{\lambda^T \dd^2 g} &= \trace{\lambda^T \dd^2 f \mat{x\\ 1}} + \trace{\lambda^T \dd f_1 \mat{\dd x_2\\ 0}} + \trace{\lambda^T \dd f_2 \mat{\dd x_1\\ 0}} \\
&= \trace{x\lambda^T \dd^2 f_{\omega, s}} + \trace{\lambda^T \dd f_1 \mat{\dd x_2\\ 0}} + \trace{\lambda^T \dd f_2 \mat{\dd x_1\\ 0}}
\end{align}$$


$$
\begin{align}
\trace{\lambda^T \dd f_2 \mat{\dd x_1\\ 0}} &= \trace{\lambda^T\block{\dd \hat{\omega_2} RS + R\dd S_2}.\dd x_1} \\
&= -\dd \omega_2^T \hat{\lambda}RS\dd x_1 + \dd s_2^T \diag\block{R^T \lambda} \dd x_1 \\
\end{align}
$$
