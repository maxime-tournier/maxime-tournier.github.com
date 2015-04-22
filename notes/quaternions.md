---
title: Quaternions
categories: [math, lie group]

---

Quaternions are primarily used as a representation of space rotations.
They are convenient to use due to their compact form and their nice
geometric properties. Most operations on rotations can be expressed by
means of quaternion representation, which generally offers a somewhat
more intuitive point of view.

* TOC
{:toc}

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
\norm{q} - 1$$. It is also compact and simply connex, meaning that
every smooth closed path can be deformed to a point.


## Lie Group

The multiplication and inverse are smooth (and $$1 \in S^3$$) so
$$S^3$$ has a Lie group structure, which provides the connection with
rotations in $$\RR^3$$.

### Adjoint Representation

The tangent space at $$1$$ is the space of imaginary quaternions,
which we identify with $$\RR^4$$. The inner automorphism $$\Psi_q$$:

$$ \Psi_q: h \mapsto q h \bar{q} $$

is sometimes known as the *conjugation by $$q$$*, and its derivative
at $$1$$ is the *adjoint* of the Lie group:

$$ Ad_g = \dd \Psi_g(1): \RR^3 \to \RR^3 $$

From the multiplicative property of the norm, we see that $$Ad_g$$ is
an isometry of $$\RR^3$$, so it is either a rotation or a reflection
(or a mix of the two). Since $$S^3$$ is connex and $$1 \in S^3$$ (and
$$Ad_1 = I_3$$ which is a rotation) $$Ad_g$$ has to be a rotation:
there is a smooth path from $$1$$ to any quaternion so that
$$\det\block{Ad_g}$$ has to remain $$1$$.

$$Ad$$ provides a group homomorphism between $$S^3$$ and $$SO(3)$$
(the *adjoint representation*):

$$ Ad: S^3 \to SO(3) $$

so that multiplying quaternions corresponds to composing
rotations. However, it is not injective (the representation is not
*faithful*): both $$q$$ and $$-q$$ share the same adjoint. This
$$2$$-to-$$1$$ relationship is known as the *double covering* of
$$SO(3)$$ by $$S^3$$.


### Lie Algebra

The derivative of the adjoint map at the identity provides the Lie algebra
structure on $$\RR^3$$, noted $$\alg{s^3}$$:

$$ ad = \dd Ad_1: \RR^3 \mapsto \alg{so(3)} $$

where the Lie bracked is given by:

$$ [x, y] = ad(x)y $$

The Lie bracked can also be expressed as the quaternion commutator:

$$ [x, y] = xy - yx $$

In coordinates, the Lie bracked is twice the cross product:

$$ [x, y] = 2 x \times y $$

### Exponential Map

The classical characterization of the exponential (due to Euler)
conveys pretty well what the exponential does:

$$ \exp(v) = \lim_{n \to \infty} \block{ 1 + \frac{v}{n} }^n $$

That is: we cut our vector $$v$$ in infinitesimally small pieces to
obtain elements of the group infinitesimally close to $$1$$ in the
direction $$v$$, and multiply (compose) these elements together. As in
the matrix case, one can verify that the exponential is also given by
the usual Taylor series, this time using quaternion product:

$$ \exp(v) = \sum_{i=0}^{\infty} \frac{v^i}{i!} $$

When $$v \neq 0$$, there exist a unit vector $$n\in S^2$$ such that
$$v = \norm{v} n$$. This unit vector satisfies $$n^2 = -1$$, so the
above sum can be rewritten as:

$$
\begin{align}
\exp(v) &= \sum_i \frac{v^{2i}}{2i!} + \frac{v^{2i+1}}{2i+1!} \\
&= \sum_i (-1)^i \frac{\norm{v}^{2i}}{2i!} + (-1)^{i} \frac{\norm{v}^{2i+1}}{2i+1!} n \\
&= \cos \norm{v} + \sin \norm{v} n
\end{align}
$$

The injectivity radius is $$2\pi$$, but we start getting the same
rotations after $$\pi$$. The logarithm is the inverse operation,
defined inside the injectivity radius:

$$ \log q = \theta n $$

where:

$$
\begin{align}
    \theta &= \inv{\cos}(w) \\ 
    n &= \frac{v}{\norm{v}} \\
\end{align}
$$

Since both $$q$$ and $$-q$$ represent the same rotation, it is common
to *flip* the quaternion to the positive real hemisphere prior to
computing the logarithm:

$$ f(q) =
\begin{cases}
q & \text{if} &  w_q \geq 0\\
-q & \text{if} & w_q < 0\\
\end{cases}
$$

Doing so ensures the arc-cosine is well defined, and that the rotation
interpolated along the logarithm by $$\exp\block{\alpha \log(q)},\
\alpha \in [0, 1]$$ takes the *short way*.

## Rotation Correspondence

At this point, we still do not know which rotation is represented by a
given unit quaternion. From the Taylor series, we see that $$\exp(v)$$
and $$v$$ commute, so that:

$$Ad_{\exp(v)} = \exp(v)v\exp(-v) = \exp(v)\exp(-v)v = v$$

and the rotation axis of $$Ad_{\exp(v)}$$ is $$v$$. From the adjoint
representation, the additive structure along vector $$v$$ corresponds
to the addition of angles, since we compose rotations along the same
axis. Hence the rotation angle is propotional to
$$\norm{v}$$. Finally, $$\exp(\pi n) = -1$$ for a unit vector $$n$$,
which corresponds to the identity rotation (or $$2\pi$$), so the
rotation angle for $$\exp(v)$$ is $$2\norm{v}$$. Equivalently:

$$ R_{n, \theta} \simeq \cos \frac{\theta}{2} + \sin \frac{\theta}{2} n $$

Now the double covering makes more sense.

## Riemannian Manifold

The ambient metric in $$\RR^4$$ induces a Riemannian manifold
structure on $$S^3$$, which plays nicely with the Lie group structure
as we shall see.

### Bi-invariant Metric

The ambient metric on $$\RR^4$$ induces a metric each fiber (tangent
space) of the tangent bundle $$TS^3$$. This metric is left-invariant:

$$ \inner{u}{v}_{\RR^4} = \inner{qu}{qv} $$

It is also right invariant (and thus also $$Ad$$-invariant):

$$ \inner{u}{v}_{\RR^4} = \inner{uq}{vq} $$

To see both of these, it is useful to come back to the matrix
representation of quaternions, in which the metric is:

$$ \inner{u}{v} = \trace{u^Tv} $$

So the Lie algebra metric, extended to the whole manifold by left or
right translation, is a *Riemannian metric* (the one induced by the
ambient space). An interesting consequence is that the Lie group
exponential can be used to compute geodesics for this metric.

### Geodesics

Geodesics are locally shortest path between points in the
manifold. The fact that the Riemannian metric is bi-invariant implies
that curves of the form:

$$ q \exp(t v),\quad t\in\RR $$

are geodesic curves (and all geodesics are of this form). We can
obtain geodesic curves joining two unit quaternions $$a, b \in S^3$$
as follows:

$$ f(t) = a\exp\block{t \log\block{ \inv{a} b } } $$

This is generally referred to as the *spherical linear interpolation*,
or SLERP, and is used to interpolate the corresponding rotations.

# Misc.

## Conversion to Rotation Matrix 

## Interpolation

## Averaging

## Geodesic Projection

## Exponential Map Derivative

## Conversion to/from Euler Angles

