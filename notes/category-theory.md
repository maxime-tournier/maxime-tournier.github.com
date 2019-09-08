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

A morphism $$f: a \to b$$ is called:

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

A preorder on a set can be seen as a locally small category
$$\cat{C}$$ where the objects are elements of the set and there is
exactly one morphism between any two comparable objects:

$$f: a \to b \iff a \leq b$$

Reflexivity and transitivity axioms of the preorder provide the
requirements for $$\cat{C}$$ to be a category. Associativity comes
from the fact that there is *at most* one morphism between any two
objects.

### Monoids

A monoid can be seen as a category with a single object, and where
morphisms are the elements of the monoid.

### Groups 

A group is a monoid, so it can also be seen as a category with a
single objects and where morphism are the group elements but in this
case, every morphism is an isomorphism.


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

In particular, functors can never "disconnect" connected objects. For
any category $$\cat{C}$$, there exists an *identity* functor
$$\id_{\cat{C}}$$ sending every object to itself and acting similarly
on morphisms. Likewise, functors can be composed:

$$\begin{align}
(F \circ G)(x) &= F(G(x)) \\
(F \circ G)(f: x \to y) &= F(G(f): G(x) \to G(y)): (F \circ G)(x) \to (F \circ G)(y) \\
\end{align}$$

and we immediately check:

- identity: $$(F \circ G)\block{\id_x} = \id_{F(G(x))}$$
- associativity: $$(F \circ G)\block{f \circ g} = F(G(f) \circ G(g)) = \block{F \circ G}(f) \circ \block{F \circ G}(g)$$

In other words, there is a category $$\Cat$$ of locally small
categories, in which functors are the morphisms.

## Full and Faithful Functors

Functors between locally small categories induce functions between
hom-sets. Let $$F: \cat{C} \to \cat{D}$$ be a functor between such
categories $$\cat{C}$$ and $$\cat{D}$$, hom-sets are related by:

$$F_{x, y}: \hom{C}{x, y} \to \hom{D}{F(x), F(y)}$$

The functor $$F$$ is said to be:

- *faithful* if $$F_{x, y}$$ is injective
- *full* if $$F_{x, y}$$ is surjective
- *fully faithful* if $$F_{x, y}$$ is bijective

for every object $$x, y \in \Ob{C}$$.

## Diagrams

Diagrams let us select patterns (*shapes*) of interest from a category
$$\cat{C}$$. Let $$\cat{J}$$ be a small or finite category
representing our pattern, then an occurence of this pattern in
$$\cat{C}$$ is simply a structure-preserving mapping from $$\cat{J}$$
to $$\cat{C}$$. In other words, diagrams $$D$$ of shape $$\cat{J}$$ in
$$\cat{C}$$ are just functors between $$\cat{J}$$ and $$\cat{C}$$:

$$D: \cat{J} \to \cat{C}$$


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

$$\natsq{\alpha}{F}{G}{x}{y}{f}$$

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

Vertical composition is associative since component composition
is. Moreover, for any functor $$F: \cat{C} \to \cat{D}$$ there exists
an *identity* natural transformation $$\id_F$$ from $$F$$ to itself,
whose components are the identity morphisms:

$$(\id_F)_x: F(x) \to F(x) = \id_{F(x)}$$

Therefore, we just obtained another category called the *functor
category*, denoted by $$\funcat{\cat{C}, \cat{D}}$$, where objects are
functors from $$\cat{C}$$ to $$\cat{D}$$ and morphisms are natural
transformations. 

## Natural Isomorphisms

A natural isomorphism is an isomorphism in $$\funcat{\cat{C},
\cat{D}}$$: an invertible natural transformation. This is equivalent
to requiring that all components are isomorphisms in $$\cat{D}$$.


### On Naturality

One might easily question the "naturality" in the definition of
natural transformations: after all, it all seems pretty artificial on
first sight.
[This](https://ncatlab.org/nlab/show/natural+transformation+%28discussion%29)
discussion provides an answer: morphisms in a category $$\cat{C}$$ can
be seen as functors $$\cat{I} \to \cat{C}$$ (selecting morphisms) and
in particular, natural transformations are functors $$\cat{I} \to
\cat{D}^\cat{C}$$. Now, a pretty *natural* thing to ask is that
functor categories be [exponential objects](#exponentials) in
$$\Cat$$, in which case we get an isomorphism between functors
$$\cat{I} \to\cat{D}^\cat{C}$$ and $$\cat{I} \times \cat{C} \to
\cat{D}$$ via Currying and recover the definition of natural
transformations. In other words, the definition of natural
transformations is the one that makes $$\Cat$$ a cartesian closed
category.

# The Yoneda Lemma

The Yoneda *lemma* is a fundamental result connecting a (locally
small) category to the category of sets through its Hom-sets. As such,
it makes it possible to use results in $$\Set$$ which is very
well-known. Its main motivation is to prove the Yoneda *embedding*,
which relates the set of morphisms between two elements to the
Hom-functors of both elements:

$$\hom{C}{a, b} \simeq \hom{\funcat{C, \Set}}{\hom{C}{a, -}, \hom{C}{b, -}}$$

In particular, two elements are isomorphic whenever their Hom-functors
are naturally isomorphic *i.e.* whenever their Hom-sets are
isomorphic. In other words, knowing an object is really the same as
knowning how it releates to other objects.

## Hom Functors

Let $$\cat{C}$$ be a locally small category and $$\Set$$ be the
category of sets. Let us also fix an object $$a \in \Ob{C}$$, and
consider the two following functors:

### Covariant Hom-Functor

$$H^a = \hom{C}{a, -}$$ connects sets of morphisms starting at $$a$$
by mapping a morphism to its post-composition:

$$\begin{align}
H^a: \cat{C} &\to \Set\\
x &\mapsto \hom{C}{a, x} \\
(f: x \to y) &\mapsto H^a(f): \hom{C}{a, x} \to \hom{C}{a, y} = (g \mapsto f \circ g) \\
\end{align}$$


### Contravariant Hom-Functor

$$H_a = \hom{C}{-, a}$$ connects sets of morphisms ending at $$a$$ by
mapping a morphism to its pre-composition:

$$\begin{align}
H_a: \op{C} &\to \Set\\
x &\mapsto \hom{C}{x, a} \\
(f: y \to x) &\mapsto H_a(f): \hom{C}{x, a} \to \hom{C}{y, a} = (g \mapsto g \circ f) \\
\end{align}$$


### Representable Functors 

$$H_a$$ and $$H^a$$ are sometimes called *the* (covariant,
contravariant) representable functors, and any functor naturally
isomorphic to one of them is called *a* representable functor.

## Yoneda Embedding

One can easily check that both $$H^a$$ and $$H_a$$ are indeed
functors. Now, let us consider the Yoneda *embedding*, which is
defined as the following functor:

$$\begin{align}
H_{-}: \cat{C} &\to \funcat{\op{C}, \Set}\\
a &\mapsto H_a \\
(f: a \to b) &\mapsto H_{f}: H_a \to H_b \\
\end{align}$$

For $$H_f$$ to be a natural transformation (*i.e.* a morphism of the
functor category $$\funcat{\op{C}, \Set}$$) from $$H_a$$ to $$H_b$$,
it must have components $$\block{H_f}_x: H_a(x) \to H_b(x)$$, that is:

$$\block{H_f}_x: \hom{C}{x, a} \to \hom{C}{x, b}$$

The obvious choice given $$f: a \to b$$ is to post-compose by $$f$$:

$$\block{H_f}_x = (g \mapsto f \circ g)$$

Given a morphism $$h: y \to x$$, the naturality square for $$H_f$$ is
the following:

$$\natsq{\block{H_f}}{H_a}{H_b}{x}{y}{h}$$

In order to check that this diagram is indeed commutative, we start
from any morphism $$g: x \to a \in H_a(x)$$ and follow both sides of
the diagram:

$$\begin{matrix}
g &\overset{H_a(h)}\longmapsto & g \circ h &\overset{\block{H_f}_y}\longmapsto & f \circ (g \circ h)  \\
g &\overset{\block{H_f}_x}\longmapsto & f \circ g &\overset{H_b(h)}\longmapsto & (f \circ g) \circ h  \\
\end{matrix}$$

The associativity of morphism composition shows that the naturality
square above indeed commutes, and that the Yoneda embedding is indeed
a functor. The Yoneda *lemma* will establish that this functor is
*fully faithful*, *i.e.* that the Yoneda embedding is indeed an
embedding.

## Lemma and Consequence

Let us consider some other functor $$F: \op{C} \to \Set$$, a morphism
$$h: x \to y$$ and a natural transformation $$\alpha: H_a \to F$$ with
naturality square as follows:

$$\natsq{\alpha}{H_a}{F}{y}{x}{h}$$

In particular, let us consider the case where $$y = a$$ and $$h: x \to
a$$ is *any* element of $$H_a(x)$$ then follow the image of the
identity morphism $$\id_a \in H_a(a)$$ along both sides of the
naturality square:

$$
\begin{matrix}
	\id_a &\longmapsto& \alpha_a\block{\id_a} &\longmapsto & F(h)\block{\alpha_a\block{\id_a}} \\
	\id_a &\longmapsto& \id_a \circ h &\longmapsto & \alpha_x(h) \\
\end{matrix}
$$


Since this holds for any $$h\in H_a(x)$$, the component $$\alpha_x$$
is:

$$\alpha_x = F(-) \block{\alpha_a\block{\id_a}}$$

which is completely determined by the value of $$\alpha_a\block{\id_a}
\in F(a)$$. Again, this holds for any $$x \in \op{C}$$, meaning
$$\alpha$$ itself is completely determined by the value of
$$\alpha_a\block{\id_a}$$. Conversely, any value in $$F(a)$$ can be
used to *define* a natural transformation $$\alpha$$ by specifying the
value $$\alpha_a\block{\id_a}$$. Therefore, we obtain the following
isomorphism of sets:

$$F(a) \simeq \hom{\funcat{\op{C}, \Set}}{H_a, F}$$

which is the Yoneda lemma for the contravariant representable functor
$$H_a$$. In particular, taking $$F=H_b$$ yields:

$$H_b(a) = \hom{C}{a, b} \simeq \hom{\funcat{\op{C}, \Set}}{H_a, H_b}$$

This shows that the Yoneda embedding is fully faithful (thus indeed an
*embedding*): morphisms in $$\hom{C}{a, b}$$ are in one-to-one
correspondence with natural transformations in $$\hom{\funcat{\op{C},
\Set}}{H_a, H_b}$$. As a consequence, any locally small category
$$\cat{C}$$ can be viewed as a subcategory of *(embedded in)* the
category of contravariant functors from itself to sets (*presheaves*).

Finally, the Yoneda lemma is natural in both $$a$$ and $$F$$, meaning
that the two following squares commute:

$$
\begin{matrix}
 	F(a) & \longrightarrow & \hom{\funcat{\op{C}, \Set}}{H_a, F} \\
    \downarrow &  & \downarrow \\
 	F(b) & \longrightarrow & \hom{\funcat{\op{C}, \Set}}{H_b, F} \\ 
\end{matrix}
\quad\quad\quad
\begin{matrix}
 	F(a) & \longrightarrow & \hom{\funcat{\op{C}, \Set}}{H_a, F} \\
    \downarrow &  & \downarrow \\
 	G(a) & \longrightarrow & \hom{\funcat{\op{C}, \Set}}{H_a, G} \\ 
\end{matrix} 
$$

(TODO show this)

## Covariant Case

All of the above translates fairly directly to the covariant case,
giving:

$$F(a) \simeq \hom{\funcat{C, \Set}}{H^a, F}$$

Note that the Yoneda embedding becomes contravariant:

$$H^{-}: \op{C} \to \funcat{C, \Set}$$

### Haskell

The Yoneda lemma applied to the category of Haskell types $$\Hask$$
(where objects are types, morphisms are functions, functors are type
constructors and natural transformations are polymorphic functions)
gives the following:

- $$F a \simeq \forall x.(a \to x) \to F x$$ (covariant)
- $$F a \simeq \forall x.(x \to a) \to F x$$ (contravariant)

The covariant version essentially states that values are equivalent to
their [CPS](cps.html) transformation.

# Initial and Terminal Objects

An object $$1 \in \Ob{c}$$ is called *initial* if there exists a
unique morphism *from* this object to any object $$y \in \Ob{C}$$ in
the category:

$$1 \overset{!}\longrightarrow y$$

From this definition, one can immediately check that initial objects
(provided they exist) are *unique up to unique isomorphism*. To see
why, let us consider two initial objects $$x_1$$ and $$x_2$$. Since
both are initial, all the morphisms on the following diagram are
unique:

$$\underset{\underset{\id_{x_1}}\circlearrowright}{x_1} \overset{f}{\underset{g}{\rightleftharpoons}} \underset{\underset{\id_{x_2}}\circlearrowleft}{x_2}$$

Therefore, $$f \circ g = \id_{x_2}$$ and $$g \circ f = \id_{x_1}$$,
which provides the isomorphism. Moreover, both $$f$$ and $$g$$ are
unique, so the isomorphism is unique. Dually, a *terminal* object is
an initial object in $$\op{C}$$: an object $$0$$ is terminal if there
exists a unique morphism from every object *to* the terminal object.
 
### Examples

- in $$\Set$$, the initial object is the empty set, and terminal
  objects are the singleton sets.
- in a preorder, an initial object is a least element, and a terminal
  object is a greatest element
 
 
# Limits and Colimits

## Products

Let us consider a locally small category $$\cat{C}$$, three objects
$$a, b, a \times b \in \Ob{C}$$ and two morphisms $$\pi_1: a \times b
\to a$$ and $$\pi_2: a \times b \to b$$. The object $$a \times b$$ is
called the *product* of $$a$$ and $$b$$ if, and only if, it satisifies
the following *universal property*:

> For every object $$c \in \Ob{C}$$ with morphisms $$f: c \to a$$ and
> $$g: c \to b$$, there exists a *unique* morphism $$\inner{f, g}: c \to a \times
> b$$ such that the following diagram commutes:

$$
\begin{matrix}
      & & c &  & \\
      & \overset{f}\swarrow & \downarrow & \overset{g}\searrow & \\
    a &\underset{\pi_1}\longleftarrow& a \times b & \underset{\pi_2}\longrightarrow& b \\
\end{matrix}
$$

As with terminal objects, products (if they exist) are unique up to
unique isomorphism and the proof is similar. In particular, there is a
unique morphism $$\id_{a\times b}$$ from the product to
itself. Dually, *coproducts* are products in $$\op{C}$$:

$$
\begin{matrix}
      & & c &  & \\
      & \overset{f}\nearrow & \uparrow & \overset{g}\nwarrow & \\
    a &\underset{i_1}\longrightarrow& a + b & \underset{i_2}\longleftarrow& b \\
\end{matrix}
$$

### Examples

- in $$\Set$$, products are the usual catesian product of sets, and
  coproducts are disjoint unions
- in a preorder, products are *meets* (greatest lower bound) and
  coproducts are *joins* (lowest upper bound)


## Equalizers

## Pushouts

## Exponentials

## Cones

In every example above, we had the following ingredients:

- some diagram $$D: \cat{J} \to \cat{C}$$ representing a particular
  pattern $$\cat{J}$$ in $$\cat{C}$$,
- some object of interest $$u \in \Ob{C}$$ connected to every object
  in $$D$$,
- all triangles between connected objects of the diagram and $$u$$
  must commute.

This set of requirements can be expressed very concisely by a natural
transformation $$\alpha$$ between the constant functor: $$\Delta_u:
\cat{J} \to \cat{C}$$ and $$D$$:

$$\alpha: \Delta_u \to D$$

For every vertex $$j \in \Ob{J}$$ the components of $$\alpha$$ are:

$$\alpha_j: \Delta_u(j) = u \to D(j)$$

Therefore $$u$$ is connected to any object in $$D$$. For this reason
$$u$$ is called the *apex* of the cone. The naturality square for
$$\alpha$$ gives us precisely the commutative triangles we need:

$$\natsq{\alpha}{\Delta_u}{D}{i}{j}{f}$$

Dually, *cocones* are cones in $$\op{C}$$.

## Limits

We just saw that a cone of shape $$\cat{J}$$ and apex $$v$$ is an element of
$$\hom{\funcat{J, C}}{\Delta_v, D}$$. Coming back to the examples above, in each
case we had a special cone with the property that any cone of the same shape
factorizes though it by a *unique* morphism. In other words, every cone of this
shape is in one-to-one correspondence with the morphism used in its
factorization. This can be expressed by the following bijection:

$$\hom{C}{v, u} \simeq \hom{\funcat{J, C}}{\Delta_v, D}$$

But remember that all the examples above also require commutative
triangles for the factoring morphism. This suggests that a mere
bijection between hom-sets is not enough, and that we instead require
some kind of natural isomorphism. The bijection above relates the two
following presheaves:

$$\begin{align}
H_u: v &\mapsto \hom{C}{v, u} \\
H_D\circ\Delta: v &\mapsto \hom{\funcat{J, C}}{\Delta_v, D} \\
\end{align}$$

The first one is our trusty representable functor, which we already
encountered in the [Yoneda lemma](#the-yoneda-lemma). It is easy to
check that the second one is also a functor: $$\Delta: \cat{C} \to
\funcat{J, C}$$ is a covariant functor obtained by post-composing
morphisms, then we apply the representable functor in $$\funcat{J,
C}$$. A natural isomorphism between $$H_u$$ and $$H_D\circ\Delta$$
has the following naturality square:

$$\natism{\alpha}{H_u}{H_D\circ\Delta}{v}{w}{f}$$

where $$H_u(f) = (-) \circ f$$ and $$\block{H_D \circ \Delta}(f) =
H_D\block{\Delta_f} = (-) \circ \Delta_f$$. As for the Yoneda lemma,
let us follow the images of $$\id_u$$, given some morphism $$f: v \to
u$$:

$$\begin{matrix}
\id_u &\longmapsto& f &\longmapsto& \alpha_v(f)\\
\id_u &\longmapsto& \alpha_u\block{\id_u} & \longmapsto & \alpha_u\block{\id_u} \circ \Delta_f\\
\end{matrix}$$

Both right-hand sides refer to the same cone *i.e.* natural
transformation $$\Delta_v \to D$$, so the components must match:

$$\begin{align}
\block{\alpha_v(f)}_i &= \block{\alpha_u\block{\id_u} \circ \Delta_f}_i \\
&= \block{\alpha_u\block{\id_u}_i} \circ \block{\Delta_f}_i \\
\end{align}$$

Now, it is easily checked that $$\block{\Delta_f}_i$$ is simply $$f$$ in
disguise. $$\alpha_v(f)$$ can be any cone with vertex $$v$$, so its $$i$$-th
component is a morphism $$\block{\alpha_v(f)}_i: v \to D(i)$$. Finally,
$$\alpha_u\block{\id_u}$$ is our special cone, and its $$i$$-th component is a
morphism: $$\block{\alpha_u\block{\id_u}_i}: u \to D(i)$$. In other words, we
have the following commutative triangle:

$$\comtri{v}{u}{D(i)}{f}{\block{\alpha_u\block{\id_u}_i}}{\block{\alpha_v(f)}_i}$$

which are exactly the commutativity conditions we're looking for, leading to the
following definition:

> A *limit* for diagram $$D: \cat{J} \to \cat{C}$$ is an object $$u \in \Ob{C}$$
together with a natural isomorphism $$\hom{C}{v, u} \simeq \hom{\funcat{J,
C}}{\Delta_v, D}$$ natural in $$v$$. The *universal cone* for the limit is the
one corresponding to $$\id_u$$.

In a nutshell:
 
- for every cone $$\Delta_v \to D$$ we get a *unique* morphism $$f: v \to u$$
  that factors any morphism $$h: v \to D(i)$$ through $$u$$ as $$h = g \circ
  f$$, where $$g: u \to D(i)$$
  
- conversely, for every morphism $$f: v \to u$$ there is a cone $$\Delta_v \to
  D$$ factored as above.
  
Equivalently, we can use this property to define morphisms of cones,
leading to a cone category, in which the universal cones are the
terminal objects. Dually, colimits are limits in $$\op{C}$$.

## Examples

- Terminal(initial) objects are (co)limits on the empty diagram $$\cat{0} \to \cat{C}$$
- (co)products are (co)limits on the two object diagram $$\cat{2} \to \cat{C}$$


# Adjunctions

# Monads






# Notes & References

[^1]:
    [classes](https://en.wikipedia.org/wiki/Von_Neumann%E2%80%93Bernays%E2%80%93G%C3%B6del_set_theory) are
    a syntactic restriction to avoid logical paradoxes arising when
    considering *e.g.* the set of all sets not containing themselves
    (Russel's paradox)
