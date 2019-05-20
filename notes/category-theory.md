---
title: Category Theory
categories: [math]
---

# Category

A category $$\mathcal{C}$$ is given by the following:

- a class[^1] of objects $$\Ob{C}$$
- a class of morphisms $$\Hom{C}$$, whose elements $$f$$ have a *source object*
  and a *target object*, denoted by $$f: a \to b$$. The class of morphisms from
  $$a$$ to $$b$$ is denoted by $$\hom{C}{a, b}$$.
  
satisfying the following properties:

- for all morphisms $$f: a \to b$$ and $$g: b \to c$$, there exists a unique
  morphism $$g \circ f:a \to c$$ called the *composition* of $$g$$ and $$f$$,
  and the composition is *associative*:
  
   $$h \circ \block{g  \circ g} = \block{h \circ g} \circ f$$

- for all objects $$c \in \Ob{c}$$, there exists an *identity morphism* $$1_c \in
  \hom{C}{c, c}$$ such that for any morphism $$f: a \to b$$, the following holds:
  
  $$1_b \circ f = f = f \circ 1_a$$
  
When $$\hom{C}{a, b}$$ are all sets, the category $$\cat{C}$$ is said *locally
small*, which is generally the case for all practical purposes. Likewise, when
$$\Hom{C}$$ is a set, the category is said *small*.

# Functors

# Natural Transformations

# Initial and Terminal Objects

# Products and Coproducts

# Pushouts and Pullbacks

# Limits and Colimits

# Exponentials

# Adjunctions

# Monads


# Yoneda's Lemma




# Notes & References

[^1]:
    [classes](https://en.wikipedia.org/wiki/Von_Neumann%E2%80%93Bernays%E2%80%93G%C3%B6del_set_theory)
    are a syntactic tool to avoid logical paradoxes arising when considering
    *e.g.* the set of all sets not containing themselves (Russel's paradox)
