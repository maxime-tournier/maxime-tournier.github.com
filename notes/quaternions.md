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

$$ \mat{ x & y \\ -\bar{y} & \bar{x} } $$

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
product, this space forms the quaternions, noted $\HH$.

We will generally use the *real* vector space structure, and identify
a quaternion with its coordinates $$(w, x, y, z)$$ in the canonical
basis.

## Canonical Basis

The 4 basis vectors for the *real* vector space structure are commonly
noted $$(1, i, j, k)$$ and verify:

$$ i^2 = j^2 = k^2 = ijk = -1 $$


These formula are actually sufficient to define the quaternions, and
were carved on /Brougham (Broom) Bridge/ in Dublin by *Hamilton*, on
the 16th of October 1843 (yay wikipedia).
  
In the remaining of this document, we will denote quaternions using
the following notations:
  
$$ q = (w, x, y, z) =: w + v $$
  
where $$v$$$ is a pure imaginary quaternion with coordinates $$(0, x,
y, z)$$.
