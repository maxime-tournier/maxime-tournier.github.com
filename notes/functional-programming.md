---
title: Functional Programming
categories: [prog, math]
---

A few notes on Programming Language Theory (PLT) and Functional
Programming (FP) idioms.


# Functors

# Parametricity/Free Theorems

# Recursion Schemes

# Monads

# Free Monads

Some mathematicals objects are *free* in the sense that there's always
of getting one from nothing: for instance, given a set $$S$$ one can
build the *free monoid* $$S^*$$ the set of lists of elements of $$S$$
under concatenation. It costs no extra assumption on $$S$$ to
construct it, and in some sense it is not very useful apart from this
one property: any other monoid on $$S$$ may be obtained from it by a
suitable *evaluation* of lists of elements of $$S$$.

This intuition can be made precise by seeing free objects as initial
objects in a suitable category (the comma category), but the
"intuitive" idea is that free objects provide a syntactic basis with
just enough structure to meet some algebraic requirements (for
instance monoid, group, vector space) from which other objects can be
obtained e.g. by quotienting using an equivalence relation. For
instance, one can obtain a monoid homomorphism $$\NN^* \to (\NN, +)$$
by quotienting $$\NN^*$$ by the smallest monoid congruence satisfying:

- $$(m, n) ~ (n, m)$$ (commutativity)
- $$(m, n) ~ (m + n)$$ (additivity)

See also the tensor product/exterior algebra for similar
constructions.

What about free *monads*, then? Since "monads are just monoids in the
category of endofunctors", we expect free monads to be free monoids in
the category of endofunctors.



