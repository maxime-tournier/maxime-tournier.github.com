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
  and a *target* object, denoted by $$f: a \to b$$. 
  
The class of morphisms from $$a$$ to $$b$$ is denoted by $$\hom{C}{a,
  b}$$. For $$\cat{C}$$ to be a category, it must satisfy the
  following properties:

- for all morphisms $$f: a \to b$$ and $$g: b \to c$$, there exists a
  morphism $$g \circ f:a \to c$$ called the *composition* of $$g$$ and
  $$f$$, and the composition is *associative*:
  
   $$h \circ \block{g  \circ f} = \block{h \circ g} \circ f$$

- for all objects $$c \in \Ob{c}$$, there exists an *identity
  morphism* $$\id_c \in \hom{C}{c, c}$$ such that for any morphism
  $$f: a \to b$$, the following holds:
  
  $$\id_b \circ f = f = f \circ \id_a$$
  
One can easily check that identity morphisms are unique. When $$\hom{C}{a, b}$$
are all sets, the category $$\cat{C}$$ is said *locally small*, which is
generally the case for all practical purposes. Likewise, when $$\Hom{C}$$ is a
set, the category is said *small*. 


## Morphisms

A morphism $$f: a \to b$$ is:

- a *monomorphism* when $$f \circ g_1 = f \circ g_2 \Rightarrow g_1 = g_2$$ for all $$g_1, g_2: x \to a$$
- an *epimorphism* when  $$g_1 \circ f = g_2 \circ f \Rightarrow g_1 = g_2$$ for all $$g_1, g_2: b \to y$$
- an *isomorphism* when there exists $$g: b \to a$$ such that $$\id_a = g \circ f$$ and $$\id_b = f \circ g$$

## Opposite Category

Given one category $$\cat{C}$$, one can construct its *opposite category*
$$\op{C}$$, obtained from the same objects and reversing all morphisms:

$$f^{op} \in \hom{\op{C}}{b, a} \iff f \in \hom{C}{a, b}$$

## Product Category


## Examples

### Preorders

### Monoids

### Graphs


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
categories, in which functors are the morphisms.

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

In other words, the following diagram is *commutative*, meaning that all
parallel arrows are equal:

$$\begin{matrix}
	& F(x) & \overset{F(f)}\longrightarrow & F(y) & \\
	\alpha_x \hspace{-1.5em} &\downarrow &  & \downarrow & \hspace{-1.5em} \alpha_y \\
	& G(x) & \underset{G(f)}\longrightarrow & G(y) & \\
\end{matrix}$$

This commutative diagram is called the *naturality square* for
$$\alpha$$. The natural transformation $$\alpha$$ is completely
determined by its components (and associated naturality squares),
which are generally used as its definition.

## Functor Categories

Given three functors $$F, G, H: \cat{C} \to \cat{D}$$ and natural
transformations $$\alpha: F \to G$$ and $$\beta: G \to H$$, their
*vertical composition* $$\beta \circ \alpha$$ has components:

$$(\beta \circ \alpha)_x = \beta_x \circ \alpha_x$$

One can easily check the naturality conditions from those of
$$\alpha$$ and $$\beta$$:

$$\begin{matrix}
	& F(x) & \overset{F(f)}\longrightarrow & F(y) & \\
	\alpha_x \hspace{-1.5em} &\downarrow &  & \downarrow & \hspace{-1.5em} \alpha_y \\
	& G(x) & \overset{G(f)}\longrightarrow & G(y) & \\
	\beta_x \hspace{-1.5em} &\downarrow &  & \downarrow & \hspace{-1.5em} \beta_y \\
	& H(x) & \underset{H(f)}\longrightarrow & H(y) & \\
\end{matrix}$$

Moreover, for any functor $$F: \cat{C} \to \cat{D}$$ there exists an
*identity* natural transformation $$\id_F$$ from $$F$$ to itself,
whose components are the identity morphisms:

$$(\id_F)_x: F(x) \to F(x) = \id_{F(x)}$$

Therefore, we just obtained another category called the *functor
category*, denoted by $$[\cat{C}, \cat{D}]$$, where objects are
functors from $$\cat{C}$$ to $$\cat{D}$$ and morphisms are natural
transformations.


# Yoneda's Lemma

## Hom-Functors

Let $$\cat{C}$$ be a locally small category and $$\Set$$ be the
category of sets. Let us also fix some object $$a \in \Ob{C}$$, and
consider the two following functors:

### Covariant

$$\begin{align}
H^a: \cat{C} &\to \mathrm{Set}\\
x &\mapsto \hom{C}{a, x} \\
(f: x \to y) &\mapsto H^a(f): \hom{C}{a, x} \to \hom{C}{a, y} = (g \mapsto f \circ g) \\
\end{align}$$

### Contravariant

$$\begin{align}
H_a: \op{C} &\to \mathrm{Set}\\
x &\mapsto \hom{C}{x, a} \\
(f: y \to x) &\mapsto H_a(f): \hom{C}{x, a} \to \hom{C}{y, a} = (g \mapsto g \circ f) \\
\end{align}$$

One can easily check that both $$H^a$$ and $$H_a$$ are indeed
functors. Now, let us consider the *Yoneda embedding*, which is the
following functor:

$$\begin{align}
Y: \cat{C} &\to [\op{C}, \Set]\\
a &\mapsto H_a \\
(f: a \to b) &\mapsto Y(f): H_a \to H_b \\
\end{align}$$

For $$Y(f)$$ to be a natural transformation from $$H_a$$ to $$H_b$$,
it must have components $$Y(f)_x: H_a(x) \to H_b(x)$$, that is:

$$Y(f)_x: \hom{C}{x, a} \to \hom{C}{x, b}$$

The obvious choice given $$f: a \to b$$ is to have:

$$Y(f)_x = (g \mapsto f \circ g)$$

The naturality square for $$Y(f)$$ is the following:

$$\begin{matrix}
	& H_a(x) & \overset{H_a(f)}\longrightarrow & H_a(y) & \\
	Y(f)_x \hspace{-1.5em} &\downarrow &  & \downarrow & \hspace{-1.5em} Y(f)_y \\
	& H_b(x) & \underset{H_b(f)}\longrightarrow & H_b(y) & \\
\end{matrix}$$

Given any morphism $$g: x \to a \in \hom{C}{x, a} = H_a(x)$$, we obtain:

$$\begin{align}
g &\overset{H_a(f)}\mapsto g \circ f \\
g &\overset{Y(f)_x}\mapsto f \circ g \\
f \circ g &\overset{H_b(f)}\mapsto (f \circ g) \circ f \\
g \circ f &\overset{Y(f)_y}\mapsto f \circ (g \circ f) \\
\end{align}$$

where the two last lines are equal by associativity of morphism
composition, showing the naturality square above indeed commutes and
the Yoneda embedding is indeed a functor.


# Limits and Colimits

## Initial and Terminal Objects

## Products and Coproducts

## Pushouts and Pullbacks

## Equalizer and Coequalizers

## Cones and Cocones


# Exponentials

# Adjunctions

# Monads






# Notes & References

[^1]:
    [classes](https://en.wikipedia.org/wiki/Von_Neumann%E2%80%93Bernays%E2%80%93G%C3%B6del_set_theory)
    are a syntactic restriction to avoid logical paradoxes arising when
    considering *e.g.* the set of all sets not containing themselves (Russel's
    paradox)
