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
   
   We start by building the complex numbers as the real $$2 \times 2$$
   matrices of the form:

   $$ \mat{ 
   x  & y  \\
   -y & x  \\
   } $$
   
   The above set is obviously a $$2$$-dimensional vector
   space, whose basis is noted $$(1, i)$$. One can check that $$i^2 = -1$$.
   
   This space is closed under matrix multiplication, and determinants
   show that this space, together with this (commutative)
   multiplication, form a *field*.

