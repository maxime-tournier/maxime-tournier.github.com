---
title: Geometric Algebra
categories: [math]
---

Vectors can be combined to build parallelograms, parallelograms can be
combined with vectors to build parallelepipeds, and so on. And then on
can scale all of these to obtain new objects of the same
kind. Geometric Algebra gives a precise meaning to this composition
operation, called the *geometric product*, which can serve as the
basis for performing most geometric operations on geometric objects,
in any dimension.

Most of the following is adapted from the online resource by Alan
MacDonald[^1].

{% include toc.md %}

# Geometric Product

As mentioned in the introduction, the geometric product is essentially
a *composition* operation on geometric objects. As such, it should
enjoy a few basic properties to be [well-behaved](category-theory):

- the composition should be associative: $$(ab) c = a (bc)$$
- there should be an identity object, denoted $$1$$, such that for any
  object $$x$$:

$$1 x = x 1 = x$$

Furthermore, the space of $$n$$-dimensional geometric objects
$$\GG^n$$ (whatever they are, at this point) should be closed under
the geometric product, making $$\GG^n$$ a monoid. 

Let us populate $$\GG^n$$ with vectors from $$\RR^n$$, and examine how
the geometric product plays with the existing composition on vectors:
vectors can be added and scaled by real numbers to obtain new
vectors. So we can *define* the sum of two parallelograms by summing
their constituent vectors pairwise:

$$(u_1  v_1) + (u_2  v_2) \eqdef (u_1 + u_2)  (v_1 + v_2)$$ 

This generalises to arbitrary geometric objects, and the geometric
product is distributive over the (commutative) sum as just
defined. Similarly, we can extend scalar multiplication on vectors as:

$$(\lambda u) v = \lambda (u v)$$

One can easily check that the sum and scalar multiplication make
$$\GG^n$$ a vector space. Also, we see that scalars end up being
geometric objects in their own right, spanned by the unit element
$$1$$ (called $$0$$-vectors). So far, we've seen how the geometric
product behaves internally and with respect to the vector space
structure, but we still don't know how to compute it in
practice. However, we *do* know that it is bilinear so it has a
symmetric part and an antisymmetric part:

$$uv = \half\block{uv + vu} + \half\block{uv - vu}$$

Let us now add that tiny bit of Euclidean geometry giving the
geometric product all its power (and name). Consider an inner product
$$\cdot$$ over $$\RR^n$$: it is a symmetric positive bilinear
form. All that we're asking of the geometric product of two vectors is
that its symmetric part be the inner product:

$$\half\block{uv + vu} = u\cdot v$$

From this, we immediately obtain $$u^2 = u\cdot u = \norm{u}^2$$, and
every non-zero vector $$u \neq 0$$ has an inverse:

$$\quad u^{-1} = \frac{u}{\ \norm{u}^2}$$

Likewise, when $$u\cdot v = 0$$ only the anti-symmetric part remains,
hence:

$$uv = -vu$$

Summarizing, we obtained that:

- $$\GG^n$$ is a vector space of geometric objects, containing vectors
  of $$\RR^n$$
- $$\GG^n$$ is a monoid for the geometric product, which is bilinear
- the symmetric part of the geometric product is the inner product for
  vectors

# Basis

Given an orthonormal orthogonal basis $$e_1, e_2, e_3$$ for $$\RR^3$$,
we can quickly check than the following family forms a basis for
$$\GG^3$$:

$$
\begin{matrix}
1 & \text{scalars} \\
e_1,\ e_2,\ e_3 &  \text{vectors} \\
e_1 e_2,\ e_1 e_3,\ e_2 e_3 & \text{bi-vectors} \\
e_1 e_2 e_3 & \text{tri-vectors} \\
\end{matrix}$$

Scalars are called $$0$$-vectors, vectors of $$\RR^n$$ are called
$$1$$-vectors and so on. General elements of $$\GG^n$$ are called
multi-vectors. This basis is called the *canonical* basis due to the
arrangement of the indices in strict increasing order. Similarly, for
$$\GG^4$$ one obtains:

$$
\begin{matrix}
1 & \text{scalars} \\
e_1,\ e_2,\ e_3,\ e_4 &  \text{vectors} \\
e_1 e_2,\ e_1 e_3,\ e_1 e_4,\ e_2 e_3,\ e_2 e_4,\ e_3 e_4 & \text{bi-vectors} \\
e_1 e_2 e_3,\ e_1 e_2 e_4,\ e_1 e_3 e_4,\ e_2 e_3 e_4 & \text{tri-vectors} \\
e_1 e_2 e_3 e_4 & \text{quadri-vectors} \\
\end{matrix}$$

A quick induction shows that $$\GG^n$$ has dimension $$2^n$$. Since
the geometric product is associative, finding inverses for basis
$$k$$-vectors is easy:

$$\block{e_{i_k} \ldots e_{i_1}}\block{e_{i_1} \ldots e_{i_k}} = 1$$

The inverse is another $$k$$-vector which can be reordered to the
original canonical basis $$k$$-vector by a number $$m$$ of swaps,
giving:

$$\block{e_{i_1} \ldots e_{i_k}}^{-1} = (-1)^{m} \block{e_{i_1} \ldots e_{i_k}}$$

In particular, basis bi-vectors satisfy:

$$\block{e_j e_k}^2 = -1$$

which provides the connection with complex numbers and quaternions.

# Outer Product

Since the symmetric part of the geometric product for two vectors is
the inner product, the anti-symmetric part is logically called the
*outer product* (or *exterior product*), denoted by
$$\wedge$$[^wedge]:

$$uv = u\cdot v + u \wedge v$$

For two vectors $$u, v$$ in the plane $$e_1e_2$$, their geometric
product is:

$$\underbrace{(a e_1 + b e_2)}_u\underbrace{(c e_1 + d e_2)}_v =
\underbrace{(ac + bd)}_{u\cdot v} + \underbrace{(ac - bd) e_1 e_2}_{u
\wedge v}$$

hence the outer product $$u\wedge v$$ has coefficient $$ac -
bd=\norm{u}\norm{v}\sin(\alpha)$$, which is the signed area of the
parallelogram delimited by $$u$$ and $$v$$ (where $$\alpha$$ is the
angle between them). The following formula hints at the connection
with complex numbers and quaternions:

$$uv = \norm{u}\norm{v}\block{\cos(\alpha) + \sin(\alpha) e_1e_2}$$

More generally, the product of $$1$$-vectors always has a scalar part
and a bi-vector part. The outer product can be extended inductively
from vectors to the whole geometric algebra by defining:

$$x \wedge y = \inner{x y}_{m + n}$$

for a $$m$$-vector $$x$$ and an $$n$$-vector $$y$$, and where
$$\inner{z}_k$$ is the *grade projection* operator selecting the
$$k$$-vector part of a multivector $$z$$. The outer product as defined
remains associative, antisymmetric and bilinear. In particular, the
outer product of linearly dependent vectors is zero.

# Norm

The Euclidean norm on vectors extends naturally to multi-vectors: let
$$x = \sum x_I e_I$$, where $$e_I=\Pi_{i \in I} e_i$$ and $$I$$ ranges
over the set of canonical basis index sets[^index], the norm of $$x$$
is defined by:

$$\norm{x}^2 = \sum x_I^2$$

One can verify that it is indeed a norm, and that it does not depend
on the choice of a canonical basis for $$\GG^n$$.

# Blades

Blades represent linear subspaces of $$\RR^n$$, and can be defined
equivalently as:

- exterior products of linearly independent $$1$$-vectors
- geometric products of orthogonal $$1$$-vectors



Let $$B \subseteq \RR^n$$ be a vector space of dimension $$k$$ and
$$\block{b_i}_{i\leq k}$$ an orthogonal basis of $$B$$, then the
product:

$$b = b_1 \ldots b_k$$ 

is called a *blade* of grade $$k$$, or $$k$$-blade. In particular, the
canonical basis vectors are all blades. Of particular interest is
*the* canonical $$n$$-vector, called the *pseudo-scalar*:

$$1^* \eqdef e_1 \ldots e_n$$

An important property is that the pseudo-scalar does not depend on the
orthonormal basis (except for sign). If $$e'_1 \ldots e'_n$$ is another
orthonormal basis for $$\RR^n$$, then $$e'_1 \ldots e'_n$$ is a unit
$$n$$-vector, therefore:

$$e'_1 \ldots e'_n = \pm 1^*$$ 

The multiplication of a basis $$k$$-vector $$e$$ by $$1^*$$
cancels-out every occurence of basis $$1$$-vector in $$e$$ and adds-in
every missing basis $$1$$-vector, up to a sign. For instance, in
$$\GG^3$$:

$$e_1 e_3 1^* = e_2$$

This operation defines the *dual* of a multi-vector $$x\in\GG^n$$:

$$x^* = x 1^*$$

One can easily show that the dual of a $$k$$-blade is an
$$(n-k)$$-blade representing the orthogonal complement of the original
subspace: our $$k$$-blade can be completed into an orthonormal basis
of the whole space, and multiplication by the pseudo-scalar for this
basis ($$\pm 1^*$$) gives the result. Finally, we note that the sum of
$$k$$-blades is generally *not* a blade, except when $$k=1$$ and
$$k=n-1$$[^blade].

# References 

[^1]: Alan MacDonald, *[A Survey of Geometric Algebra and Geometric
     Calculus](http://www.faculty.luther.edu/~macdonal/GA&GC.pdf)*,
     Adv. Appl. Cliff. Alg. 27, 853–891 (2017).
     
[^wedge]: It is quite unfortunate that the $$\wedge$$ symbol has been
    used for something acting like a *join* in lattice theory, which
    has symbol $$\vee$$ (like a union)

[^blade]: Use the dual twice.

[^index]: *e.g.* for $$\GG^3$$: $$I \in \set{\set{0}, \set{1}, \set{2}, \set{3}, \set{1, 2}, \set{1, 3}, \set{2, 3}, \set{1, 2, 3}}$$
