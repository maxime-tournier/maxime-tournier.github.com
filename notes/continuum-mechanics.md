---
title: Continuum Mechanics
categories: [phys]
---

# Deformation Gradient

Let us consider a geometric object represented as a domain $$\Omega \subset
\RR^3$$. As the object evolves over time, we describe the trajectory of its
points as a function $$p(x, t)$$ of the material coordinates and time, starting
at $$p(x, 0) = x$$.

If the deformation is smooth (meaning we don't tear or pinch the object), one
can obtain at any given time $$t \geq 0$$ some information about the deformation
around each point $$x \in \Omega$$ by studying the *deformation gradient*
tensor, generally denoted by $$F$$:

$$F(x, t) = \ddd{p}{x}(x, t)$$

For example, in the initial configuration the deformation gradient is everywhere
the identity:

$$F(x, t) = I$$

which tells us that that the initial configuration is everywhere
undeformed. Likewise, an object not moving $$p(x, t) = x$$ has a deformation
gradient everywhere the identity. If we now apply a linear transformation to the
whole object as a function of time, as:

$$p(x, t) = A(t) x$$

(for instance, by scaling it $$A(t) = tI$$) we obtain the following deformation
gradient:

$$F(x, t) = A(t)$$

On the contrary, if the deformation depends on $$x$$, the deformation gradient
tells us how the deformation looks like locally around $$x$$ as a linear map.

## Displacement Field

The displacement field $$u(x, t)$$ is simply defined as the difference between
current and initial positions for a given object point $$x$$:

$$
\begin{align}
u(x, t) &= p(x, t) - p(x, 0) \\
  &= p(x, t) - x\\
\end{align}
$$

It is easy to see that the deformation gradient can be obtained from the
displacement gradient as:

$$\ddd{u}{x}(x, t) = F - I$$

so that the deformation gradient is obtained as:

$$F(x, t) = \ddd{u}{x}(x, t) + I$$

## Shape Functions

For numerical simulations, the displacement/deformation field is generally
*discretized* using a finite number of primitive degrees of freefom (DOFs)
$$q_j$$ and the deformation at an arbitrary material coordinate $$x$$ is
obtained by interpolating the deformation at the $$q_j$$. For instance, $$q_j$$
can simply be composed of selected points in the solid, and a material point
$$p(x)$$ is given *weights* $$w_j(x)$$ with respect to each of the material
points, so that the deformed point is given by:

$$p(x) = \sum_j w_j(x) q_j$$

Generally, $$q_j$$ are the nodes in a tetrahedral/hexahedral grid and $$w(x)$$
are the associated barycentric coordinates. More advanced interpolation schemes
are possible, for instance using rigid frames for $$q_j$$ and [dual quaternion
skinning](dual-quaternions) for interpolating the displacement field.

In the case of a linear combination as above, the deformation gradient at
material coordinates $$x$$ can be simply obtained by:

$$F(x, t) = \ddd{p}{x}(x, t) = \sum_j \ddd{w_j}{x}(x) q_j(t)$$

The function $$w: \RR^3 \to \RR$$ is called a *shape function*. 

# Strain Tensor

The deformation tensor gives a nice local representation of an arbitrary (but
smooth) object deformation. Now, if we want to simulate elastic materials, we
will need to associate a potential energy to a deformed object. Since we have a
local measure of deformation $$F$$, we can obtain a deformation *energy* by
summing infinitesimal energies associated with the deformation gradient $$F(x,
t)$$ at each point of the object:

$$V(t) = \int_\Omega W(F(x, t)).\dd x$$

$$W$$ is called a potential energy density, and associates an infinitesimal
energy to a deformation tensor $$F$$. Now, for our energy to be well-behaved we
will need $$W$$ to satisfy the following:

- $$E$$ is bounded by below, and reaches its minimum at the undeformed state:
  $$W(I) = 0$$
- rotational-invariance: $$W(UF) = W(F)$$ for any rotation matrix $$U\in SO(3)$$

Rather than defining the energy density directly on $$F$$, one generally 
computes a so-called *deformation tensor* meeting the above requirements. Then,
a *stain tensor* is obtained which quantifies how much the deformation tensor
deviates from the identity. Finally, an energy density is associated to the
strain tensor.

## Principal Stretches

Use the polar decomposition of $$F=QR$$, where $$Q\in SO(3)$$ and $$R \in
\SS^+$$ is positive definite (assuming no orientation ever occurs), and use the
pure deformation part:

$$C = R$$

$$R$$ can be diagonalized as $$R=USU^T$$, and the eigenvalues are known as the
*principal stretches*. These eigenvalues depend solely on the coefficients of
the characteristic polynomial of $$R$$, which are known as its *principal
invariants*. Therefore, one can formulate rotationnally invariant energy
densities either as a function of the eigenvalues directly, or as a function of
the principal invariants. The latter generally have a more physical
interpretation.

## Cauchy-Green Deformation Tensor (Right) 

$$C = F^T F = R^T R$$ 

## Green-Lagrange Strain Tensor

$$E = \half(C - I)$$

## Cauchy Strain Tensor

For small displacements only (not rotationally invariant):

$$\epsilon = \half\block{F^T + F} - I$$

# Energy Densities

## Neo-Hookean Materials

$$W = k.\tr\block{F^T F - I}$$


# Energy Integration

TODO gauss points etc

## Quadrature Rules

Given a discretized deformation field with DOFs $$q_j$$, shape functions $$w_j$$
and an energy density function $$W$$, there may exist efficient ways of
integrating the deformation energy $$V$$. Such integration schemes are called
*quadrature rules* and generally involve the evaluation of the deformation
gradients at a finite number of points, called *integration points*.





