---
title: Category Theory
categories: [math]
---

Some more general abstract nonsense.

# Categories

A category $$\mathcal{C}$$ is given by the following:

- a class[^1] of *objects* $$\Ob{C}$$
- a class of *morphisms* $$\Hom{C}$$, whose elements $$f$$ have a *source object*
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

## Opposite Category

Given one category $$\cat{C}$$, one can construct its *opposite category*
$$\op{C}$$, obtained from the same objects and reversing all morphisms:

$$f^{op} \in \hom{\op{C}}{b, a} \iff f \in \hom{C}{a, b}$$

## Examples

### Preorders
    
### Graph Paths

### Monoids

# Functors

Functors are structure-preserving maps between categories: they preserve
identities and composition. Let us consider two categories $$\cat{C}$$ and
$$\cat{D}$$, a functor $$F$$ between them is given by:

- for each object $$c \in \Ob{C}$$, a unique object $$F(c) = d\in\Ob{D}$$
- for each morphism $$f: a \to b \in \Ob{C}$$, a unique morphism $$F(f): F(a) \to F(b)$$

satisfying the following properties:

- for all objects $$c \in \Ob{C}$$, identities are preserved: 

$$F(1_c) = 1_{F(c)}$$

- for all morphisms $$f: a \to b$$ and $$g: b \to c$$, composition is preserved: 

$$F\block{g \circ f} = F(g) \circ F(f)$$

There is a category $$\mathrm{Cat}$$ of locally small categories, in which
functors are the morphisms.

## Diagrams

# Natural Transformations

# Limits and Colimits

## Initial and Terminal Objects

## Products and Coproducts

## Pushouts and Pullbacks

## Equalizer and Coequalizers

## Cones and Cocones


# Exponentials

# Adjunctions

# Monads


# Yoneda's Lemma




# Notes & References

[^1]:
    [classes](https://en.wikipedia.org/wiki/Von_Neumann%E2%80%93Bernays%E2%80%93G%C3%B6del_set_theory)
    are a syntactic restriction to avoid logical paradoxes arising when
    considering *e.g.* the set of all sets not containing themselves (Russel's
    paradox)
