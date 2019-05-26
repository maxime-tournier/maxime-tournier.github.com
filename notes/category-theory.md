---
title: Category Theory
categories: [math]
---

Some more general abstract nonsense.

{% include toc.md %}

# Categories

A category $$\mathcal{C}$$ is given by the following:

- a class[^1] of *objects* $$\Ob{C}$$
- a class of *morphisms* $$\Hom{C}$$, whose elements $$f$$ have a *source* object
  and a *target* object, denoted by $$f: a \to b$$. The class of morphisms from
  $$a$$ to $$b$$ is denoted by $$\hom{C}{a, b}$$.
  
satisfying the following properties:

- for all morphisms $$f: a \to b$$ and $$g: b \to c$$, there exists a unique
  morphism $$g \circ f:a \to c$$ called the *composition* of $$g$$ and $$f$$,
  and the composition is *associative*:
  
   $$h \circ \block{g  \circ g} = \block{h \circ g} \circ f$$

- for all objects $$c \in \Ob{c}$$, there exists an *identity
  morphism* $$\id_c \in \hom{C}{c, c}$$ such that for any morphism
  $$f: a \to b$$, the following holds:
  
  $$\id_b \circ f = f = f \circ \id_a$$
  
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

## Product Category

# Functors

Functors are structure-preserving maps between categories: they preserve
identities and composition. Let us consider two categories $$\cat{C}$$ and
$$\cat{D}$$, a functor $$F$$ between them is given by:

- for each object $$c \in \Ob{C}$$, a unique object $$F(c) \in\Ob{D}$$
- for each morphism $$f: a \to b \in \Ob{C}$$, a unique morphism
  $$F(f): F(a) \to F(b)$$

satisfying the following properties:

- for all objects $$c \in \Ob{C}$$, identity morphisms are preserved:

$$F\block{\id_c} = \id_{F(c)}$$

- for all morphisms $$f: a \to b$$ and $$g: b \to c$$, their
  composition is preserved:

$$F\block{g \circ f} = F(g) \circ F(f)$$

In particular, functors can never "disconnect" connected
objects. There is a category $$\mathrm{Cat}$$ of locally small
categories, in which functors are the morphisms, which justifies the
notation of functors as morphisms:

$$F: \cat{C} \to \cat{D}$$

## Diagrams

# Natural Transformations

Natural transformations are the categorical equivalent of homotopies:
given two categories $$\cat{C}$$ and $$\cat{D}$$ and two functors $$F:
\cat{C} \to \cat{D}$$ and $$G: \cat{C} \to \cat{D}$$, a *natural
transformation* between $$F$$ and $$G$$ is a functor:

$$\alpha: \cat{C} \times \cat{I} \to \cat{D}$$

where $$\cat{I}$$ is the so-called *interval category* with a unique
non-trivial morphism between two objects:

$$\cat{I} = \overset{0}\cdot \overset{!}\to \overset{1}\cdot$$

and where $$\alpha$$ satisfies the following property:

$$
\begin{align}
	\alpha\block{-, 0} &= F \\
	\alpha\block{-, 1} &= G \\
\end{align}
$$

Let us fix an object $$x \in \Ob{C}$$, the image of morphism
$$\block{\id_x, !}$$ by $$\alpha$$ is a morphism:

$$\alpha\block{\id_x, !}: F(x) \to G(x)$$

called the *component* of $$\alpha$$ at $$x$$. Since there is a unique
such morphism $$\block{\id_x, !}$$ for each $$x$$, its image by
$$\alpha$$ is denoted by $$\alpha_x$$. Now let $$f: x \to y$$ be a
morphism in $$\Hom{C}$$, we consider the following morphism in
$$\Hom{C \times I}$$:

$$(f, !): (x, 0) \to (y, 1)$$

This morphism decomposes as:

$$\begin{align}
	(f, !) &= \block{f, \id_1} \circ \block{\id_x, !} \\
	       &= \block{\id_y, !} \circ \block{f, \id_0} \\
\end{align}$$

hence its image by $$\alpha$$ satisfies:

$$\begin{align}
\alpha(f, !) 
&= \alpha\block{f, \id_1} \circ \alpha\block{\id_x, !} \\
&= \alpha\block{\id_y, !} \circ \alpha\block{f, \id_0} \\
\end{align}$$

which can be rewritten succintly as:

$$\block{G(f), \id_1} \circ \alpha_x = \alpha_y \circ \block{F(f), \id_0}$$

In other words, the following diagram commutes:

$$\begin{matrix}
	F(x) & \overset{F(f)}\longrightarrow & F(y)  \\
	\alpha(x) \downarrow &  & \downarrow \alpha(y) \\
	G(x) & \underset{G(f)}\longrightarrow & G(y) \\
\end{matrix}$$

This diagram is called the *naturality square* for $$\alpha$$. The
natural transformation $$\alpha$$ is completely determined by its
components (and associated naturality squares).

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
