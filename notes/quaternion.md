---
title: Quaternions
categories: [math, lie group]
---

Quaternions are primarily used as a representation of space rotations.
They are convenient to use due to their compact form and their nice
geometric properties. Most operations on rotations can be expressed by
means of quaternion representation, which generally offers a somewhat
more intuitive point of view.

{% include toc.md %}

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
that this space, together with this (commutative) multiplication, form
a *field*, the complex numbers, noted $$\CC$$.

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

*(notice the $$2\times 2$$ complex blocks)* (TODO some signs are wrong :-/)

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
ab = (w_a + v_a) (w_b + v_b) = w_a w_b - v_a^T v_b \ \  + \ \ w_a v_b + v_a w_b + v_a\times v_b
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
multiplication, is isomorphic to the complex representation with unit
complex coefficients, *i.e.* the group $$SU(2)$$.

## Smooth Manifold 

$$S^3$$ is a closed, $$3$$-dimensional smooth sub-manifold of $$\RR^4$$
as the inverse image of $$0$$ by the smooth function $$f(q) =
\norm{q}^2 - 1$$. It is also compact and simply connex, meaning that
every smooth closed path can be deformed to a point.


## Lie Group

The multiplication and inverse are smooth (and $$1 \in S^3$$) so
$$S^3$$ has a Lie group structure, which provides the connection with
rotations in $$\RR^3$$ through the adjoint representation.

### Adjoint Representation

The tangent space at $$1$$ is the space of imaginary quaternions,
which we identify with $$\RR^3$$. The inner automorphism $$\Psi_q$$:

$$ \Psi_q: S^3 \to S^3 \quad h \mapsto q h \bar{q} $$

is sometimes known as the *conjugation by $$q$$*, and its derivative
at $$1$$ is the *adjoint* of the Lie group:

$$ \Ad_q = \dd \Psi_q(1): \RR^3 \to \RR^3 \quad x \mapsto qx\bar{q} $$

From the multiplicative property of the norm, we see that $$\Ad_g$$ is
an isometry of $$\RR^3$$, so it is either a rotation or a reflection
(or a mix of the two). Since $$S^3$$ is connex and $$1 \in S^3$$ (and
$$\Ad_1 = I_3$$ which is a rotation) $$\Ad_g$$ has to be a rotation:
there is a smooth path from $$1$$ to any quaternion so that
$$\det\block{\Ad_g}$$ has to remain $$1$$.

$$\Ad$$ provides a group homomorphism between $$S^3$$ and $$SO(3)$$
(the *adjoint representation*):

$$ \Ad: S^3 \to SO(3) $$

so that multiplying quaternions corresponds to composing
rotations. However, it is not injective (the representation is not
*faithful*): both $$q$$ and $$-q$$ share the same adjoint. This
$$2$$-to-$$1$$ relationship is known as the *double covering* of
$$SO(3)$$ by $$S^3$$.


### Lie Algebra

The derivative of the adjoint map at the identity provides the Lie algebra
structure on $$\RR^3$$, noted $$\alg{s^3}$$:

$$ \ad = \dd \Ad_1: \RR^3 \mapsto \alg{so(3)} $$

where the Lie bracked is given by:

$$ [x, y] = \ad(x)y $$

The actual derivation of $$\Ad$$ at the identity gives an expression
as the quaternion commutator:

$$ [x, y] = xy - yx $$

In coordinates, the Lie bracked is twice the cross product:

$$ [x, y] = 2 x \times y $$

While we're at it, $$\ad$$ also provides a Lie algebra isomorphism
between $$\alg{s^3}\ (\simeq \RR^3)$$ and $$\alg{so(3)}$$, the space of
$$3 \times 3$$ skew-symmetric matrices:

$$ \ad(x) = 2 \hat{x} $$

and the Lie bracket is preserved:

$$ \ad( [x, y] ) = [\ad(x), \ad(y)] $$


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
rotations again after a radius of $$\pi$$. The *logarithm* is the
inverse operation, defined inside the injectivity radius:

$$ \log(q) = \theta n $$

where:

$$
\begin{align}
    \theta &= \arccos(w) \\ 
    n &= \frac{v}{\norm{v}} \\
\end{align}
$$

Since both $$q$$ and $$-q$$ represent the same rotation, it is common
to *flip* the quaternion to the positive real hemisphere prior to
computing the logarithm:

$$ q^+ =
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

$$\Ad_{\exp(v)} = \exp(v)v\exp(-v) = \exp(v)\exp(-v)v = v$$

and the rotation axis of $$\Ad_{\exp(v)}$$ is $$v$$. From the adjoint
representation, the additive structure along vector $$v$$ corresponds
to the addition of angles, since we compose rotations along the same
axis. Hence the rotation angle is propotional to
$$\norm{v}$$. Finally, $$\exp(\pi n) = -1$$ for a unit vector $$n$$,
which corresponds to the identity rotation (or $$2\pi$$), so the
rotation angle for $$\exp(v)$$ is $$2\norm{v}$$. Equivalently:

$$ R_{n, \theta} \simeq \cos \frac{\theta}{2} + \sin \frac{\theta}{2} n $$

Now hopefully that double covering thing makes more sense.

## Riemannian Manifold

The ambient metric in $$\RR^4$$ induces a Riemannian manifold
structure on $$S^3$$, which plays nicely with the Lie group structure
as we shall see.

### Bi-invariant Metric

The ambient metric on $$\RR^4$$ induces a metric each fiber (tangent
space) of the tangent bundle $$TS^3$$. This metric is left-invariant:

$$ \inner{u}{v}_{\RR^4} = \inner{qu}{qv} $$

It is also right invariant (and thus also $$\Ad$$-invariant):

$$ \inner{u}{v}_{\RR^4} = \inner{uq}{vq} $$

To see both of these, it is useful to come back to the matrix
representation of quaternions, in which the metric is:

$$ \inner{u}{v} = \trace{u^Tv} $$

So the Lie algebra metric, extended to the whole manifold by left or
right translation, is a *Riemannian metric* (the one induced by the
ambient space). An interesting consequence is that the Lie group
exponential can be used to compute geodesics for this metric.

### Geodesics

Geodesics are locally shortest paths between points in the
manifold. The fact that the Riemannian metric is bi-invariant implies
that curves of the form:

$$ q \exp(t x),\quad t\in\RR $$

are geodesic curves (and all geodesics are of this form). We can
obtain geodesic curves joining two unit quaternions $$a, b \in S^3$$
as follows:

$$ f(t) = a\exp\block{t \log\block{ \inv{a} b } } $$

This is generally referred to as the *spherical linear interpolation*,
or SLERP, and is used to interpolate the corresponding rotations.

### Geodesic Distance

The Riemannian metric induces a distance on the manifold, obtained by
measuring the length of the shortest geodesic curve between two unit
quaternions. The length of the geodesic between $$a$$ and $$b$$ (and
thus the geodesic distance) is:

$$ d(a, b) = \norm{\log\block{\inv{a}b}} $$

Expanding a little bit, we have:

$$
\begin{align} 
d(a, b) &= \arccos\block{w_{\inv{a}b}} \\
&= \arccos\block{ w_a w_b + v_a^T v_b } \\
\end{align}
$$

which is exactly the *angle* between vectors $$a$$ and $$b$$, as one
would expect from vectors on a sphere.

# Misc.


## Polar Decomposition Derivative

We have seen that any unit quaternion can be written as:

$$ q = \cos(\theta) + \sin(\theta) n $$

where $$\theta \in S^1$$ and $$n \in S^2$$ (which gives the
[Hopf fibration](http://en.wikipedia.org/wiki/Hopf_fibration)). We now
consider the derivative of this formula:

$$ \dd q = -sin(\theta) \dd \theta + cos(\theta) n \dd \theta + \sin(\theta) \dd n $$

To keep things readable, we introduce $$c = \cos(\theta)$$ and $$s =
\sin(\theta)$$, and remark that $$n^T\dd n = 0$$ since $$n \in S^2$$,
$$n\dd n = n \times \dd n$$, and $$n^2 = -1$$. The body-fixed
derivative is:

$$
\begin{align}
\db q &= \inv{q} \dd q \\
&= (c - s n) (-s \dd \theta + c n \dd \theta + s \dd n) \\
&= -cs \dd \theta + c^2 n \dd \theta + cs \dd n + s^2 n \dd \theta -sc n^2 \dd \theta - s^2 n \dd n  \\
&= n\ \dd \theta + cs\ \dd n - s^2 n\ \dd n\\
&= n\ \dd \theta + s \block{c  - s n} \dd n\\
\end{align}
$$

One can verify that $$(c - s n) \dd n = R_{-\theta, n} \dd n$$, so
that:

$$
\begin{align}
\db q &= R_{-\theta, n}\block{n\ \dd \theta + s\ \dd n}
\end{align}
$$

When $$q \neq 1, -1$$, the formula above provides the orthogonal
decomposition:

$$
\begin{align}
\dd \theta &= n^T \db q\\
\dd n &= \frac{1}{\sin(\theta)} R_{\theta, n} \block{I - nn^T}\db q\\
\end{align}
$$



## Conversion to Rotation Matrix 

Let $$q = (w, v)$$ be a unit quaternion. The corresponding rotation
$$\Ad_g$$ is:

$$
\begin{align}
\Ad_g x &=q x \bar{q} \\
&= (w + v) x ( w - v) \\
&= w^2 x + w \block{vx - xv} - vxv\\
&= w^2 x + w \ad(v) x - vxv\\
\end{align}
$$

One can check that:

$$ -vxv = v v^T x + \hat{v}^2 x $$

and we have seen that $$\ad(v) = 2 \hat{v} $$, so the final formula
is:

$$
\begin{align}
\Ad_g &= w^2 I + 2w \hat{v} + v v^T + \hat{v}^2 \\
&= I + 2w \hat{v} + \norm{v}^2\block{ \frac{v v^T}{\norm{v}^2} - I} + \hat{v}^2 \\
\end{align}
$$

Note the similarity with Rodrigues' formula (except it's written using
$$\frac{\theta}{2}$$).


## Interpolation

As seen above, the SLERP between unit quaternions $$a$$ and $$b$$ is
expressed as:

$$ f(t) = a\exp\block{t \log\block{ \inv{a} b } } $$

Kim et al[^Kim95] propose spline-like interpolation curves for unit
quaternions $$\block{q}_{i \geq 0}$$ as follows: the sline basis
functions $$B_i(t)$$ are first transformed to cumulative basis
functions $$\tilde{B}_i(t) = B_i(t) - B_{i-1}(t)$$, then the curve is
obtained as:

$$ f(t) = q_0 \prod_{i=1}^n \exp\block{ \tilde{B}_i(t) \log\block{q_{i-1}^{-1}q_i}} $$

The authors report low acceleration/torque and easy to compute
derivatives. This scheme is easily generalizable to other Lie groups.

[^Kim95]: Kim, Myoung-Jun, Myung-Soo Kim, and Sung Yong Shin. *"A general construction scheme for unit quaternion curves with simple high order derivatives."* Proceedings of the 22nd annual conference on Computer graphics and interactive techniques. ACM, 1995.

## Averaging

The Euclidean mean minimizes the sum of distances squared to all the
points considered. This property is easily generalized to Riemannian
manifolds, provided the minimizer exists and is unique. Since $$S^3$$ is
compact, the existence is granted, but the unicity is not (think of
averaging antipodal points).

Arsigny et al[^Arsigny06], as others before[^Moakher02], describe a
fast, gradient-descent-like iterative scheme to compute the *geometric
mean* of $$n$$ unit quaternions $$(q_i)_{i \geq 1}$$. The algorithm
produces successive estimates of the geometric mean $$\mu_k$$ as
follows:

$$
\begin{align}
\mu_0 &= 1 \\
\mu_{k+1} &= \mu_k \exp\block{ \frac{1}{n} \sum_{i=0}^n \log\block{ \mu_k^{-1} q_i } } \\
\end{align}
$$

That is: the data is linearized in the tangent space at $$\mu_k$$,
then the Euclidean mean is computed, and the exponential maps the
result back to the group. The algorithm stops when the tangent
Euclidean mean is sufficiently close to zero. This algorithm
generalizes easily to other Lie groups, but the existence might be
harder to establish.

Right translations can of course be used instead, depending on the
problem.

[^Arsigny06]: Arsigny, Vincent, Xavier Pennec, and Nicholas Ayache. *"Bi-invariant means in Lie groups. Application to left-invariant polyaffine transformations."* (2006).

[^Moakher02]: Moakher, Maher. *"Means and averaging in the group of rotations."* SIAM journal on matrix analysis and applications

## Fast Square Root

The Cayley map for imaginary quaternions has an interesting geometric
interpretation:

$$
\begin{align}
Cay(x) &= \frac{1 - x}{1 + x} \\
&= \frac{ \block{1 - x}^2}{ \block{1 - x}{1 + x} } \\
&= \block{ \frac{1 - x}{\norm{1 - x}} }^2 \\
\end{align}
$$

That is:

1. flip the vector in $$\alg{s^3}$$ (multiply by $$-1$$)
2. add $$1$$ to the real part (go to $$\HH$$)
3. project to $$S^3$$
4. square the result

Now, since $$Cay$$ is its own inverse, one can interpret its
computation for a unit quaternion as the following (reversed) steps:

1. square root
2. unproject to $$1 + \alg{s^3}$$ (divide by real part)
3. substract $$1$$ (project to $$\alg{s^3}$$)
4. flip the result

So the square root can be expressed in terms of $$Cay$$ and other
simple operations. In particular, we have:

$$q^{\half} = \pi_{S^3} \block{ 1 - \frac{1 - q}{1 + q} }$$

A much simpler formula can also be obtained:

$$q^\half = \pi_{S^3} \block{q + \norm{q}^2}$$

That is: add the squared norm of a quaternion to its real part, then
normalize the result. Sweet !

TODO find out whether we can obtain a of such formula for different
powers, probably using the magnitudes of $$Cay(q)$$ and $$\log(q)$$.

## TODO Exponential Map Derivative

## Geodesic Projection

There is a closed-form solution to the problem of the geodesic
projection of a unit quaternion to a geodesic. This problem arises in
non-Euclidean generalizations[^Fletcher04] of the Principal Component
Analysis.

Assuming we are given a geodesic curve starting at $$1$$, with
(normalized) direction $$n \in S^2$$, and a unit quaternion $$q$$, we
consider the following optimization problem:

$$ \argmin{\alpha \in \RR}\quad f(\alpha) = \half d\block{\exp\block{\alpha n}^{-1}, q}^2 $$

We may rewrite the objective function a little bit:

$$
\begin{align}
f(\alpha) &= \half \norm{ \log \block{ \exp\block{-\alpha n} q } }^2 \\
\end{align}
$$

We introduce $$e = \exp\block{-\alpha n}$$ and $$u = e q$$. The
derivative, using *spatial* coordinates where needed, is:

$$
\begin{align}
\dd f(\alpha) &= -\log(u)^T \ds \log(u) \ds \exp(-\alpha n) n \\
&=  -\log(u)^T n \\
\end{align}
$$

(TODO we need the derivatives of $$\exp$$ and $$\log$$)

The first-order optimality conditions for our problem imply that:

$$ \log(u)^T n = 0 $$

Since the logarithm will be along the imaginary part, it is equivalent
to ask for the imaginary part of $$u$$ to be orthogonal to $$n$$:

$$ v_u = w_e v_q + w_q v_e + v_e \times v_q $$

$$v_e = -\sin (\alpha) n$$ so we may drop the cross product
which will be orthogonal to $$n$$. We are left with:

$$ w_e v_q + w_q v_e \quad \bot \quad n $$

Or:

$$ \cos(\alpha) v_q^T n - w_q \sin(\alpha) = 0 $$

- if $$w_q \neq 0$$:
	
	$$
	\begin{align}
	\frac{v_q^T n}{w_q} &= \tan(\theta) \\
	\alpha &= \theta \mod \pi
	\end{align}
	$$

	With $$x = \frac{v_q^T n}{w_q}$$, the projected quaternion is:

	$$\exp\block{\theta n} = \frac{1 + x n}{\sqrt{1 + x^2}} = \frac{1 + nn^T\frac{v_q}{w_q}}{\sqrt{1 + x^2}}$$

	Geometrically, this corresponds to:

	1. (in $$\HH$$) project $$q$$ to the Lie algebra by dividing it by
       its real part, so that $$\pi(q) = 1 + \frac{v_q}{w_q} \in 1 +
       \alg{s^3}$$. This corresponds to scaling $$q$$ until it reaches
       the tangent space at the identity.
	2. (in $$\alg{s^3}$$) project the imaginary part of the result
       $$\frac{v_q^T n}{w_q}$$ orthogonally along $$n$$
    3. (in $$\HH$$) project back the result onto $$S^3$$ by normalizing

	This is exactly the
    [gnomonic projection](http://en.wikipedia.org/wiki/Gnomonic_projection)
    for $$S^3$$, as one could expect.

	It seems that the action of unprojecting from the tangent space is
    exactly the square root of the Cayley transform, which would be
    awesome since geodesic projections would then be kinda
    Cayley-invariant. This needs more work though.

- if $$v_q^T n \neq 0$$:

	$$
	\begin{align}
	\frac{w_q}{v_q^T n} &= \tan(\theta) \\
	\alpha &= \pi / 2 - \theta  \mod \pi
	\end{align}
	$$
  
TODO figure out whether Said et al[^Said07] got the same formula.
  

[^Fletcher04]: Fletcher, P. Thomas, et al. *"Principal geodesic analysis for the study of nonlinear statistics of shape."* Medical Imaging, IEEE Transactions on 23.8 (2004): 995-1005.

[^Said07]: Said, Salem, et al. *"Exact principal geodesic analysis for data on so (3)."* 15th European Signal Processing Conference (EUSIPCO-2007). EURASIP, 2007.

## Conversion to/from Euler Angles

# References
