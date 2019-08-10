---
title: Geometric Algebra
categories: [math]
---

Vectors can be combined to build parallelograms, parallelograms can be
combined with vectors to build parallelepipeds, and so on. And then
you can scale all of these to obtain new objects of the same
kind. Geometric Algebra gives a precise meaning to this composition
operation, called the *geometric product*, which can then serve as the
basis for performing most geometric operations on geometric objects,
in any dimension.

Most of the following is adapted from the excellent online resource by
Alan MacDonald[^1].

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
the geometric product, which makes $$\GG^n$$ a monoid.

Let us add more structure and examine how the geometric product plays
with the existing composition on vectors: vectors in $$\RR^n$$ can be
added and scaled by real numbers to obtain new vectors. So we can
*define* the sum of two parallelograms by summing their constituent
vectors pairwise:

$$(u_1  v_1) + (u_2  v_2) \overset{\text{def}}= (u_1 + u_2)  (v_1 + v_2)$$ 

This generalises to arbitrary geometric objects, and the geometric
product is distributive over the (commutative) sum as just
defined. Similarly, we can extend scalar multiplication on vectors as:

$$(\lambda u) v = \lambda (u v)$$

One can easily check that the sum and scalar multiplication make
$$\GG^n$$ a vector space. Also, we see that scalars end up being
geometric objects in their own right, spanned by the unit element
$$1$$. So far, we've seen how the geometric product behaves internally
and with respect to the vector space structure, but we still don't
know how to compute it in practice. However, we *do* know that it is
bilinear so it has a symmetric part and an antisymmetric part:

$$uv = \half\block{uv + vu} + \half\block{uv - vu}$$

Let us now add that one bit of Euclidean geometry giving the geometric
product all its power (and name). Consider an inner product $$\cdot$$
over $$\RR^n$$: it is a symmetric positive bilinear form. All that
we're asking of the geometric product of two vectors is that its
symmetric part be the inner product:

$$\half\block{uv + vu} = u\cdot v$$

From this we immediately obtain $$u^2 = u\cdot u = \norm{u}^2$$, and
every non-zero vector $$u \neq 0$$ has an inverse:

$$\quad u^{-1} = \frac{u}{\ \norm{u}^2}$$

Likewise, when $$u\cdot v = 0$$, only the anti-symmetric part remains,
hence:

$$uv = -vu$$

Summarizing, we obtained that:

- the geometric algebra $$\GG^n$$ is a vector space of geometric objects
- $$\GG^n$$ is closed under the geometric product, which is bilinear
- the symmetric part of the geometric product of two vector is their
  inner product

# Basis

Given an orthonormal orthogonal basis $$e_1, e_2, e_3$$ for $$\RR^3$$,
we can quickly check than the following family forms a for $$\GG^3$$:

$$
\begin{matrix}
1 & \text{scalars} \\
e_1,\ e_2,\ e_3 &  \text{vectors} \\
e_1 e_2,\ e_1 e_3,\ e_2 e_3 & \text{bi-vectors} \\
e_1 e_2 e_3 & \text{tri-vectors} \\
\end{matrix}$$

This basis is called the *canonical* basis due to the arrangement of
the indices in strict increasing order. Similarly, for $$\GG^4$$ one
obtains:

$$
\begin{matrix}
1 & \text{scalars} \\
e_1,\ e_2,\ e_3,\ e_4 &  \text{vectors} \\
e_1 e_2,\ e_1 e_3,\ e_1 e_4,\ e_2 e_3,\ e_2 e_4,\ e_3 e_4 & \text{bi-vectors} \\
e_1 e_2 e_3,\ e_1 e_2 e_4,\ e_1 e_3 e_4,\ e_2 e_3 e_4 & \text{tri-vectors} \\
e_1 e_2 e_3 e_4 & \text{quadri-vectors} \\
\end{matrix}$$

A quick induction shows that $$\GG^n$$ has dimension $$2^n$$.

# Outer Product

Since the symmetric part of the geometric product is the inner
product, its anti-symmetric is logically called the *outer product*,
denoted by $$\wedge$$[^wedge]:

$$uv = u\cdot v + u \wedge v$$

For two vectors $$u, v$$ in the plane $$e_1e_2$$, their product is:

$$\underbrace{(a e_1 + b e_2)}_u\underbrace{(c e_1 + d e_2)}_v =
\underbrace{(ac + bd)}_{u\cdot v} + \underbrace{(ac - bd) e_1 e_2}_{u
\wedge v}$$

and the outer product $$u\wedge v$$ has coefficient $$ac -
bd=\norm{u}\norm{v}\sin(\alpha)$$, which is the signed area of the
parallelogram delimited by $$u$$ and $$v$$ (where $$\alpha$$ is the
angle between them).

# References 

[^1]: Alan MacDonald, *[A Survey of Geometric Algebra and Geometric
     Calculus](http://www.faculty.luther.edu/~macdonal/GA&GC.pdf)*,
     Adv. Appl. Cliff. Alg. 27, 853–891 (2017).
     
[^wedge]: It is a bit unfortunate to used the $$\wedge$$ symbol for
    something acting like a *join* in lattice theory, which has
    symbol $$\vee$$ (like a union)
