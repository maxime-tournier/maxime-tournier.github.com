---
title: Rigid Bodies
categories: [math]
---

Quick reference notes on rigid transformations.

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
(see [quaternions](quaternions)).

## Wrenches

Similarly, wrenches are $$\RR^6$$ coordinates for the Lie coalgebra
$$\alg{se(3)}^\star$$.

## Adjoint Representations

Adjoint representation of the **group** on its Lie algebra:

$$\Ad_{R, t} = \mat{ R & \\ \hat{t}R & R } $$

Adjoint representation of the **algebra** on itself:

$$\ad \mat{\omega\\ v} = \mat{ \hat{\omega} & \\ \hat{v} & \hat{\omega} }$$

## Exponential

Given $$(\omega, v) \in \alg{se(3)}$$ of the Lie algebra:

$$\mat{\omega & v \\ 0 & 0}$$

It is easy to see that:

$$\mat{\omega & v \\ 0 & 0}^i = \mat{\omega^i & \omega^{i-1} v \\ 0 & 0}$$

for $$i \geq 1$$. Therefore the exponential power series becomes:

$$ \sum_{i=0} \frac{1}{i!} \mat{\omega & v \\ 0 & 0}^i = \mat{ \sum_i \frac{\omega^i}{i!} & \sum_i \frac{\omega^i}{(i + 1)!} v \\ 0 & 1}$$


One can immediately recognize $$ \sum_i \frac{\omega^i}{i!}$$ as the
$$SO(3)$$ exponential. Due to the group being non-commutative, the
term $$\sum_i \frac{\omega^i}{(i + 1)!}$$ can not be identified with
the derivative of the exponential map (at least not
trivally). The
[derivative of the exponential map](https://en.wikipedia.org/wiki/Derivative_of_the_exponential_map) formula
tells us that:

$$\db \exp(x).\dd x = \sum_{i \geq 0} \frac{ (-1)^i}{(i + 1)!} \ad^i(x) . \dd x$$

When applied in $$\alg{s^3}$$, we obtain:

$$\db \exp\block{-\frac{\omega}{2}} = -\frac{1}{2} \sum_{i \geq 0} \frac{ \hat{\omega}^i}{(i + 1)!}$$

since $$\ad(\omega) = 2 \hat{\omega}$$. On the other hand, since
$$\exp(-x) = \exp^{-1}(x)$$, we also have:

$$\db \exp\block{-\frac{\omega}{2}} = -\frac{1}{2}\Ad_{\exp\block{ \frac{\omega}{2} } }.\db \exp\block{\frac{\omega}{2}}$$

and finally:

$$\sum_i \frac{\omega^i}{(i + 1)!} v = \Ad_{\exp\block{ \frac{\omega}{2} } }.\db \exp\block{\frac{\omega}{2}}. v$$

where the **quaternion** exponential is used. Alternatively, one can
use:

$$\sum_i \frac{\omega^i}{(i + 1)!} v = \exp(\omega) \block{\db \exp\block{\omega}.v}$$

using the body-fixed $$SO(3)$$ exponential in $$\alg{so(3)}$$
coordinates. The final formula is surprisingly simple:

$$ \exp\mat{\omega & v \\ 0 & 0} = \mat{ \exp(\omega) & \ds
\exp(\omega).v \\ 0 & 1} $$

An equivalent (but slightly faster) way is to express both $$\ad$$ and
$$\exp$$ in $$\alg{so(3)}$$ coordinates.

## Logarithm

From the above formula for the exponential, the logarithm (where
defined) is the inverse function:

$$ \log\mat{R & t \\ 0 & 1} = \mat{ \log(R) & \ds\log(R) t \\ 0 & 0} $$


# Rigid Bodies

## Inertia Tensor

Given a point attached to a rigid frame $$g = (R, t)$$ with local
coordinates $$p \in \RR^3$$, the position of the point in the world
frame is:

$$x = R.p + t$$

and its velocity is:

$$\dd x = \dd R.p + \dd t$$

If the point is given a mass $$m > 0$$, the corresponding kinetic
energy is:

$$
\begin{align}
T(g, \dd g) &= \half m \norm{\dd x}^2 \\
&= \half m \norm{R \hat{\omega}^b.p + \dd t}^2 \\
&= \half m \norm{\hat{\omega}^b.p + v^b}^2 \\
&= \half m \norm{-\hat{p}.\omega^b + v^b}^2 \\
&= \half m \mat{ {\omega^b}^T & {v^b}^T} \mat{-\hat{p}^T\hat{p} & \hat{p} \\ -\hat{p} & I} \mat{\omega^b \\ v^b}
\end{align}
$$

The symmetric matrix $$\hat{p}^T\hat{p}$$ decomposes as:

$$\hat{p}^T\hat{p} = -\hat{p}^2 = \norm{p}^2 I - p p^T$$

using the double cross-product formula. We end up with the following
inertia tensor for a single point mass, in body-fixed coordinates:

$$\mathcal{M} = m \mat{\norm{p}^2 I - p p^T & \hat{p} \\ -\hat{p} & I}$$

If we now use more than one point, the total kinetic energy is the sum
of the kinetic energies, and the total inertia tensor is the sum of
all the individual tensors:

$$\mathcal{M} = \sum_i m_i \mat{\norm{p_i}^2 I - p_i p_i^T & \hat{p_i} \\ -\hat{p_i} & I}$$

For a continuous object, the sum becomes an integral and the point
masses become mass densities:

$$\mathcal{M} = \int_\Omega \rho(p) \mat{\norm{p}^2 I - p p^T & \hat{p} \\ -\hat{p} & I}.\dd p$$

Coming back to discrete sums, we see that the top-right off-diagonal
term is $$\block{\sum_i m_i p_i}^{\hat{}}$$. If we now let $$m =
\sum_i m_i$$ the total mass of the object and $$c = \frac{\sum_i m_i
p_i}{m}$$ the coordinates of its center of mass, we see that the
off-diagonal terms $$m \hat{c}$$ become zero when the coordinate
system origin is chosen at the center of mass (since $$c = 0$$). From
now on, we will assume that this is indeed the case and that the
inertia tensor is block-diagonal:

$$\mathcal{M} = \mat{ \sum_i m_i\block{\norm{p_i}^2 I - p_i p_i^T} &&  \\ && m I }$$

The rotational inertia tensor $$\mathcal{I} = \sum_i
m_i\block{\norm{p_i}^2 I - p_i p_i^T}$$ is symmetric positive
semidefinite, and positive definite as long as the object has non-zero
volume. As such, it can be diagonalized in some orthogonal basis $$U \in SO(3)$$,
called the principal axes of inertia:

$$\mathcal{I} = U S U^T$$

## Parallel Axis Theorem

We now consider the following problem: *how do inertia tensors vary
under changes of coordinates?* A body-fixed coordinate change is
described using a right-translation by some constant element $$h\in
SE(3)$$:

$$\mathrm{R}_h: g \mapsto g h$$

Now, assuming we know the inertia tensor $$\mathcal{M}$$ in $$g h$$,
we have:

$$T(gh, \dd(gh)) = \db (gh)^T.\mathcal{M}.\db (gh)$$

We obtain the expression in terms of $$\db g$$ by differentiating:

$$\db(gh).\dd g = \db \mathrm{R}_h(g).\db g = \Ad_{\inv{g}}.\db g$$

Finally:

$$T(g, \db g) = \db g^T \underbrace{\Ad_h^{-T}.\mathcal{M}.\Ad_h^{-1}}_{\mathcal{M'}} \db g$$

$$\mathcal{M'}$$ is known as the *pullback* of the inertia tensor
$$\mathcal{M}$$ by $$R_h$$. The pulled-back tensor is the following,
again in body-fixed coordinates:

$$\mathcal{M'} = \mat{R_h.\mathcal{I}.R_h^T + m \block{ \norm{t_h}^2 I  - t_h t_h^T}& m \hat{t}_h \\ - m \hat{t}_h & m I}$$

It corresponds to the the rotation tensor $$\mathcal{I}$$ expressed in
the old coordinate frame, in addition to the inertia tensor for a
single point located at the center of mass. This result is known as
the
[parallel axis theorem](https://en.wikipedia.org/wiki/Parallel_axis_theorem).


## Moving Wrenches Around

Consider the right-translation by a constant translation:

$$\mathcal{R}_h: h = \mat{I & \delta \\ 0 & 1 }$$

We differentiate the right-translation using spatial coordinates at
$$g = (R, t)$$:

$$\dd \mathcal{R}(g) = \mat{ \dd R & \dd R \delta + \dd t \\ 0 & 0}$$

Then the pullback of a wrench $$\kappa = (\tau, f)$$ by $$\mathcal{R}_h$$ gives:

$$\dd^\star \mathcal{R}_h(g).\kappa = \mat{\tau + \hat{R\delta} \times f \\ f }$$

Calling $$A$$ the center of the translated frame and $$B$$ that the
original frame, we obtain for torques:

$$\tau_B = \tau_A + \vec{BA} \times f$$

which is known as the *Varignon* formula.

## Center of Pressure

We look to find a point in space where the equivalent wrench has
minimal torque. Assuming we are given a wrench $$\kappa = (\tau, f)$$
in spatial coordinates, the equivalent torque translated at $$x$$ is:

$$\tau_x = \tau + f \times x$$

Henceforth, we look to minimize the following function:

$$p = \argmin{x} \norm{ \tau + f \times x}^2 $$

We first notice that for any solution $$p$$, $$p + \lambda f$$ is also
solution, so it only makes sense to look for a solution that is
orthogonal to $$f$$:

$$x \in f^\bot$$

As a consequence, we also have $$f \times x \in f^\bot$$ and the
objective function can be replaced with:

$$p = \argmin{x \in f^\bot} \norm{ \tau^\bot + f \times x}^2 $$

One can easily check that this problem has a closed-form solution
given by:

$$p = \frac{f \times \tau^\bot}{\norm{f}^2} = \frac{f \times \tau}{\norm{f}^2}$$

Now, the center of pressure generally satisfies extra conditions, for
instance it should lie in the ground plane: 

$$n^T(p + \lambda f) = 0$$

which allows to compute $$\lambda$$ and the actual CoP.

## Newton-Euler Equations

TODO

## Scaling Inertia Tensors

Consider body-fixed diagonal scaling: 

$$S = (s_x, s_y, s_z)$$

The unscaled inertia tensor $$\mathcal{I}$$ satisfies:

$$
\begin{align}
\mathcal{I}_{xx} &= \int_\Omega \rho(p)\block{y^2 + z^2}.dV \\
\mathcal{I}_{xy} &= -\int_\Omega \rho(p) \block{xy}.dV
\end{align}
$$

What we seek to compute is:

$$
\begin{align}
\mathcal{I'}_{xx} &= \int_{S\Omega} \rho\block{S^{-1}p} \block{y^2 + z^2}.dV \\
&= \int_{\Omega} \rho\block{p} \block{s_y^2 y^2 + s_z^2 z^2}.dV.\det(S)\\
\mathcal{I'}_{xy} &= -\int_{S\Omega} \rho\block{S^{-1}p} \block{xy}.dV \\
&= -\int_\Omega \rho(p) \block{s_x x s_y y}.dV.\det(S)
\end{align}
$$

Scaling off-diagonal terms is easy: 

$$\mathcal{I'}_{xy} = \det(S) s_x s_y \mathcal{I}_{xy}$$

Scaling inertia diagonal requires more work: first, one need to obtain
scatter around axes from the unscaled tensor diagonal:

$$w = \mat{0 & 1 & 1\\1 & 0 & 1 \\ 1 & 1 & 0}^{-1} \diag\block{\mathcal{I}}$$

So that: $$w_x = \int_\Omega \rho(p) x^2.dV$$ and so on for $$w_y,
w_z$$. Then, the scaled diagonal is computed based on scaled scattering:

$$\diag\block{I'} = \det(S)\mat{0 & 1 & 1\\1 & 0 & 1 \\ 1 & 1 & 0} S^2 w $$


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

# Trackball

Let us consider a picked point $$x$$ in world coordinates. Assuming
the camera has projection matrix $$F$$, the projected point
coordinates $$y$$ is:

$$y = \pi \block{F x}$$

where $$\pi\mat{x\\y\\z} = \mat{\frac{x}{z}\\\frac{y}{z}}$$ computes
the perspective projection of a point onto the image plane. Now, we'll
assume our point is in fact attached to frame a rotating around some
pivot point $$p$$, with rotation $$\exp(\omega)$$. The coordinates of
our point become:

$$x(\omega) = \underbrace{\mat{I & p \\ 0 & 1}}_{T_p} \underbrace{\mat{\exp(\omega) & 0 \\ 0 & 1}}_{\exp(\kappa)} \underbrace{\mat{I & -p \\ 0 & 1}}_{T_p^{-1}} x_0$$

where $$x_0$$ are the (fixed) homogeneous coordinates of our point in
the rotating frame. Our projection equation becomes:

$$y = \pi \block{F.T_p.\exp(\kappa).T_p^{-1}.x_0 }$$

Differentating gives:

$$\dd y = \dd \pi(F.x(\omega)).F.\dd x(\omega).\dd \omega$$

$$\dd x(\omega).\dd \omega = T_p.\exp(\kappa).T_p^{-1}.\block{\Ad_{T_p}.\mat{\db \exp(\omega).\dd \omega & 0 \\ 0 & 0}}x_0$$

using canonical coordinates for the Lie algebra, we get:

$$\Ad_{T_p}.\mat{\db \exp(\omega).\dd \omega \\ 0} = \mat{\db \exp(\omega) \\ \hat{p}. \db \exp(\omega).\dd \omega}$$

and finally:

$$\begin{align}
\dd x(\omega).\db \omega &= T_p.\exp(\kappa).T_p^{-1} \mat{\db \exp(\omega).\dd \omega & \hat{p}. \db \exp(\omega).\dd \omega \\ 0 & 0} \mat{x_0 \\ 1} \\
&= T_p.\exp(\kappa).T_p^{-1} \mat{\hat{p - x_0}.\db \exp(\omega).\dd \omega\\ 0} \\
&= \mat{\exp(\omega).\hat{p - x_0}.\db \exp(\omega).\dd \omega\\ 0}
\end{align}
$$

On the other hand, differentiating $$\pi$$ gives:

$$\dd \pi(x, y, z) = \mat{\frac{1}{z} & 0 & -\frac{x}{z^2} \\
                                 0    & \frac{1}{z} & -\frac{y}{z^2}} \mat{\dd x\\ \dd y \\ \dd z}$$

And we obtain:

$$\dd y(\omega).\dd \omega = \dd \pi(x, y, z).F.\exp(\omega).\hat{p - x_0}.\db \exp(\omega).\dd \omega$$

We may now pull 2D image space forces to rotation torques and update $$\omega$$ accordingly.

# Blending

See [dual quaternions](dual-quaternions).

