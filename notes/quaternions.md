---
title: Quaternions
---

Quaternions are primarily used as a representation of space
rotations. As such, they turn out to be quite convenient to use in
practice due to their compactness and nice geometric properties. Most
operations on rotations can be expressed by means of quaternion
representation, which generally offers a somewhat more intuitive point
of view.



# Construction and Properties

In this section, we give a simple construction of the quaternions and
list their basic properties (product, inverse, norm, etc...).

## Complex Numbers
   
We start by building the complex numbers as the real $$\small{2 \times 2}$$
matrices of the form:

   $$ \mat{ 
   x  & y  \\
   -y & x  \\
   } $$
   
The above set is a $$2$$-dimensional vector space, whose basis is
noted $$(1, i)$$. One can check that $$i^2 = -1$$.
   
This space is closed under matrix multiplication, and one can show
that this space, together with this (commutative!) multiplication, form
a *field*, the complex numbers, noted $$\CC$$

## Quaternions

We repeat this construction, this time with *complex* matrices of the
form:

$$ \mat{ x & \bar{y} \\ -y & \bar{x} } $$

We end up with a 2-dimensional *complex* vector-space, that is also
a 4-dimensional *real* vector space, given by real $$4 \times
4$$ matrices of the form:

$$ \mat{ 
	w &  x &  y &  z \\
	-x &  w & -z &  y \\
	-y &  z &  w & -x \\
	-z & -y &  x &  w \\ 
   }
   $$

*(notice the $$2\times 2$$ complex blocks)*

This space is again closed under matrix multiplication, but the
multiplication is no longer commutative. Together with the matrix
product, this space forms the quaternions, noted $$\HH$$.

We will generally use the *real* vector space structure, and identify
a quaternion with its coordinates $$(w, x, y, z)$$ in the canonical
basis.

## Canonical Basis

The 4 basis vectors for the *real* vector space structure are commonly
noted $$(1, i, j, k)$$ and verify:

$$ i^2 = j^2 = k^2 = ijk = -1 $$


These formula are actually sufficient to define the quaternions, and
were carved on *Brougham (Broom) Bridge* in Dublin by **Hamilton**, on
the 16th of October 1843 (yay wikipedia).
  
In the remaining of this document, we will denote quaternions using
the following notations:
  
$$ q = (w, x, y, z) =: w + v $$
  
where $$v$$ is a pure imaginary quaternion with coordinates $$(0, x,
y, z)$$, which we will happily identify with the corresponding vector
in $$\RR^3$$ when needed. $$w$$ is called the *real* part, and $$v$$
the *imaginary* part, just like for complex numbers.

## Product

The quaternion product follows from the matrix representation
product. In coordinates, it has the following expression:

$$
ab = (w_a + n_a) (w_b + n_b) = w_a w_b - n_a^T n_b \quad + \quad w_a n_b + n_a w_b + n_a\times n_b
$$

In particular, for pure imaginary quaternions $$x$$ and $$y$$ this gives:

$$ x y = x \times y - x^Ty $$

## Conjugate

As for the complex numbers, this corresponds to the transpose of the
matrix representation. This immediately implies that:
  
$$ \bar{p q} = \bar{q} \ \bar{p} $$

In coordinates, the conjugation is simply given by:

$$ \bar{q} = w_q - v_q $$
  
The real and imaginary parts of a quaternion may be expressed using
conjugation:

$$ w_q = \frac{q + \bar{q}}{2} $$

$$ v_q = \frac{q - \bar{q}}{2} $$

## Norm

As for the complex numbers, the conjugation induces a norm over the
quaternions (the Frobenius norm on the matrix representation), which
coincides with the Euclidean norm on $$\RR^4$$:
  
$$ |q|^2 = q \bar{q} = \bar{q} q = ||q||_{\RR^4}^2 \geq 0 $$

As for the complex numbers, the norm is multiplicative:

$$ |ab| = |a|\ |b| $$

## Inverse

From the norm definition, a simple expression can be obtained for
the quaternion inverse:

$$ \inv{q} = \frac{\bar{q}}{|q|^2} $$

# Unit Quaternions: $$S^3$$

The unit sphere $$S^3$$ is stable under quaternion multiplication and
inverse, (and contains $$1$$) which makes it a multiplicative subgroup
of $$\HH$$. As we shall see, this space is particularly interesting
for representing rotations. The unit sphere, together with quaternion
multiplication, is isomorphic to the complex represention with unit
complex coefficients, *i.e.* the group $$SU(2)$$.

## Smooth Manifold 

$$S^3$$ is a closed, $$3$$-dimensional smooth submanifold of $$\RR^4$$
as the inverse image of $$0$$ by the smooth function $$f(q) =
\norm{q} - 1$$.

It is also compact and simply connex, meaning that every smooth closed
path can be deformed to a point. The multiplication and inverse are
smooth, so $$S^3$$ has a Lie group structure. Finally, the ambient
metric in $$\RR^4$$ induces a Riemannian structure, which plays nicely
with the Lie group structure as we shall see.

## Lie Group

The tangent space at $$1$$ is the space of imaginary quaternions,
which we identify with $$\RR^4$$. The inner automorphism $$\Psi_q$:

$$ \Psi_q: h \mapsto q h \bar{q} $$

is sometimes known as the *conjugation by $$q$$*, and its derivative
at $$1$$ is the *adjoint* of the group:

$$ Ad_g = \dd \Psi_g(1): \RR^3 \to \RR^3 $$

From the multiplicative property of the norm, we see that $$Ad_g$$ is
an isometry of $$\RR^3$$, so it is either a rotation or a reflection
(or a mix of the two). Since $$S^3$$ is connex and $$1 \in S^3$$ (and
$$Ad_1 = I_3$$) $$Ad_g$$ has to be a rotation.

## Riemannian Manifold


 


