---
title: Rigid Bodies
categories: [math, lie group]
---

Quick notes.

{% include toc.md %}

# Lie Group Structure

As a subgroup of $$GL(4)$$:

$$ SE(3) = \left\{ \mat{R & t \\ 0 & 1},\quad R \in SO(3),\ t \in \RR^3 \right\} $$

## Product

$$ \mat{R_1 & t_1 \\ 0 & 1} \mat{R_2 & t_2 \\ 0 & 1} = \mat{ R_1 R_2 & R_1 t_2 + t_1 \\ 0 & 1} $$

## Inverse

$$ \mat{R & t \\ 0 & 1}^{-1} = \mat{\inv{R} & -\inv{R}t \\ 0 & 1} $$

## Lie Algebra

As a subalgebra of $$\alg{gl(4)}$$:
	
$$\alg{se(3)} = \left\{ \mat{ \omega & v \\ 0 & 0 }, \quad \omega \in \alg{so(3)}, \ v\in\RR^3 \right \} $$

## Twists

Twists are $$\RR^6$$ coordinates for $$\alg{se(3)}$$ as follows:

$$\mat{\omega \\ v} \simeq \mat{ \hat{\omega} & v \\ 0 & 0}$$

where $$\hat{\omega} x = \omega \times x$$ is the cross-product matrix
(see [quaternions](quaternions.html)).

## Wrenches

Similarly, wrenches are $$\RR^6$$ coordinates for the Lie coalgebra
$$\alg{se(3)}^\star$$.

## Adjoint Representations

Group adjoint representation on twists:

$$\Ad_{R, t} = \mat{ R & \\ \hat{t}R & R } $$

Algebra adjoint representation on twists:

$$\ad \mat{\omega\\ v} = \mat{ \hat{\omega} & \\ \hat{v} & \hat{\omega} }$$

## Exponential

Given a Lie algebra element:

$$\mat{\omega & v \\ 0 & 0}$$

It is easy to see that:

$$\mat{\omega & v \\ 0 & 0}^i = \mat{\omega^i & \omega^{i-1} v \\ 0 & 0}$$

for $$i \geq 1$$. So the exponential power series becomes:

$$ \sum_{i=0} \frac{1}{i!} \mat{\omega & v \\ 0 & 0}^i = \mat{ \sum_i \frac{\omega^i}{i!} & \sum_i \frac{\omega^i}{(i + 1)!} v \\ 0 & 1}$$

One can recognize $$\sum_i \frac{\omega^i}{(i + 1)!}$$ as the power
series for the $$SO(3)$$ exponential derivative, while $$ \sum_i
\frac{\omega^i}{i!}$$ is the $$SO(3)$$ exponential, which gives:

$$ \exp\mat{\omega & v \\ 0 & 0} = \mat{ \exp(\omega) & \dd\exp(\omega) v \\ 0 & 1} $$

See the page on [quaternions](quaternions.html) for the corresponding
formula (modulo a factor $$2$$).

## Logarithm

From the above formula for the exponential, the logarithm (where
defined) is the inverse function:

$$ \log\mat{R & t \\ 0 & 1} = \mat{ \log(R) & \dd\log(R) t \\ 0 & 0} $$


# Rigid Bodies

## Inertia Tensor

TODO

## Newton-Euler Equations

TODO

## Time Integration

Parallel transport of the spatial angular momentum $$\mu_k^s$$ from
$$R_k$$ to $$R_{k+1}$$. Constant spatial angular momentum gives:

$$ \mu_{k+1}^s = \mu_k^s$$

On body-fixed angular momenta:

$$ \mu_{k+1}^b = \Ad_{R_k^TR_{k+1}}^T \mu_k^b$$

On body-fixed angular velocities:

$$ \omega^b_{k+1} = \mathcal{I}_b^{-1} \Ad_{R_k^TR_{k+1}}^T \mathcal{I}_b \omega_k^b $$

Note: differentiating at $$R_{k+1} = R_k$$ gives the Newton-Euler
equations.

# Kinematics

While choosing a body-fixed or spatial reference frame has advantages
for a theoretic study of $$SE(3)$$, it is not ideal for simulating
rigid bodies in practice because it ties the linear and angular
dynamics together. But as seen before, these two dynamics can be
decoupled when the reference frame is chosen appropriately.

## Reference Frame

The configuration of a rigid body will be expressed using a rigid
transform, where the rotation matches that of the principal axis of
inertia of the object, and the translation will correspond to the
center of mass.

We further decouple the angular/linear dynamics by choosing a
body-fixed frame for angular velocities, and an absolute frame for
linear velocities. Similar to body-fixed/spatial frames, we index this
frame using a $$^r$$ superscript.

## Left Translation

$$\dd^r L_h g = \mat{ I & \\  & R_h}$$

## Right Translation

$$\dd^r R_h g = \mat{ Ad_{R_h^T} & \\ -R_g \hat{t_h} & I}$$

$$
\begin{align}
\dd^r R_h^T(g).\lambda &= \mat{ Ad_{R_h^T}^T & \hat{t_h} R_g^T\\  & I} \mat{\tau \\ f}\\
&= \mat{Ad_{R_h^T}^T \tau + \hat{t_h} R_g^T f \\ f}
\end{align}
$$

$$\dd_r^2 R_h^T(g).\lambda = \mat{ - \hat{t_h}\hat{R_g^Tf} & 0 \\ 0 & 0 } $$


## Point Mapping

Let $$x \in \RR^3$$ be a fixed point in our rigid frame, we consider
the mapping to the absolute coordinates:

$$s: (R, t) \mapsto Rx + t$$

$$ \dd^r s(g) = \mat{-R \hat{x} & I } $$

$$ \dd^r s(g)^T f = \mat{\hat{x}R^Tf \\ f} $$

$$ \dd_r^2 s(g)^T f = \mat{\hat{x} \hat{R^Tf} & 0 \\ 0 & 0} $$

## Inverse

$$ \dd^r \inv{g} = \mat{-\Ad_R & \\ -\hat{R^Tt} & -R^T} $$

$$
\begin{align}
\dd^r {\inv{g}}^T \lambda &= \mat{-\Ad_R^T & \hat{R^Tt} \\  & -R} \mat{\tau^b \\ f^r} \\
&= \mat{-\Ad_R^T \tau - \hat{f} R^Tt \\  -R f}
\end{align}
$$

Now the geometric stiffness for this one is quite a beast. Let's focus
on the rotational part only for now:

$$
\begin{align}
\dd \Ad_R^T\tau.\db R &= \block{\Ad_R \ad\block{\db R} }^T \tau \\ 
&= \ad\block{\db R}^T \underbrace{Ad_R^T \tau}_\mu \\
\end{align}
$$

Now, using the fact that the *coadjoint* representation of the Lie
algebra:

$$ \ad^\star(x) = -\ad(x)^T $$

arising from the coadjoint representation of the group $$\Ad^\star(g)
= -\Ad\block{\inv{g}}^T$$, is indeed a Lie algebra representation, it
is antisymmetric:

$$\ad^\star(x) y = - \ad^\star(y)x$$

Therefore, we get:

$$ \dd \Ad_R^T\tau.\db R = -\ad\block{\mu}^T \db R $$

Of course, in the special case of rotations we also have $$\Ad_R^T =
R^T$$, so that:

$$
\begin{align}
\dd \Ad_R^T\tau.\db R &= \dd R^T \tau \\
&= \db R^T R^T \tau \\
&= \hat{\mu}\db R \\
&= - \ad\block{\mu}^T \db R
\end{align}
$$

and we land back on our feet.

## Product

Here again, only on rotations:

$$ \db (ab) = \mat{\db R_b & \db L_a} = \mat{\Ad_\inv{b} & I}$$

$$ \db (ab)^T\tau = \mat{ \Ad_{\inv{b}}^T \tau \\ \tau } $$


$$
\begin{align}
\db \Ad_{\inv{b}}^T \tau.\db b &= \block{\Ad_\inv{b} \ad\block{ \db \inv{b}.\db b} }^T \tau \\
&= \ad^T\block{  \db \inv{b}.\db b } \underbrace{\Ad_{\inv{b}}^T\tau}_\mu \\
&= -\ad^T\block{\mu} \db \inv{b}.\db b \quad \quad \text{(cf. inverse section above)} \\ 
&= \ad^T\block{\mu} \Ad_b.\db b \\
&= -\hat{\mu} R_b \\
\end{align}
$$

$$ \dd_b^2 (ab)^T \tau = \mat{0 & \ad^T\block{\mu} \Ad_b \\ 0 & 0 } $$

## Left Difference

On rotations:

$$ \db \block{\inv{a}b} = \mat{-\Ad_{\inv{b}a} & I}$$

$$ \db \block{\inv{a}b}^T\tau = \mat{ -\Ad_{\inv{b}a}^T \tau \\ \tau } $$

$$
\begin{align}
\db \Ad_{\inv{b}a}^T \tau.\block{\db a, \db b} &=
-\ad^T\underbrace{\block{\Ad_{\inv{b}a}^T\tau}}_\mu \block{\db \block{\inv{b} a}.\block{\db a, \db b}} \\
&= -\ad^T(\mu) \block{ \db a - \Ad_{\inv{a}b} \db b}
\end{align}
$$

$$ \dd_b^2 \block{\inv{a}b}^T \tau = \mat{ -\ad^T(\mu) & \ad^T(\mu) \Ad_{\inv{a}b} \\ 0 & 0 } $$


