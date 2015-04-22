---
title: Quaternions
---

Quaternions are primarily used as a representation of space
rotations. As such, they turn out to be quite convenient to use in
practice due to their compactness and nice geometric properties. Most
operations on rotations can be expressed by means of quaternion
representation, which generally offers a somewhat more intuitive point
of view.



# Construction

## Complex Numbers
   
We start by building the complex numbers as the real $$\small{2 \times 2}$$
matrices of the form:

   $$ \mat{ 
   x  & y  \\
   -y & x  \\
   } $$
   
The above set is obviously a $$\small{2}$$-dimensional vector
space, whose basis is noted $$(1, i)$$. One can check that $$i^2 = -1$$.
   
This space is closed under matrix multiplication, and one can show
that this space, together with this (commutative!) multiplication, form
a *field*.

## Quaternions

We repeat this construction, this time with *complex* matrices:

$$ \mat{ x & y \\ -\bar{y} & \bar{x} } $$

We end up with a 2-dimensional *complex* vector-space, that is also
a 4-dimensional *real* vector space, given by real $$\small{4 \times
4}$$ matrices of the form:

$$ \mat{ 
	w &  x &  y &  z \\
	-x &  w & -z &  y \\
	-y &  z &  w & -x \\
	-z & -y &  x &  w \\ 
   }
   $$


	

