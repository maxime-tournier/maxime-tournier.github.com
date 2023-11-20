---
title: Geometric Stiffness for SO(3)
categories: [math]
---

{% include toc.md %}

When simulating rigid bodies, one generally ends up with non-symmetric Hessian
matrices due to the curvature of $$SO(3)$$ (where Schwartz' theorem about the
symmetry of Hessian does not hold), which do not play well with numerical
solvers down the road. More generally, second derivatives are a pain to work
with since there are several definitiions to choose from (e.g. Lie derivative,
Levi-Civita connection) which do not always agree. Even though there is
sometimes a natural choice for some spaces ($$SO(3)$$ which is compact and as
such enjoys a bi-invariant Riemannian metric), for others there is none
($$SE(3)$$ which does not have a bi-invariant Riemannian metric) and it is not
always clear what to do in such situations.

Putting theoretical considerations aside, one can simply remark that in
practical applications like physics simulation, one generally uses local charts
for these spaces anyways (for instance, using body-fixed velocity exponential
for rotation integration), therefore an obvious solution is to work inside these
charts, which side-steps the problem entirely. This means one must keep track of
the local chart all along, together with their derivatives. These notes describe
the procedure for exponential/logarithmic charts.

# Functions from $$SO(3)$$

Let $$f: SO(3) \to E$$, with $$E$$ some finite dimensional vector space. Let
$$R$$ be the current input, we parametrize $$SO(3)$$ locally using the exponential:

$$\begin{align}
\hat{f}^s: \quad \RR^3 &\to E \\
\omega &\mapsto f(\exp(\omega)R) = \hat{f}^s(\omega)\\
\end{align}$$

We'll call this $$\hat{f}^s$$ the *spatial* parametrization of $$f$$ at
$$R$$. Similarly, the *body-fixed* parametrization at $$R$$ is:

$$\begin{align}
\hat{f}^b: \quad \RR^3 &\to E \\
\omega &\mapsto f(R\exp(\omega)) = \hat{f}^b(\omega)\\
\end{align}$$

## Jacobian

$$\dd \hat{f}^s(\omega).\dd \omega = \dd f(\exp(\omega)R).\dd \exp(\omega).\hat{\dd \omega}.R$$

which at $$\omega=0$$ gives:

$$\dd \hat{f}^s(0).\dd \omega = \dd f(R).\hat{\dd \omega} R$$

therefore $$\dd \omega$$ plays the role of a spatial velocity. So we get: 

$$\dd \hat{f}^s(0).\dd \omega = \dd^s f(R).\dd \omega$$

Likewise:

$$\dd \hat{f}^b(0).\dd \omega = \dd f(R).R \hat{\dd \omega}$$

and this time $$\dd \omega$$ plays the role of a body-fixed velocity:

$$\dd \hat{f}^s(0).\dd \omega = \dd^s f(R).\dd \omega$$

## Hessian

$$\begin{align}
\dd^2 \hat{f}^s(\omega).\dd \omega_2.\dd \omega_1 &= \dd^2 f(\exp(\omega)R).\dd \exp(\omega).\hat{\dd \omega_2} R.\dd \exp(\omega).\hat{\dd \omega_1} R \\
&+ \dd f(\exp(\omega)R).\dd^2 \exp(\omega).\hat{\dd \omega_2}.\hat{\dd \omega_1} R \\
\end{align}
$$

which at $$\omega=0$$ gives:

$$\begin{align}
\dd^2 \hat{f}^s(0).\dd \omega_2.\dd \omega_1 &= \dd^2 f(R).\hat{\dd \omega_2} R.\hat{\dd \omega_1} R + \dd f(R).\frac{\hat{\dd \omega_1}\hat{\dd \omega_2} + \hat{\dd \omega_2} \hat{\dd \omega_1}}{2}R \\ 
&= \dd^2 f(R).\hat{\dd \omega_2} R.\hat{\dd \omega_1} R + \dd f(R).\block{\frac{\dd \omega_1 \dd \omega_2^T + \dd \omega_2 \dd \omega_1^T}{2} - \dd \omega_2^T \dd \omega_1 I}R
\end{align}
$$


## Geometric Stiffness

Let us now introduce a generalized end-force $$\lambda \in E$$ and consider the
following pairing:

$$\trace{\lambda^T\dd^2 \hat{f}^s(0).\dd \omega_2.\dd \omega_1}$$

### Point Mapping

$$f(R) = Rx$$ for some fixed $$x \in \RR^3$$. $$f$$ is linear therefore:

$$\dd f(R).\dd R = \dd R.x = f(\dd R)$$

$$\dd^2 f = 0$$

Given an end-force $$\lambda \in \RR^3$$, the geometric stiffness is:


$$\begin{align}
\trace{\lambda^T\dd^2 \hat{f}^s(0).\dd \omega_2.\dd \omega_1} &= \trace{\lambda^T.\block{\frac{\dd \omega_1 \dd \omega_2^T + \dd \omega_2 \dd \omega_1^T}{2} - \dd \omega_2^T \dd \omega_1 I}Rx} \\
&= \trace{Rx\lambda^T.\block{\frac{\dd \omega_1 \dd \omega_2^T + \dd \omega_2 \dd \omega_1^T}{2} - \dd \omega_2^T \dd \omega_1 I}} \\
&= \dd \omega_2^T\frac{Rx \lambda^T + \lambda (Rx)^T}{2}\dd \omega_1 - \trace{\lambda^TRx} \dd \omega_2^T \dd \omega_1 \\
&= \dd \omega_2^T\frac{\hat{Rx}\hat{\lambda} + \hat{\lambda}\hat{Rx}}{2}\dd \omega_1 \\
\end{align}
$$


# Functions to $$SO(3)$$

$$f: E \to SO(3)$$

Let $$R = f\block{\bar{x}}$$ for some input $$\bar{x}$$, we parametrize the
codomain around $$R$$ using the logarithm:

$$\begin{align}
\hat{f}^s: \quad E &\to \RR^3 \\
x &\mapsto \log\block{f(x)\inv{R}} \\
\end{align}$$

Similarly:

$$\begin{align}
\hat{f}^s: \quad E &\to \RR^3 \\
x &\mapsto \log\block{\inv{R}f(x)} \\
\end{align}$$

## Jacobian

$$\dd \hat{f}^s(x).\dd x = \dd \log\block{f(x)\inv{R}}.\dd f(x).\dd x.\inv{R}$$

which at $$\bar{x}$$ gives:

$$\begin{align}
\dd \hat{f}^s\block{\bar{x}}.\dd x &= \dd f(x).\dd x.\inv{R} \\
&= \dd^s f(x).\dd x\\
\end{align}
$$

this corresponds to the spatial velocity.

## Hessian

$$\begin{align}
\dd^2 \hat{f}^s(x).\dd x_2.\dd x_1 &= \dd^2 \log\block{f(x)\inv{R}}.\dd f(x).\dd x_2.\inv{R}. \dd f(x).\dd x_1.\inv{R}\\
&+ \dd \log\block{f(x)\inv{R}}.\dd^2 f(x).\dd x_2.\dd x_1.\inv{R}
\end{align}$$

which at $$x = \bar{x}$$ reduces to:

$$\begin{align}
\dd^2 \hat{f}^s\block{\bar{x}}.\dd x_2.\dd x_1 &= \dd^2 \log(I).\dd f(x).\dd x_2.\inv{R}. \dd f(x).\dd x_1.\inv{R}\\
&+ \dd^2 f(x).\dd x_2.\dd x_1.\inv{R}
\end{align}$$

# Examples

## TODO Joint Logarithmic Coordinates

(using spatial parametrizations at $$R_p, R_c$$)

$$f\block{\omega_p, \omega_c} = \log\block{R_p^T\exp\block{-\omega_p}\exp\block{\omega_c}R_c}$$

Let $$g\block{\omega_p, \omega_c} = \exp\block{-\omega_p}\exp\block{\omega_c}$$,
we obtain the jacobian:

$$\dd f\block{\omega_p, \omega_c}.\dd \omega_p.\dd \omega_c = 
    \dd \log\block{R_p^T g\block{\omega_p, \omega_c}R_c}.R_p^T\block{\dd g\block{\omega_p, \omega_c}.\dd \omega_p.\dd \omega_c}R_c$$
    
where 

$$\dd g\block{\omega_p, \omega_c}.\dd \omega_p.\dd \omega_c = 
    -\dd \exp\block{-\omega_p}.\dd \omega_p.\exp\block{\omega_c} + \exp\block{-\omega_p}\dd \exp\block{\omega_c}.\dd \omega_c$$
    
    
