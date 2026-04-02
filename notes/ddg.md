---
title: Discrete Differential Geometry (Draft)
categories: [math, draft]
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


