---
title: Discrete Differential Geometry
categories: [math]
tags: [draft]
---

{% include toc.md %}

Discrete Differential Geometry (DDG) provides a set of tools for integrating
stuff over discrete manifolds.

# Discrete Manifolds

- simplicial complexes
- 2D manifolds: half-edge data structures (HDS)
- in general: combinatorial maps

# Differential Forms & Integration

Differential forms are the stuff that show up under the integral sign:

$$\int_\Omega \omega$$

In the above, $$\omega$$ is a differential form whose dimension matches that of
the integration domain $$\Omega$$. Its purpose is to "eat" infinitesimal volume
elements that make up $$\Omega$$ in order to produce a scalar, one per volume
element. Conceptually, these (infinitely many) scalars are then summed up to
compute the integral. The differential form thus encodes the *weighting*
associated to each volume element.

Infinitesimal volume elements around a point $$x \in \Omega$$ may be represented
by $$n$$ vectors describing the edges of the element. For integration to make
sense, we would like the action of weighting this volume element to be
$$n$$-linear: if we scale the size of any edge of our volume element, we expect
its volume to scale similarly. Additionally, if two edge vectors are the same,
the volume element degenerates and we expect its volume to be zero.

Therefore, a differential form can be seen as a function that associates some
alternating $$n$$-linear map to every point of the domain $$x$$:

$$
\begin{aligned}
\omega(x)&: \RR^n \to \RR \\
\omega(x)\block{\ldots, \lambda y + \mu z, \ldots} &= \lambda \omega(x)\block{\ldots, y, \ldots} + \mu \omega(x)\block{\ldots, z, \ldots} \\
\omega(x)\block{\ldots, x, \ldots, x, \ldots} &= 0 \\
\end{aligned}
$$

# Exterior Derivative & Stoke's Theorem

# Discrete Differential Forms & Exterior Derivative

# Metrics on discrete forms

## 0-forms

We want the metric on discrete 0-forms (sampled functions at vertices) to mimic
the $$L^2$$ inner product:

$$\inner{\hat{u},\hat{v}} \approx \int_\Omega u(x) v(v) \dd x$$

A first consequence is that the squared norm of the indicator function should
match the volume/area of the domain:

$$\inner{\hat{1},\hat{1}} = |\Omega|$$

If we require the metric to be diagonal, there's not much we can do apart from
partitioning the domain into subdomains $$\Omega_i$$ (which could be barycentric
or Voronoi-based), one for each vertex $$i$$. We end up with the following
metric:

$$M_0 = \diag\block{\left|\Omega_i\right|}$$ 

where 

$$\sum_i \left|\Omega_i\right| = \left|\Omega\right|$$
 

### TODO whitney basis/galerkin mass

## 2-forms

A discrete 2-form is the integral of some actual 2-form over a 2-cell
(triangle). Let $$\hat{\omega}_i = e_i$$ be a piecewise constant 2-form whose
integral over the i-th triangle $$T_i$$ is $$1$$ and $$0$$ over other
triangles. That is, $$\omega_i$$ is a 2-form whose discrete version is the
$$i$$-th basis vector $$e_i$$. Since $$\omega_i$$ is top-dimensional, it can be
written as $$\omega_i = f_i {1}_{T_i} \dd A$$ where $$\dd A$$ is the area form,
and $$f_i$$ is such that $$\int_{T_i} f_i \dd A = 1$$. Therefore:

$$f_i =\frac{1}{\left|T_i\right|}$$ 

Again, the metric on discrete 2-forms should mimic the $$L^2$$ inner product on
2-forms, so we get the following diagonal elements:

$$e_i^T M_2 e_i = \int_{T_i} f_i^2 \dd A = \frac{1}{\left|T_i\right|}$$

If we further require the metric to be diagonal, we're done:

$$M_2 = \diag\block{\frac{1}{\left|T_i\right|}}$$

## 1-forms





