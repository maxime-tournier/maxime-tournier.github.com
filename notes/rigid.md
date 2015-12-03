---
title: Rigid Bodies
categories: [math, lie group]
---

Quick notes.

{% include toc.md %}

# Lie Group

As a subgroup of $$GL(4)$$:

$$ SE(3) = \left\{ \mat{R & t \\ 0 & 1},\quad R \in SO(3),\ t \in \RR^3 \right\} $$

## Product

$$ \mat{R_1 & t_1 \\ 0 & 1} \mat{R_2 & t_2 \\ 0 & 1} = \mat{ R_1 R_2 & R_1 t_2 + t_1 \\ 0 & 1} $$

## Inverse

$$ \mat{R & t \\ 0 & 1}^{-1} = \mat{\inv{R} & -\inv{R}t \\ 0 & 1} $$

## Lie Algebra

As a subalgebra of $$\alg{gl(4)}$$:
	
$$\alg{se(3)} = \left\{ \mat{ \omega & v \\ 0 & 0 }, \quad \omega \in \alg{so(3)}, \ v\in\RR^3 \right \} $$

## Adjoint Representations

Using $$\RR^6$$ coordinates for $$\alg{se(3)}$$:

$$\Ad_{R, t} = \mat{ R & \\ \hat{t}R & R } $$

$$\ad \mat{\omega\\ v} = \mat{ \hat{\omega} & \\ \hat{v} & \hat{\omega} }$$

## Exponential

## Logarithm

# Rigid Bodies

## Inertia Tensor

## Newton-Euler Equations

## Time Integration

Parallel transport the spatial angular momentum $$\mu_k^s$$ from
$$R_k$$ to $$R_{k+1}$$. Constant spatial angular momentum gives:

$$ \mu_{k+1}^s = \mu_k^s$$

On body-fixed angular momenta:

$$ \mu_{k+1}^b = \Ad_{R_k^TR_{k+1}}^T \mu_k^b$$

On body-fixed angular velocities:

$$ \omega^b_{k+1} = \mathcal{I}_b^{-1} \Ad_{R_k^TR_{k+1}}^T \mathcal{I}_b \omega_k^b $$

Note: differentiating at $$R_{k+1} = R_k$$ gives the Newton-Euler
equations.

# Simulation

While choosing a body-fixed or spatial reference frame has advantages
for a theoretic study of $$SE(3)$$, it is not ideal for simulating
rigid bodies because it ties the linear and angular dynamics
together. But as seen before, these two dynamics can be decoupled when
the reference frame is chosen appropriately.

## Reference Frame

The configuration of a rigid body will be expressed using a rigid
transform, where the rotation matches that of the principal axis of
inertia of the solid, and the translation will correspond to the
center of mass of the object.

We further decouple the angular/linear dynamics by choosing a
body-fixed frame for angular velocities, and an absolute frame for
linear velocities. Similar to body-fixed/spatial frames, we index this
frame using a $$^r$$ superscript.

## Mappings

### Left Translation

$$\dd^r L_h g = \mat{ I & \\  & R_h}$$

### Right Translation

$$\dd^r R_h g = \mat{ Ad_{R_h^T} & \\ -R_g \hat{t_h} & I}$$

$$
\begin{align}
\dd^r R_h^T(g).\lambda &= \mat{ Ad_{R_h^T}^T & \hat{t_h} R_g^T\\  & I} \mat{\tau \\ f}\\
&= \mat{Ad_{R_h^T}^T \tau + \hat{t_h} R_g^T f \\ f}
\end{align}
$$

$$\dd_r^2 R_h^T(g).\lambda = \mat{ - \hat{t_h}\hat{R_g^Tf} & 0 \\ 0 & 0 } $$


### Point Mapping

Let $$x \in \RR^3$$ be a fixed point in our rigid frame, we consider
the mapping to the absolute coordinates:

$$s: (R, t) \mapsto Rx + t$$

$$ \dd^r s(g) = \mat{-R \hat{x} & I } $$

$$ \dd^r s(g)^T f = \mat{\hat{x}R^Tf \\ f} $$

$$ \dd_r^2 s(g)^T f = \mat{\hat{x} \hat{R^Tf} & 0 \\ 0 & 0} $$

### Inverse

$$ \dd^r \inv{g} = \mat{-\Ad_R & \\ -\hat{R^Tt} & -R^T} $$

$$
\begin{align}
\dd^r {\inv{g}}^T \lambda &= \mat{-\Ad_R^T & \hat{R^Tt} \\  & -R} \mat{\tau^b \\ f^r} \\
&= \mat{-\Ad_R^T \tau - \hat{f} R^Tt \\  -R f}
\end{align}
$$

Now the geometric stiffness for this one is quite a beast:


### Product





