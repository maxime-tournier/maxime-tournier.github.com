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

# Differential Forms

Differential forms are the stuff that show up under the integral sign:

$$\int_\Omega \omega$$

In the above, $$\omega$$ is a differential form whose dimension matches that of
the integration domain $$\Omega$$. Its purpose is to "eat" infinitesimal volumes
that make up $$\Omega$$ in order to produce a scalar, one per infinitesimal
volume. Conceptually, these (infinitely many) scalars are then summed up to
compute the integral. The differential form thus encodes the *weighting*
associated to each infinitesimal volume.

Infinitesimal volumes around a point $$x \in \Omega$$ may be represented by
$$n$$ (tangent) vectors describing the edges of a parallelotope. For integration
to make sense, we would like the action of weighting this parallelotope to be
$$n$$-linear: if we scale the size of any edge of the parallelotope, we expect
its volume to scale similarly. Additionally, if two edge vectors are the same,
we expect the volume to be zero. Taken together, there properties imply that the
weighting of an infinitesimal volume should act like an *alternating* $$n$$-linear
map.

Therefore, a differential form can be seen as a function that associates a
alternating $$n$$-linear map $$\omega(x): \block{T_x\Omega}^n \to \RR$$ (in the tangent
space at this point) to every point of the domain $$x$$:

$$
\begin{aligned}
\omega(x)\block{\ldots, \lambda y + \mu z, \ldots} &= \lambda \omega(x)\block{\ldots, y, \ldots} + \mu \omega(x)\block{\ldots, z, \ldots} \\
\omega(x)\block{\ldots, x, \ldots, x, \ldots} &= 0 \\
\end{aligned}
$$

For instance, the usual differential of a scalar function is a 1-form: it
associates to every point of the domain a linear form (the differential at that
point, which is trivially alternating) and can be integrated along curves. By
convention, 0-forms are scalar functions of the domain. An interesting property
of $$n$$-linear alternating maps is that their pullback by an endomorphism $$A$$
satisfies:

$$A^*\omega\block{x_1, \ldots, x_n} = \omega\block{Ax_1, \ldots, Ax_n} =
\det(A)\omega\block{x_1, \ldots, x_n}$$

In fact, this property can even serve as a *definition* of the determinant. This
implies that $$n$$-forms are rotation-invariant as one would expect: rotating
infinitesimal volumes should not change their weight.

Finally, since $$n$$-linear (alternating) maps form a 1-dimensional vector
space, we may choose some non-degenerate differential $$n$$-form $$\mu$$ as a
reference and obtain any other $$n$$-form as $$\omega = f \mu$$ where $$f$$ is a
scalar function that provides the pointwise scaling factor. Such reference
$$n$$-forms are usually called *volume forms*.

## TODO wedge products



## Volume forms

*Orientable* manifolds are exactly defined as the ones for which such a volume
form exists. When the manifold comes with a Riemannian metric, there is a
natural choice of a volume form: it is chosen such that any
orientation-preserving orthonormal parallelotope (as per the metric) is weighted
to $$1$$[^orthogonal-invariance]. Now, any orthogonal basis $$B$$ satisfies:

$$B^T M B = I$$

where $$M$$ is the inner product matrix (pointwise). Since the metric is
non-degenerate, a [Cholesky decomposition](cholesky) $$M=LL^T$$ can be used to
show that an orientation-preserving basis must decompose as $$B=L^{-T}Q$$ where
$$Q \in SO(n)$$ is a rotation. If we let $$\dd x^1, \ldots, \dd x^n$$ be local
coordinates, the associated $$n$$-form $$\dd x^1\wedge \ldots \wedge \dd x^n$$
satisfies

$$\begin{aligned}
B^*\block{\dd x^1\wedge \ldots \wedge \dd x^n} &= \det(B)\dd x^1\wedge \ldots \wedge \dd x^n \\
&= \sqrt{\det(M)}\dd x^1\wedge \ldots \wedge \dd x^n
\end{aligned}
$$

Trivially, $$B^*\block{\dd x^1\wedge \ldots \wedge \dd x^n} = \dd Bx^1 \wedge
\ldots \wedge \dd Bx^n$$ will weight the parallelotope associated to basis $$B$$
to $$1$$ and therefore, the Riemannian volume form in local coordinates is given by:

$$\sqrt{\det(g)}\dd x^1\wedge \ldots \wedge \dd x^n$$


## Metric

On Riemannian manifolds, differential 1-forms can be identified to vector fields
using the metric through the so-called *musical isomorphisms*:

$$X^\flat = g(X, .)$$

where $$g$$ is the Riemannian metric. Here $$X^\flat$$ is a 1-form obtained from
vector field $$X$$ by considering the (pointwise) inner-product with $$X$$ using
the metric $$g$$. Conversely, a 1-form $$\omega$$ and be (pointwise) represented
by the inner product with some tangent vector $$\omega^\sharp$$, producing a
vector field. This in turns provides an inner-product on differential 1-forms,
obtained by integrating the inner product of their representing vector fields
over the manifold.


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

Again, a discrete 1-form is the integral of some actual 1-form over a 1-cell
(edge). 

Before even thinking of transporting the $$L^2$$ inner product from
1-forms to discrete forms, we need some correspondance between 1-form on the
mesh and basis discrete 1-forms, which is not entirely trivial. 

### Whitney forms

More precisely, we want to construct some 1-forms $$\omega_{ij}$$ such that
$$\hat{\omega}_{ij} = e_{ij}$$, that is:

$$\begin{aligned}
\int_{ij} \omega_{ij} &= 1 \\
\int_{e \neq ij} \omega_{ij} &= 0\\
\end{aligned}
$$

If we consider Whitney basis functions $$\phi_i, \phi_j$$, we see that by
Stokes' theorem:

$$\begin{aligned}
\int_{e} \dd\block{\phi_i \phi_j} &= \int_{\partial e} \phi_i \phi_j = 0 \\
&= \int_{e} \phi_j \dd \phi_i + \phi_i \dd \phi_j \\
\end{aligned}
$$

for any edge $$e$$. Also, on edge $$ij$$ we have $$\phi_i + \phi_j = 1$$,
therefore $$\dd \phi_i = -\dd \phi_j$$, so instead of

$$\int_{ij} \phi_j \dd \phi_i + \phi_i \dd \phi_j = \int_{ij} \underbrace{\block{\phi_j - \phi_i}}_0 \dd \phi_i = 0$$ 

cancelling out as above, we could instead do:

$$\int_{ij} \phi_j \dd \phi_i - \phi_i \dd \phi_j = \int_{ij} \underbrace{\block{\phi_j + \phi_i}}_1 \dd \phi_i = \int_{\partial ij} \phi_i = 1 - 0 = 1$$ 

So the 1-form $$\phi_{ij} = \phi_j \dd \phi_i - \phi_i \dd \phi_j$$ integrates
to 1 over edge $$ij$$. For some other edge that is not $$ij$$, for instance
$$ik$$, we get:

$$\int_{ik} \underbrace{\phi_j}_0 \dd \phi_i - \phi_i \dd \phi_j = \int_{ik}
  \underbrace{\phi_j}_0 \dd \phi_i = 0$$

by our first identity. The same trick works for any edge that is not $$ij$$, so
that the $$\phi_{ij}$$ are a basis for discrete forms:

$$\hat{\phi}_{ij} = e_{ij}$$

Unlike 0 and 2-forms, there's no immediate way to link the $$L^2$$ inner product
to the usual integration of functions: the inner product is obtained from the
Riemmanian metric as:

$$\inner{\omega_1, \omega_2} = \int_\Omega \inner{\omega_1^\sharp(x), \omega_2^\sharp(x)}.\dd A$$

where the Riemannian metric is used to define the $$\sharp, \flat$$ operators
between the tangent and cotangent bundles. We may now proceed to compute the
$$L^2$$ inner product of Whitney forms $$\phi_{ij}$$, and see how to transfer it
to discrete forms with a metric. 

### Discrete Metric

$$\inner{\phi_{ij}, \phi_{kl}}_{L^2} = \int_\Omega \inner{\phi_{ij}(x), \phi_{kl}(x)}.\dd A$$

Split on edge cells $$\sigma(e)$$:

$$\inner{\phi_{ij}, \phi_{kl}}_{L^2} = \sum_e \int_{\sigma(e)} \inner{\phi_{ij}(x), \phi_{kl}(x)}.\dd A$$

Use 1-point quadrature at the center of each edge $$c(e)$$:

$$\int_{\sigma(e)} \inner{\phi_{ij}(x), \phi_{kl}(x)}.\dd A \approx
\vol{\sigma(e)} \inner{\phi_{ij}\block{c(e)}, \phi_{kl}\block{c(x)}}$$

Now assume $$e = ij$$. 

### Discrete Metric

As before, let us consider the $$L^2$$ norm for $$\phi_{ij}$$, which will
correspond the the metric diagonal terms:

$$\norm{\phi_{ij}}_{L^2}^2 = \int_\Omega \norm{\phi_{ij}(x)}^2.\dd A$$

Over triangle $$ijk$$ the basis functions $$\phi, \phi_j$$ are equal to the
barycentric coordinates $$\lambda_i, \lambda_j$$ over this triangle:

$$\int_{ijk} \norm{\phi_{ij}(x)}^2 = \int_{ijk} \norm{\lambda_i \nabla \lambda_j -
\lambda_j \nabla \lambda_i}^2$$

whose gradients $$\nabla \lambda_i, \nabla \lambda_j$$ are constant over
$$ijk$$. Expanding the squared norm gives:

$$\int_{ijk} \norm{\phi_{ij}(x)}^2 = \norm{\nabla \lambda_i}^2 \int_{ijk}\lambda_i^2 +  \norm{\nabla \lambda_j}^2 \int_{ijk}\lambda_j^2 - 2 \inner{\nabla \lambda_i, \nabla \lambda_j} \int_{ijk}\lambda_i \lambda_j$$

It can be show that:

$$
\begin{aligned}\int_{ijk} \lambda_i^2 = \int_{ijk} \lambda_j^2 &= \frac{|ijk|}{6}\\
\int_{ijk} \lambda_i\lambda_j &= \frac{|ijk|}{12} \\
\end{aligned}
$$

So that we end up with:

$$\int_{ijk} \norm{\phi_{ij}(x)}^2 = \frac{|ijk|}{6}\block{\norm{\nabla \lambda_i}^2 + \norm{\nabla \lambda_j}^2 - \inner{\nabla \lambda_i, \nabla \lambda_j}}$$


Using the Riemannian metric induced by the canonical metric on $$\RR^3$$, we can
obtain[^1] the following expression:

$$\nabla \lambda_i = \frac{|jk|}{2|ijk|} n_i$$

where $$n_i$$ is the unit vector orthogonal to $$jk$$ in triangle $$ijk$$. We
end up with the following:

$$\begin{aligned}
\norm{\nabla \lambda_i}^2 &= \frac{|jk|^2}{4 |ijk|^2} \\
\norm{\nabla \lambda_j}^2 &= \frac{|ki|^2}{4 |ijk|^2} \\
\inner{\nabla \lambda_i, \nabla \lambda_j} &= \frac{1}{4 |ijk|^2} \inner{jk, ki} \\
\end{aligned}$$

By the polarization identity:

$$
\begin{aligned}
\inner{jk, ki} &= -\inner{jk, ik} \\
&= -\frac{1}{2}\block{\norm{jk}^2 + \norm{ik}^2 - \norm{jk - ik}^2} \\
&= -\frac{1}{2}\block{\norm{jk}^2 + \norm{ik}^2 - \norm{ji}^2} \\
\end{aligned}
$$

Therefore:

$$\inner{\nabla \lambda_i, \nabla \lambda_j} = -\frac{1}{4 |ijk|^2} \frac{\norm{jk}^2 + \norm{ki}^2 - \norm{ji}^2}{2}$$


<!-- $$\inner{\nabla \lambda_i, \nabla \lambda_j} = \frac{|jk| |ik| \cos\block{\theta_k}}{4|ijk|^2}$$ -->

<!-- which can be shown to be equal to the so-called *cotan weights*: -->

<!-- $$\inner{\nabla \lambda_i, \nabla \lambda_j} = -\frac{\cot\block{\theta_k}}{4|ijk|^2}$$ -->

<!-- Similarly, it can be shown that: -->

<!-- $$\norm{\nabla \lambda_i}^2 = \frac{\cot\block{\theta_j} + \cot\block{\theta_k}}{4|ijk|^2}$$ -->

<!-- which brings us to: -->

<!-- $$\int_{ijk} \norm{\phi_{ij}(x)}^2 = \frac{1}{24|ijk|}\block{\cot\block{\theta_i} + \cot\block{\theta_j} + 3\cot\block{\theta_k}}$$ -->



# Notes & References

[^1]: It is easy to see that $$\lambda_i$$ does not change in direction $$jk$$
    (hence should be along $$n_i$$), and that moving along $$n_i$$ by the
    altititude $$h_i = \frac{2|ijk|}{|jk|}$$ of vertex $$i$$ to edge $$jk$$
    causes $$\lambda_i$$ to change linearly by exactly 1.

[^2]: From the formula $$\cot\block{\theta_i} = \frac{\modulus{ki}^2 +
    \modulus{ij}^2 - \modulus{jk}^2}{4\modulus{ijk}}$$

[^orthogonal-invariance]: we just saw that $$n$$-forms are orthogonally
    invariant, so this definition is consistent: any orthogonal basis will be
    weighted to 1
