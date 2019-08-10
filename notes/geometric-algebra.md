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
we're asking of the geometric product is that its symmetric part be
the inner product:

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
- the symmetric part of the geometric product is the inner product

# Basis


# References 

[^1]: Alan MacDonald, *[A Survey of Geometric Algebra and Geometric
     Calculus](http://www.faculty.luther.edu/~macdonal/GA&GC.pdf)*,
     Adv. Appl. Cliff. Alg. 27, 853–891 (2017).
