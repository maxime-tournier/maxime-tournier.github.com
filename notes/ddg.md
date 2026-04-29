---
title: Discrete Differential Geometry
categories: [math]
tags: [draft]
---

{% include toc.md %}

Discrete Differential Geometry (DDG) provides a set of tools for integrating
stuff consistently over discrete manifolds, usually 3-dimensional
meshes. Unfortunately, the sheer amount of knowledge required to *properly*
understand what's going on is absolutely daunting:

- one first needs a firm grasp of the theory of differential forms in the
  continuous setting, which extends tools from exterior algebra in much the same
  way differential geometry extends linear algebra (pointwise in the
  tangent/cotangent spaces).
- Riemannian geometry is then required for proper integration on manifolds and
  the metric structure on differential forms
- at this point, a whole zoo of differential operators enters the scene (exterior
  derivative, Hodge star, interior product, Lie derivative) with associated
  theorems (Stoke's thorem, Cartan formula)
- and *only then* one is finally ready to discretize the continuous concepts,
  with two main lines of work Finite Element Methods (FEM) and Discrete Exterior
  Calculus (DEC) which are both still active field of research.

So this is going to take *some* time. We'll assume the basics of linear algebra,
Euclidean (basis, dual basis, linear forms, metrics, inner products and
representation theorems, orthogonal endomorphisms) and differential/Riemannian
geometry (tangent/cotangent bundles, tangent maps, directional
derivatives, metric) to be known.

We'll start with exterior algebra, which is already quite a beast in
itself. Then, on to integration theory and the metric structure on differential
forms, followed by differential operators and Stoke's theorem. Finally, we'll
cover the approaches to discretization.

# Exterior Algebra

Integrating differential forms is fundamentally about summing up the weightings
of infinitesimal volumes over a domain. As we'll see, such weighting can be
formalized as an alternating form. It turns out that alternating forms have a
particularly rich algebraic structure, which is what *exterior algebra* is
about. The usual treatments of the subject vary between the coordinate-ful
approach, riddled with indices and painful change of coordinates, and the
coordinate-free one, arguably more elegant but often abstracted to the point of
uselessness. We'll try to strike a balance between the two, keeping the model of
alternating linear forms in mind but underlying the general construction as
well.

## Measuring Volumes

Integration theory is essentially about decomposing a domain into
infinitely many *infinitesimal volumes*, weighting each volume and
summing up the results. Assuming we're dealing with a vector space
$$V$$ of dimension $$n$$, those infinitesimal volumes can be thought
of as $$n$$-parallelotopes, which can be represented as a set of $$n$$
vectors describing the edges of the parallelotope. Weighting such a
parallelotope is given by a function $$\omega$$ taking $$n$$ vectors
to a scalar:

$$\omega: V^n \to \RR$$

Since we wish to weight *volumes*, we expect such a function to be
somewhat $$n$$-linear since scaling the length of an edge ends up
scaling the volume accordingly. Likewise, repeating an edge collapses
the parallelotope and should end up in a zero volume. Taken together,
these conditions suggest that $$\omega$$ should be an *alternating*
$$n$$-linear form, which is what exterior algebra is about.

## Alternating forms

Let us start by fixing an $$n$$-dimensional vector space $$V$$, and
consider alternating linear forms on $$V$$. We start with 1-linear
forms *i.e.* linear forms, which cannot be alternating in any
interesting way since they only have one argument. The first
non-trivial case is that of *bililinear* alternating forms: let
$$\omega: V^2 \to \RR$$ be such a form, the alternating condition
requires that:

$$\omega(x, x) = 0$$

for all $$x \in V$$ which by bilinearity implies that:

$$\begin{aligned}
\underbrace{\omega(x + y, x + y)}_0 &= \underbrace{\omega(x, x)}_0 +
\omega(x, y) + \omega(y, x) + \underbrace{\omega(y, y)}_0 \\
0 &= \omega(x, y) + \omega(y, x) \\
\end{aligned}$$

hence $$\omega(x, y) = -\omega(y, y)$$ and the name "alternating". $$k$$-linear
alternating forms behave similarly:

$$\omega(\ldots, x, \ldots, x, \ldots) = 0$$

leading to:

$$\omega(\ldots, x, \ldots, y, \ldots) = -\omega(\ldots, y, \ldots, x, \ldots)$$ 

as well. One may immediately remark that one cannot construct a non-zero
$$k$$-linear forms when $$k > n$$: by choosing a basis for $$V$$ and expanding
each argument using multi-linearity, we would always end up with at least one
repeated basis vector in the arguments, forcing the form to zero. We can also
see that permuting the arguments flips the sign of the result by the sign of the
permutation:

$$\omega\block{x_{\sigma(1)}, \ldots, x_{\sigma(k)}} = (-1)^{\sign{\sigma}} \omega\block{x_1, \ldots, x_k}$$

for $$\sigma \in S(k)$$. Obviously, forms of a same degree can be
added together and multiplied by a scalar to yield same-degree forms,
turning alternating forms of a given degree $$k$$ into a vector space,
$$A^k(V)$$. Of course, once we have a vector space it is natural to
ask for a basis. The basis for 1-forms is given by the usual dual
basis, but what about higher-degree forms? Again, using
multi-linearity, one can easily see that a $$k$$-form is entirely
determined by its value on subsets of $$k$$ distinct basis vectors
modulo permutations. This suggests that the dimension of the space of
$$k$$-linear alternating forms is:

$$\dim\block{A^k(V)} = \mat{n \\ k}$$ 

and that a basis can be constructed by associating a $$k$$-form to
each of these subsets such that the $$k$$-form evaluates to $$1$$ on
its associated subset, and $$0$$ on every other. Such basis
$$k$$-forms must involve basis $$1$$-forms somehow, so one should
expect that basis forms should be constructible from basis
$$1$$-forms, which is exactly the purpose of the *wedge product*.

## Wedge Product

Now, the *algebra* in "exterior algebra" is about constructing alternating forms
from lower-degree ones, using an operation called the *wedge product*, or
*exterior* product. Let us try to construct a $$2$$-form from a pair of
$$1$$-forms: given $$\omega_1, \omega_2 \in A^1(V)$$, we want to construct some
$$2$$-form out of $$\omega_1, \omega_2$$ and inputs $$x_1, x_2$$. There is not
much we can do with $$\omega_1, \omega_2$$ apart from applying them to each
input to obtain two pairs of scalars, and mix these bilinearly in an alternating
way. Luckily, there exists essentially one (modulo scaling) $$2\times2$$
antisymmetric bilinear form that we can use to mix the scalars, so we don't have
much choice:

$$\mat{\omega_1\block{x_1} & \omega_1\block{x_2}}\mat{0 & 1 \\ -1 & 0}\mat{\omega_2\block{x_1} \\\omega_2\block{x_2}} = \omega_1\block{x_1}\omega_2\block{x_2} - \omega_1\block{x_2}\omega_2\block{x_1}$$

Let us now try to build a $$3$$-form from a pair of a $$1$$-form and a
$$2$$-form: again, there's not much we could do except multiply the result of
applying the $$1$$-form to one argument vector, the $$2$$-form to the remaining
ones, and somehow anti-symmetrize the result:

$$\begin{aligned}
    \pm \alpha\block{x_1}\beta\block{x_2, x_3} \pm \alpha\block{x_2}\beta\block{x_3, x_1} \pm \alpha\block{x_3}\beta\block{x_1, x_2}\\
    \pm \alpha\block{x_1}\beta\block{x_3, x_2} \pm \alpha\block{x_2}\beta\block{x_1, x_3} \pm \alpha\block{x_3}\beta\block{x_2, x_1}\\
\end{aligned}
$$

But how do we pick the signs? Notice that the top row only contains even
permutations of the inputs, while the bottom row contains the remaining odd
permutations. Applying an even permutation to the inputs leaves both rows
unchanged (even though individual terms are permuted *inside* each row) and
should not change the sign: therefore the signs should be constant inside each
row. Likewise, applying an odd permutation to the inputs with end up swapping
the top and bottom rows. Since this operation should produce a sign change, we
should assign opposite signs to the top and bottom rows. Again, apart from some
global scaling parameter there's essentially no other choice we could have made.

This precedure can be generalized to higher-degree forms fairly directly, and
motivates the definition of the wedge product of a $$p$$-form $$\omega_p$$ with
a $$q$$-form $$\omega_q$$ as follows:

$$
\omega_p \wedge \omega_q = \frac{1}{k!}\sum_{\sigma \in S(k)} \sign{\sigma} \omega_p\block{x_{\sigma(1)}, \ldots, x_{\sigma(p)}}
\omega_q\block{x_{\sigma(p+1)}, \ldots, x_{\sigma(p + q)}}
$$

where the $$\frac{1}{k!}$$ normalization factor ensures proper associativity of
the wedge product. One can show that:

$$\omega_p \wedge \omega_q = \block{-1}^{pq} \omega_q \wedge \omega_p$$

for a $$p$$-form $$\omega_p$$ and $$q$$-form $$\omega_q$$. In particular: 

$$x \wedge x = \block{-1}^{k^2} x\wedge x$$ 

for any $$k$$-form $$x$$. Therefore, when $$k$$ is odd $$\block{(-1)^k}^k =
-1$$ and $$x \wedge x = 0$$, which is the case in particular when $$k =
1$$. Otherwise, $$k$$ is even $$\block{(-1)^k}^k = 1$$ and there's no reason for
$$x \wedge x$$ to vanish. By the same token:

$$x \wedge y = \block{-1}^{k^2} y \wedge x$$

for same-degree forms $$x, y$$, so when $$k$$ is odd we get $$x\wedge
y = -y \wedge x$$. $$k$$-forms obtained as the wedge product of $$k$$
vectors are conveniently named $$k$$-vectors. By a dimension argument,
one can show that the $$k$$-vectors obtained from a basis of $$V$$
span the space of alternating $$k$$-forms on $$V$$, thereby providing
a basis for it. One should be cautious though: some $$k$$-forms cannot
be expressed as a single $$k$$-vector.

## Algebra Structure

Now that we've constructed a whole family of wedge products:

$$\wedge^{p, q}: A^p(V) \times A^q(V) \to A^{p+q}(V)$$

we can "bundle together" all the individual vector spaces $$A^i(V)$$ for $$0
\leq i \leq n$$ into a single direct sum:

$$A(V) = \bigoplus_{i=0}^{i=n} A^i(V)$$

with the convention that $$A^0(V) = \RR$$. As we saw earlier, forms of degree
strictly larger than $$n$$ are all zero, so there's no point in including
them. It is easy to see that this large vector space is of dimension:

$$\dim\block{A(V)} = \sum_k \mat{n \\k} = 2^n$$

In this new vector space, we may define a single *internal* wedge product as:

$$\wedge: A(V) \to A(V)$$

which selects and applies the appropriate $$\wedge^{p, q}$$ based on the degree
of its arguments. This product gives our direct sum the structure of an
*algebra*, called the *alternating algebra* of $$V$$. This algebra comes in
"layers" given by the degree of its forms, and since the layers are well-behaved
$$A^p \wedge A^q \subseteq A^{p+q}$$ we call it a *graded* algebra.


## Abstract Construction

*(note: this section can be safely skipped for all practical purposes)*

We were promised the *exterior algebra*, but all we got was this lousy
*alternating algebra*, what gives? It turns out that the two are isomorphic, and
that everything we obtained from alternating forms (the graded algebra
structure) can be constructed purely abstractly from $$V$$ alone. The
construction is a bit abstract and can be puzzling at first, but it is a good
exercice to understand it as it is both powerful and easily generalizable.

The first step is to construct the *tensor algebra* out of $$V$$,
which is an abstraction of multi-linear forms together with the tensor
product. Again, we want to do so in a purely abstract way *i.e.*
without ever using *actual* multi-linear forms: we want to construct
stuff with *just enough structure* to behave like multilinear maps,
without actually being multilinear maps. The reason to do it this way
is that by constructing the most general possible algebra that acts
like tensors, we get factorization theorems for free. These theorems
say that anything tensor-like happening in concrete realizations must
have a well-behaved counterpart in the abstract tensor algebra, where
things can be proven once and for all.

Given two vector spaces $$V, W$$ of finite dimensions, we start by constructing
the *free vector space* of pairs of elements of $$V, W$$:

$$F(V, W) \ = \bigoplus_{x \in V, y \in W} (x, y)$$

This vector space is *enormous*: every possible pair of elements of
$$V, W$$ gives rise to a 1-dimensional subspace of $$F(V, W)$$ where
one keeps track of the scaling associated to this particular pair of
elements. In particular, in this space $$(\lambda x, y) \neq \lambda
(x, y)$$, which is somewhat unfortunate. One way to fix this and to
make this space smaller is to *quotient* it by linear subspaces. In
particular, the bilinearity relations we expect from the tensor
product can be expressed as linear subspaces. For instance, elements
of the form

$$\block{x_1 + x_2, y} - \block{x_1, y} - \block{x_2, y}$$

span linear subspaces, and quotienting $$F(V, W)$$ by these subspaces will enforce

$$\block{x_1 + x_2, y} - \block{x_1, y} - \block{x_2, y} = 0$$

in the quotient space, providing the linearity of the tensor product with
respect to the first argument. We may proceed similarly for the other following
elements:

$$\block{x, y_1 + y_2} - \block{x, y_1} - \block{x, y_2}$$

$$\block{\lambda x, y} - \lambda \block{x, y}$$

$$\block{x, \lambda y} - \lambda \block{x, y}$$

Letting $$R$$ be the subspace spanned by all these elements, we obtain the
*tensor space* $$V \otimes W$$ of $$V$$ and $$W$$ as the following quotient
vector space:

$$V \otimes W = F(V, W) / R$$

Elements of $$V \otimes W$$ are usually denoted by $$x\otimes y$$ instead of
pairs. One can show that the tensor product of vector spaces is associative (in
the sense that the resulting tensor products are naturally isomorphic). The kind
of factorization theorem (called "universal property") that comes "for free" is
the following: any bilinear mapping $$h: V \times W \to Z$$ factors *uniquely*
through $$V \otimes W$$ as $$h = \tilde{h} \circ \varphi$$ where $$\phi$$ is the
canonical projection $$\varphi: V \times W \to V \otimes W$$ onto the quotient
space, and $$\tilde{h}$$ is an algebra homomorphism. The *tensor algebra*
$$T(V)$$ is obtained by iterating the tensor product into a single direct sum:

$$T(V) = \RR \oplus V \oplus \block{V \otimes V} \oplus \block{V \otimes V \otimes V} \oplus \ldots$$

It is again a graded algebra. What about exterior algebra then? We can further
quotient $$T(V)$$ by the subspace generated by

$$x \otimes x$$

for all $$x \in V$$, enforcing $$x \wedge x = 0$$ for vectors. We end up with
the *exterior algebra*:

$$\bigwedge(V) = \sum_{k = 0}^{k=n}\bigwedge^k(V)$$

with the exterior product of $$x, y$$ given by the canonical projection:

$$x \wedge y: V^p \times V^q \to \bigwedge^{p + q}(V)$$

Again, any $$k$$-linear alternating map $$h: V^k \to Z$$ factors uniquely
through $$\bigwedge^k(V)$$ as $$h = \tilde{h} \circ \varphi$$, where $$\varphi:
V^k \to \bigwedge^k(V)$$ is the canonical projection and $$\tilde{h}$$ is an
algebra homomorphism.

## Interior Product

So far, we only have a way of constructing *higher-degree* forms using the wedge
product. What about decreasing the degree? A simple way to achieve this is
with the partial application of some $$k$$-form to a given input vector: let
$$\omega$$ be a $$p+1$$-form and $$x \in V$$ be a vector, we obtain a
$$p$$-form by feeding $$x$$ to $$\omega$$ as its first argument:

$$\block{\block{y_1, \ldots, y_p} \mapsto \omega\block{x, y_1, \ldots, y_p}} \in \bigwedge^p(V)$$

We call such a $$p$$-form the *interior product* of $$\omega$$ and $$v$$,
denoted by $$\iota_x \omega$$, where

$$\iota_v: \bigwedge^{p+1}(V) \to \bigwedge^{p}(V)$$

is the interior product. Of course, the interior product is linear in both
arguments, and since it applies to alternating forms, we get:

$$\iota_x \circ \iota_y = -\iota_y \circ \iota_x$$

In particular:

$$\iota_x \circ \iota_x = 0$$

## Inner Product

When constructing the inner product of differential forms later on, we'll need
to extend an inner product on $$V$$ to the whole exterior algebra
$$\bigwedge(V)$$. As with the wedge product, let us try to construct a
reasonable inner product for a pair of $$2$$-forms (say, decomposable), given an
existing one on $$1$$-forms. That is, we want to compute:

$$\inner{a \wedge b, c \wedge d}$$

Since we already have an inner product on $$1$$-forms, there's not much to do
apart from using it and somehow mix the results so that it behaves like a *bona
fide* inner product. More precisely, from $$\inner{a, c}, \inner{a, d},
\inner{b, c}, \inner{b, d}$$, we can construct two bilinearly varying terms
$$\inner{a, c}\inner{b, d}, \inner{a, d}\inner{b, c}$$ that we should mix
together somehow:

$$\pm\inner{a, c}\inner{b, d} \pm \inner{a, d}\inner{b, c}$$

Swapping $$\block{a, b}$$ or $$\block{c, d}$$ exchanges the two terms above, and
should also negate the result: therefore each term should get an opposite sign:

$$\pm \block{\inner{a, c}\inner{b, d} - \inner{a, d}\inner{b, c}}$$

Which sign to use? The inner product $$\inner{a \wedge b, a \wedge b}$$ should
be positive:

$$\inner{a \wedge b, a \wedge b} = \pm \underbrace{\block{\norm{a}^2 \norm{b}^2 - \inner{a, b}^2}}_{\geq 0} \geq 0$$

by Cauchy-Schwarz, so plus it is:

$$\inner{a \wedge b, c \wedge d} = \inner{a, c}\inner{b, d} - \inner{a, d}\inner{b, c}$$

(modulo positive scaling, of course). We verify that exchanging $$a
\wedge b$$ with $$c \wedge d$$ does indeed not change the result,
making this bilinear form symmetric and positive, but we should also
check that it is definite: the Cauchy-Schwarz inequality above becomes
an equality exactly when $$a$$ and $$b$$ are colinear, in which case
$$a \wedge b = 0$$. Interestingly, the above expression can be
factored as

$$\begin{aligned}
\inner{a \wedge b, c \wedge d} &= \inner{b, \inner{a, c} d - \inner{a, d} c} \\
&= \inner{b, c\block{a^\sharp}d - d\block{a^\sharp}c} \\
&= \inner{b, \block{c \wedge d}\block{a^\sharp, \cdot}} \\
&= \inner{b, \iota_{a^\sharp}\block{c \wedge d}} \\
\end{aligned}$$

where the $$1$$-form $$a = \inner{a^\sharp, \cdot}$$ is represented by
vector $$a^\sharp$$. Notice how the interior product with $$a^\sharp$$
pushes inner products involving $$a$$ down the right-hand side
$$2$$-form $$c \wedge d$$. What is more, $$c \wedge d$$ is passed
directly to the interior product so the formula extends directly to a
non-decomposable right-hand side by linearity:

$$\inner{a \wedge b, \omega} = \inner{b, \iota_{a^\sharp}\omega}$$

and proceed similarly for a non-decomposable left-hand side by
linearity. It is not too hard to check that what we obtain is indeed
still a *symmetric* bilinear form (expand everything down to
decomposable forms, on which the inner product is symmetric, then
recompose), but positive definiteness requires a bit more
scrutiny. Let us consider a non-decomposable $$2$$-form $$\omega = a
\wedge b + c \wedge d$$. Then we get:

$$\begin{aligned}
\norm{w}^2 &= \norm{a \wedge b}^2 + \norm{c \wedge d}^2 + 2 \underbrace{\inner{a \wedge b, c \wedge d}}_{\geq -\norm{a \wedge b}\norm{c \wedge d}} \\
& \geq \norm{a \wedge b}^2 + \norm{c \wedge d}^2 - 2 \norm{a \wedge b}\norm{c \wedge d} \\
&= \block{\norm{a \wedge b} - \norm{c \wedge d}}^2 \\
&\geq 0
\end{aligned}$$

thanks to Cauchy-Schwarz once again, with equality exactly when $$a
\wedge b = \lambda \block{c \wedge d}$$. Since $$\omega$$ is
non-decomposable, this leaves $$\lambda = 0$$ as the only
possibility. The argument extends easily to *any* non-decomposable
$$2$$-form, and we end up with an inner product on all $$2$$-forms.

Now that we have an inner product on $$2$$-forms, we may use the exact
same procedure to build one for $$3$$-forms. Let us consider a
$$2$$-form $$\eta$$, a $$1$$-form $$\omega$$ and a $$3$$-form $$\nu$$,
we construct an inner product satifying:

$$\inner{\eta \wedge \omega, \nu} = \inner{\eta, \iota_{\omega^\sharp} \nu}$$

and extend it to non-decomposable $$3$$-forms as before, and repeat
the process until we obtain an inner-product on every
$$\bigwedge^k(V)$$. An inner product on the whole exterior algebra
$$\bigwedge(V)$$ can be obtained as the direct sum of the inner
products for each degree *i.e.* by having $$\inner{x, y} = 0$$ for
different degree forms $$x, y$$.

## Hodge Duality

As we saw above, $$\dim\block{A^n(V)} = 1$$ so all non-zero top-degree forms are
scalar multiples of each other. These are called *volume forms*, and choosing a
preferred volume form $$\omega$$ (*e.g.* $$\dd x_1 \wedge \ldots \dd x_n$$ for a
dual basis $$\dd x_1, \ldots, \dd x_n$$) gives a correspondance between
$$k$$-forms and $$n - k$$ forms via the interior product with $$\omega$$ as
follows:

$$
\dd x_{\sigma(1)} \wedge \ldots \wedge \dd x_{\sigma(k)} \mapsto \underbrace{\block{\block{y_1, \ldots, y_{n - k}} \mapsto \omega\block{x_{\sigma(1)}, \ldots, x_{\sigma(k)}, y_1, \ldots, y_{n-k}}}}_{\iota_{x_{\sigma(k)}} \circ \ldots \circ \iota_{x_{\sigma(1)}} \omega}
$$

for all $$\sigma \in S(k)$$ where we plug the primal basis vectors corresponding
to basis $$1$$-forms into $$\omega$$ in the order they appear in the input
$$k$$-vectors, extended linearly. This correspondance is not *natural* in the
sense that it depends on the choice of basis both for the volume form and for
the identification of $$V$$ with its dual. However, when $$V$$ has an
inner-product the primal/dual identification through it becomes natural (it no
longer depends on a particular choice of basis). Furthermore, there is also a
natural choice of a volume form that no longer depends on the basis. In order do
find it, we first need to ask ourselves *how does a volume form changes as we
change basis?* 

To answer this question, let us consider the *pullback* of a volume form by an
endomorphism $$A: V \to V$$:

$$A^*\omega: \block{x_1, \ldots x_n} \mapsto \omega\block{Ax_1, \ldots, Ax_n}$$

where we transform every input vector by $$A$$ before feeding them to
$$\omega$$. The result is another $$n$$-form, which are a $$1$$-dimensional
subspace so there should be some multiplicative constant $$\lambda_A$$ such that
$$A^*\omega = \lambda_A \omega$$. In fact, one can easily see that the result is
a volume form (that is, $$\lambda_A \neq 0$$) exactly when $$A$$ is
non-singular. Obviously, $$\lambda_I = 1$$ and $$\lambda_{AB} = \lambda_{BA} =
\lambda_A \lambda_B$$, therefore $$\lambda$$ is a group homomorphism $$GL(n) \to
\RR^\times$$, which means that $$\lambda$$ must factor through the determinant
$$\det$$. One can show that the scaling factor actually *is* the determinant:

$$A^*\omega = \det(A)\omega$$

How does this help us with choosing a basis-independent volume form? Well, the
above formula implies that *any* orientation-preserving orthonormal basis will
preserve volume forms, since any two such basis are related by rotations, hence
$$\det(A) = 1$$[^orthonormal-determinant]. This makes sense intuitively:
rotating $$n$$-parallelotopes should not change volumes. So, we can simply start
from *any* orientation-preserving basis, scale the canonical $$n$$-form so that
it evaluates to $$1$$ on the orthonormalized basis *and we'll aways end up with
the exact same volume form*.

More precisely, let us pick some arbitrary dual basis $$\dd x_1, \ldots, \dd
x_n$$. In the primal basis, an orthonormal basis matrix $$B$$ satisfies $$B^TMB
= I$$ where $$M$$ is the Gram matrix of the inner product, which is
positive-definite. Therefore, $$B$$ is of the form $$B = L^{-T}U$$ where $$M =
LL^T$$ is the [Cholesky](cholesky) decomposition of $$M$$ and $$U\in
SO(n)$$. The canonical $$n$$-form $$\dd x_1 \wedge \ldots \wedge \dd x_n$$
evaluated on $$B$$ yields:

$$\begin{aligned}
\block{\dd x_1 \wedge \ldots \wedge \dd x_n}\block{Bx_1, \ldots, Bx_n} &=
\det(B)\underbrace{\block{\dd x_1 \wedge \ldots \wedge \dd x_n}\block{x_1, \ldots, x_n}}_1 \\
&= \det(B)
\end{aligned}$$

Therefore, the scaling factor for $$B$$ to evaluate to $$1$$ should be
$$\frac{1}{\det(B)} = \sqrt{\det{M}}$$, and the natural volume form is given by:

$$\sqrt{\det{M}} \dd x_1\wedge \ldots \wedge \dd x_n$$



# Differential Forms

Differential forms are the stuff that show up under the integral sign:

$$\int_\Omega \omega$$

In the above, $$\omega$$ is a differential form whose dimension
matches that of the integration domain $$\Omega$$. Its purpose is to
"eat" infinitesimal volumes that compose $$\Omega$$ in order to
produce a scalar, one per infinitesimal volume. Conceptually, these
(infinitely many) scalars are then summed up to compute the
integral. The differential form thus encodes the varying *weighting*
associated to each infinitesimal volume.

Therefore, a differential form can be seen as a function that
associates an alternating $$n$$-linear map $$\omega(x) \in
A^n\block{T_x\Omega}$$ (in the tangent space at this point) to every
point of the domain $$x$$. For instance, the usual differential of a
scalar function is a differential 1-form: it associates to every point
of the domain a linear form (the differential at that point, which is
trivially alternating) and can be integrated along curves. By
convention, 0-forms are scalar functions of the domain.

Of course, all the [exterior algebra](#exterior-algebra) machinery we
already constructed translates point-wise to differential forms, so we
may consider the *wedge product* of forms:

$$\block{\omega \wedge \eta}(x) = \omega(x) \wedge \eta(x)$$

as well as the *interior product* of a vector field $$X$$ and a
differential form $$\omega$$:

$$\block{\iota_X \omega}(x) = \iota_{X(x)} \omega(x)$$

and so on. An interesting property of $$n$$-linear alternating maps
that will come handy later on is that their pullback by an
endomorphism $$A$$ satisfies:

$$A^*\omega\block{x_1, \ldots, x_n} = \omega\block{Ax_1, \ldots, Ax_n} =
\det(A)\omega\block{x_1, \ldots, x_n}$$

In fact, this property can even serve as a *definition* of the determinant. This
implies that $$n$$-forms are rotation-invariant as one would expect: rotating
infinitesimal volumes should not change their weight.

Finally, since $$n$$-linear alternating maps form a 1-dimensional vector space,
we may choose some non-degenerate differential $$n$$-form $$\mu$$ as a reference
and obtain any other $$n$$-form as $$\omega = f \mu$$ where $$f$$ is a scalar
function that provides the pointwise scaling factor. Such reference $$n$$-forms
are usually called *volume forms*.


## Volume forms

*Orientable* manifolds are defined as the ones for which such a volume
form exists. When the manifold further comes with a Riemannian metric,
there is a natural choice of a volume form: it is chosen such that any
orientation-preserving orthonormal parallelotope (as per the metric)
is weighted to $$1$$[^orthogonal-invariance]. More precisely, any
$$M$$-orthonormal basis $$B$$ satisfies:

$$B^T M B = I$$

where $$M$$ is the inner product matrix. Since the inner-product is
non-degenerate, a [Cholesky decomposition](cholesky) $$M=LL^T$$ can be used to
show that an orientation-preserving orthonormal basis must decompose as
$$B=L^{-T}Q$$ where $$Q \in SO(n)$$ is a rotation. Let $$\dd x^1, \ldots, \dd
x^n$$ be local coordinates, the canonical $$n$$-form $$\dd x^1\wedge \ldots
\wedge \dd x^n$$ satisfies

$$\begin{aligned}
B^*\block{\dd x^1\wedge \ldots \wedge \dd x^n} &= \det(B)\dd x^1\wedge \ldots \wedge \dd x^n \\
&= \sqrt{\det(M)}\dd x^1\wedge \ldots \wedge \dd x^n
\end{aligned}
$$

Trivially, $$B^*\block{\dd x^1\wedge \ldots \wedge \dd x^n} = \dd Bx^1 \wedge
\ldots \wedge \dd Bx^n$$ will weight the parallelotope associated to basis $$B$$
to $$1$$ and therefore, the Riemannian volume form in local coordinates is given by:

$$\sqrt{\det(g)}\dd x^1\wedge \ldots \wedge \dd x^n$$

where $$g$$ is the Riemannian metric.

## Metric

On Riemannian manifolds, differential 1-forms can be identified to vector fields
using the metric. This provides the so-called *musical isomorphisms*:

$$X^\flat = g(X, \cdot)$$

where $$g$$ is the Riemannian metric. Here $$X^\flat$$ is a 1-form obtained from
vector field $$X$$ by considering the (pointwise) inner-product with $$X$$ using
the metric $$g$$. Likewise, a 1-form $$\omega$$ can be (pointwise) represented
by the inner product with some tangent vector $$\omega^\sharp$$, producing a
vector field. This in turns provides an inner-product on differential 1-forms,
obtained by integrating the inner product of their representing vector fields
over the manifold:

$$\inner{\inner{\omega_1, \omega_2}} = \int_\Omega \inner{\omega_1^\sharp, \omega_2^\sharp} \mu$$

where $$\mu$$ is the Riemannian volume form. The metric on $$1$$-forms
is extended to arbitrary-degree forms on every tangent space using the
procedure described in [exterior algebra](#inner-product), which we
integrate over the manifold as above to obtain an inner-product on
differential forms. 

## Hodge Star




# Exterior Derivative & Stoke's Theorem


# Discrete Manifolds

- simplicial complexes
- 2D manifolds: half-edge data structures (HDS)
- in general: combinatorial maps

# Discrete Differential Forms & Exterior Derivative

# Metrics on discrete forms

## 0-forms

We want the metric on discrete 0-forms (sampled functions at vertices) to mimic
the $$L^2$$ inner product:

$$\inner{\hat{u},\hat{v}} \approx \int_\Omega u(x) v(v) \dd x$$

A first consequence is that the squared norm of the indicator function should
match the volume/area of the domain:

$$\inner{\hat{1},\hat{1}} = |\Omega|$$

If we require the metric to be diagonal, there's not much we can do apart from
partitioning the domain into subdomains $$\Omega_i$$ (which could be barycentric
or Voronoi-based), one for each vertex $$i$$. We end up with the following
metric:

$$M_0 = \diag\block{\left|\Omega_i\right|}$$ 

where 

$$\sum_i \left|\Omega_i\right| = \left|\Omega\right|$$
 

### TODO whitney basis/galerkin mass

## 2-forms

A discrete 2-form is the integral of some actual 2-form over a 2-cell
(triangle). Let $$\hat{\omega}_i = e_i$$ be a piecewise constant 2-form whose
integral over the i-th triangle $$T_i$$ is $$1$$ and $$0$$ over other
triangles. That is, $$\omega_i$$ is a 2-form whose discrete version is the
$$i$$-th basis vector $$e_i$$. Since $$\omega_i$$ is top-dimensional, it can be
written as $$\omega_i = f_i {1}_{T_i} \dd A$$ where $$\dd A$$ is the area form,
and $$f_i$$ is such that $$\int_{T_i} f_i \dd A = 1$$. Therefore:

$$f_i =\frac{1}{\left|T_i\right|}$$ 

Again, the metric on discrete 2-forms should mimic the $$L^2$$ inner product on
2-forms, so we get the following diagonal elements:

$$e_i^T M_2 e_i = \int_{T_i} f_i^2 \dd A = \frac{1}{\left|T_i\right|}$$

If we further require the metric to be diagonal, we're done:

$$M_2 = \diag\block{\frac{1}{\left|T_i\right|}}$$

## 1-forms

Again, a discrete 1-form is the integral of some actual 1-form over a 1-cell
(edge). 

Before even thinking of transporting the $$L^2$$ inner product from
1-forms to discrete forms, we need some correspondance between 1-form on the
mesh and basis discrete 1-forms, which is not entirely trivial. 

### Whitney forms

More precisely, we want to construct some 1-forms $$\omega_{ij}$$ such that
$$\hat{\omega}_{ij} = e_{ij}$$, that is:

$$\begin{aligned}
\int_{ij} \omega_{ij} &= 1 \\
\int_{e \neq ij} \omega_{ij} &= 0\\
\end{aligned}
$$

If we consider Whitney basis functions $$\phi_i, \phi_j$$, we see that by
Stokes' theorem:

$$\begin{aligned}
\int_{e} \dd\block{\phi_i \phi_j} &= \int_{\partial e} \phi_i \phi_j = 0 \\
&= \int_{e} \phi_j \dd \phi_i + \phi_i \dd \phi_j \\
\end{aligned}
$$

for any edge $$e$$. Also, on edge $$ij$$ we have $$\phi_i + \phi_j = 1$$,
therefore $$\dd \phi_i = -\dd \phi_j$$, so instead of

$$\int_{ij} \phi_j \dd \phi_i + \phi_i \dd \phi_j = \int_{ij} \underbrace{\block{\phi_j - \phi_i}}_0 \dd \phi_i = 0$$ 

cancelling out as above, we could instead do:

$$\int_{ij} \phi_j \dd \phi_i - \phi_i \dd \phi_j = \int_{ij} \underbrace{\block{\phi_j + \phi_i}}_1 \dd \phi_i = \int_{\partial ij} \phi_i = 1 - 0 = 1$$ 

So the 1-form $$\phi_{ij} = \phi_j \dd \phi_i - \phi_i \dd \phi_j$$ integrates
to 1 over edge $$ij$$. For some other edge that is not $$ij$$, for instance
$$ik$$, we get:

$$\int_{ik} \underbrace{\phi_j}_0 \dd \phi_i - \phi_i \dd \phi_j = \int_{ik}
  \underbrace{\phi_j}_0 \dd \phi_i = 0$$

by our first identity. The same trick works for any edge that is not $$ij$$, so
that the $$\phi_{ij}$$ are a basis for discrete forms:

$$\hat{\phi}_{ij} = e_{ij}$$

Unlike 0 and 2-forms, there's no immediate way to link the $$L^2$$ inner product
to the usual integration of functions: the inner product is obtained from the
Riemmanian metric as:

$$\inner{\omega_1, \omega_2} = \int_\Omega \inner{\omega_1^\sharp(x), \omega_2^\sharp(x)}.\dd A$$

where the Riemannian metric is used to define the $$\sharp, \flat$$ operators
between the tangent and cotangent bundles. We may now proceed to compute the
$$L^2$$ inner product of Whitney forms $$\phi_{ij}$$, and see how to transfer it
to discrete forms with a metric. 

### Discrete Metric

$$\inner{\phi_{ij}, \phi_{kl}}_{L^2} = \int_\Omega \inner{\phi_{ij}(x), \phi_{kl}(x)}.\dd A$$

Split on edge cells $$\sigma(e)$$:

$$\inner{\phi_{ij}, \phi_{kl}}_{L^2} = \sum_e \int_{\sigma(e)} \inner{\phi_{ij}(x), \phi_{kl}(x)}.\dd A$$

Use 1-point quadrature at the center of each edge $$c(e)$$:

$$\int_{\sigma(e)} \inner{\phi_{ij}(x), \phi_{kl}(x)}.\dd A \approx
\vol{\sigma(e)} \inner{\phi_{ij}\block{c(e)}, \phi_{kl}\block{c(x)}}$$

Now assume $$e = ij$$. 

### Discrete Metric

As before, let us consider the $$L^2$$ norm for $$\phi_{ij}$$, which will
correspond the the metric diagonal terms:

$$\norm{\phi_{ij}}_{L^2}^2 = \int_\Omega \norm{\phi_{ij}(x)}^2.\dd A$$

Over triangle $$ijk$$ the basis functions $$\phi, \phi_j$$ are equal to the
barycentric coordinates $$\lambda_i, \lambda_j$$ over this triangle:

$$\int_{ijk} \norm{\phi_{ij}(x)}^2 = \int_{ijk} \norm{\lambda_i \nabla \lambda_j -
\lambda_j \nabla \lambda_i}^2$$

whose gradients $$\nabla \lambda_i, \nabla \lambda_j$$ are constant over
$$ijk$$. Expanding the squared norm gives:

$$\int_{ijk} \norm{\phi_{ij}(x)}^2 = \norm{\nabla \lambda_i}^2 \int_{ijk}\lambda_i^2 +  \norm{\nabla \lambda_j}^2 \int_{ijk}\lambda_j^2 - 2 \inner{\nabla \lambda_i, \nabla \lambda_j} \int_{ijk}\lambda_i \lambda_j$$

It can be show that:

$$
\begin{aligned}\int_{ijk} \lambda_i^2 = \int_{ijk} \lambda_j^2 &= \frac{|ijk|}{6}\\
\int_{ijk} \lambda_i\lambda_j &= \frac{|ijk|}{12} \\
\end{aligned}
$$

So that we end up with:

$$\int_{ijk} \norm{\phi_{ij}(x)}^2 = \frac{|ijk|}{6}\block{\norm{\nabla \lambda_i}^2 + \norm{\nabla \lambda_j}^2 - \inner{\nabla \lambda_i, \nabla \lambda_j}}$$


Using the Riemannian metric induced by the canonical metric on $$\RR^3$$, we can
obtain[^1] the following expression:

$$\nabla \lambda_i = \frac{|jk|}{2|ijk|} n_i$$

where $$n_i$$ is the unit vector orthogonal to $$jk$$ in triangle $$ijk$$. We
end up with the following:

$$\begin{aligned}
\norm{\nabla \lambda_i}^2 &= \frac{|jk|^2}{4 |ijk|^2} \\
\norm{\nabla \lambda_j}^2 &= \frac{|ki|^2}{4 |ijk|^2} \\
\inner{\nabla \lambda_i, \nabla \lambda_j} &= \frac{1}{4 |ijk|^2} \inner{jk, ki} \\
\end{aligned}$$

By the polarization identity:

$$
\begin{aligned}
\inner{jk, ki} &= -\inner{jk, ik} \\
&= -\frac{1}{2}\block{\norm{jk}^2 + \norm{ik}^2 - \norm{jk - ik}^2} \\
&= -\frac{1}{2}\block{\norm{jk}^2 + \norm{ik}^2 - \norm{ji}^2} \\
\end{aligned}
$$

Therefore:

$$\inner{\nabla \lambda_i, \nabla \lambda_j} = -\frac{1}{4 |ijk|^2} \frac{\norm{jk}^2 + \norm{ki}^2 - \norm{ji}^2}{2}$$


<!-- $$\inner{\nabla \lambda_i, \nabla \lambda_j} = \frac{|jk| |ik| \cos\block{\theta_k}}{4|ijk|^2}$$ -->

<!-- which can be shown to be equal to the so-called *cotan weights*: -->

<!-- $$\inner{\nabla \lambda_i, \nabla \lambda_j} = -\frac{\cot\block{\theta_k}}{4|ijk|^2}$$ -->

<!-- Similarly, it can be shown that: -->

<!-- $$\norm{\nabla \lambda_i}^2 = \frac{\cot\block{\theta_j} + \cot\block{\theta_k}}{4|ijk|^2}$$ -->

<!-- which brings us to: -->

<!-- $$\int_{ijk} \norm{\phi_{ij}(x)}^2 = \frac{1}{24|ijk|}\block{\cot\block{\theta_i} + \cot\block{\theta_j} + 3\cot\block{\theta_k}}$$ -->



# Notes & References

[^1]: It is easy to see that $$\lambda_i$$ does not change in direction $$jk$$
    (hence should be along $$n_i$$), and that moving along $$n_i$$ by the
    altititude $$h_i = \frac{2|ijk|}{|jk|}$$ of vertex $$i$$ to edge $$jk$$
    causes $$\lambda_i$$ to change linearly by exactly 1.

[^2]: From the formula $$\cot\block{\theta_i} = \frac{\modulus{ki}^2 +
    \modulus{ij}^2 - \modulus{jk}^2}{4\modulus{ijk}}$$

[^orthogonal-invariance]: we saw above that $$n$$-forms are rotation-invariant,
    so the definition makes sense: any orientation-preserving orthonormal basis
    will be weighted to 1

[^einstein-notation]: or *raises/lowers* the indices in Einstein notation

[^orthonormal-determinant]: rotations in metric $$M$$ are exactly the
    orientation and metric-preserving endomorphisms $$A$$ such that $$A^TMA =
    M$$ with $$\det(A) > 0$$, therefore $$\det(A) = 1$$.
