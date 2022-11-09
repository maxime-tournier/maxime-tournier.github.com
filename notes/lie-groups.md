---
title: Lie Groups
categories: [math]
---

# Definition

A Lie group is a group that is also a differentiable manifold, and where the
product/inverse operations are smooth. We will only consider real *matrix* Lie
groups, which are subgroups of $$GL_n(\RR)$$.

# Translations

The left and right translations by some element $$h \in G$$ are smooth maps:

$$ L_h(g) = h.g$$

$$ R_h(g) = g.h$$

Note that $$L_g$$ and $$R_h$$ commute for any $$g, h$$ by associativity of the
group product. They are also invertible and their inverse is smooth (which makes
them diffeomorphisms), with inverses given by:

$$\inv{L_h} = L_{\inv{h}}$$

$$\inv{R_h} = R_{\inv{h}}$$

As such, their tangent maps induce isomorphisms between tangent spaces:

$$\dd L_h(g): T_g(G) \simeq T_{hg}(G)$$

$$\dd R_h(g): T_g(G) \simeq T_{gh}(G)$$

where the inverses are given by:

$$\dd \inv{L_h} = \dd L_{\inv{h}}$$

$$\dd \inv{R_h} = \dd R_{\inv{h}}$$

In particular, the tangent space at each point $$g$$ is isomorphic to the
tangent space at the identity element:

$$\dd \inv{L_h}(e): T_e(G) \simeq T_g(G)$$

$$\dd \inv{R_h}(e): T_e(G) \simeq T_g(G)$$

These isomorphisms vary smoothly with $$g$$ so that the tangent bundle $$TG$$ is
*globally* isomorphic to $$G \times T_e(G)$$ (it is said *trivializable*): any
tangent vector $$\dd g$$ is uniquely determined by its coordinates in the
tangent space at the identity (together with the base point $$g$$). These
coordinates are called the left/right trivializations of $$\dd g$$ and satisfy:

$$\dd g = \dd L_g.\db g$$

$$\dd g = \dd R_g.\ds g$$

where $$\db g, \ds g$$ are tangent vectors at the identity element, respectively
called the *body-fixed* and *spatial* velocities in robotics. Since we are only
dealing with matrix Lie groups, left/right translation derivatives are given by
matrix products:

$$\dd L_h(g).\dd g = h.\dd g$$

$$\dd R_h(g).\dd g = \dd g.h$$

Therefore, left/right trivializations are simply:

$$\dd g = g.\db g = \ds g.g$$

For reasons that will become clear later, the tangent space at the identity
$$T_e(G)$$ is called the *Lie algebra* of the group $$G$$, denoted $$\alg{g}$$.


# Adjoint

The inner automorphism $$\Psi_h(g) = h.g.\inv{h}$$ is obtained by composing
left/right translations by $$h$$: 

$$\Psi_h = L_h \circ R_\inv{h} = R_\inv{h} \circ L_h$$

Its differential is an isomorphism between tangent spaces, and is an
automorphism when differentiated at the identity. This automorphism is called
the *adjoint* of the group:

$$\begin{align}
\Ad_h &= \dd \Psi_h(e): \alg{g} \to \alg{g}\\
&= \dd L_h.\dd R_\inv{h} = \dd R_\inv{h}.\dd L_h \\
\end{align}
$$

It is a group homomorphism: 

$$\Ad: G \to GL(\alg{g})$$

$$\Ad_{gh}=\Ad_g.\Ad_h$$

$$\Ad_{\inv{h}}=\inv{\Ad_h}$$

This group homomorphism provides the *adjoint representation* of the group over
its Lie algebra. The adjoint can itself be differentiated:

$$
\begin{align}
\dd \Ad_g(x).\dd g &= \dd g.x.\inv{g} - g.x.\inv{g}.\dd g.\inv{g}\\
    &= g.\db g.x.\inv{g} - g.x.\db g.\inv{g} \\
    &= \Ad_g.\block{\db g.x - x.\db g} \\
\end{align}
$$

The expression $$\block{\db g.x - x.\db g}$$ is called a *Lie bracket*, denoted
by $$[\db g, x]$$. It is always an element of the Lie algebra. From the above,
we see that the left-trivialized tangent of $$\Ad_g$$ is $$[\db g, .]$$, which
we denote by $$\ad(\db g)$$. One can easily check that the right-trivialized
tangent is $$\ad(\ds g)$$. The Lie bracket operator is what provides the Lie
algebra structure on the tangent space at the identity.

## Coordinate Changes

As stated above, the left/right trivializations of a tangent vector $$\dd g \in
T_g G$$ satisfy:

$$\dd g = g.\db g = \ds g.g$$

which implies:

$$\ds g = g.\db g.\inv{g} = \Ad_g.\db g$$

In other words, the group adjoint provides the conversion between *body-fixed*
and *spatial* coordinates.

## Coadjoint Representation

Simliarly, the dual space of the Lie algebra $$\alg{g}^\star$$ can also be used
for the left/right trivialization of tangent *covectors* $$\tau_g^\star \in
T_g^\star (G)$$, via the natural pairing between tangent vectors and covectors:

$$
\begin{align}
\tau_g^\star.\dd g &= \tau_g^\star.\dd L_g.\db g \\
&= \tau_g^\star.\dd R_g.\ds g \\
\end{align}
$$

from which we identify the *body-fixed* coordinates $$\tau_g^{\star b} =
\tau_g^\star.\dd L_g$$, and *spatial* coordinates $$\tau_g^{\star s} =
\tau_g^\star.\dd R_g$$. The conversion between the two is given by:

$$
\begin{align}
\tau_g^{\star b} &= \tau_g^\star.\dd L_g \\
&= \tau_g^{\star s}.\inv{\dd R_g}.\dd L_g \\
&= \tau_g^{\star s}.\dd R_{\inv{g}}.\dd L_g \\
&= \tau_g^{\star s}.\Ad_g \\
\end{align}
$$

In other words: 

$$\tau_g^{\star b} = \Ad_g^\star.\tau_g^{\star s}$$

It should be noted that the mapping $$g \mapsto \Ad_g^\star$$ is *not* a group
representation since $$\Ad_{gh}^\star = \Ad_h^\star.\Ad_g^\star$$. Instead, the
mapping $$g \mapsto \Ad_{\inv{g}}^\star$$ is used and is called the *coadjoint*
representation of the group over its Lie coalgebra. It satisfies:

$$\tau_g^{\star s} = \Ad_{\inv{g}}^\star.\tau_g^{\star b}$$

Put succintly, while the adjoint representation maps body-fixed velocities to
spatial velocities, the coadjoint representation maps body-fixed forces to
spatial forces. 

## Lie Coalgebra

TODO

- differentiating of the coadjoint representation at the identity gives a Lie
  bracket on the cotangent space at the identity: the Lie coalgebra
  


# Derivatives

As seen above, tangent vectors between Lie groups can be left/right trivialized
over the corresponding Lie algebras. Likewise, tangent maps between tangent
spaces can be left/right trivialized as linear maps between Lie algebras:

$$f: G \to H$$

$$\dd f(g): T_g(G) \to T_{f(g)} H$$

$$\db f(g): \alg{g} \to \alg{h} = \dd L_{\inv{f(g)}}.\dd f(g).\dd L_g$$

$$\ds f(g): \alg{g} \to \alg{h} = \dd R_{\inv{f(g)}}.\dd f(g).\dd R_g$$

From the above it can be checked that:

$$\db L_h = I$$

$$\ds R_h = I$$

Since left/right translation commute $$R_h \circ L_g = L_g \circ R_h$$, we also
get the following:

$$
\begin{align}
\ds L_h &= \Ad_h \\
\db R_h &= \Ad_{\inv{h}}\\
\end{align}
$$

Probably the most useful to remember in practice is $$\ds L_h = \Ad_h$$, and
recover the others from it. It also provides the interpretation of the group
adjoint as a coordinate change.

## Inverse 

From $$g.\inv{g} = e$$, we get:

$$\dd g.\inv{g} + g.\dd\inv{g} = 0$$

In other words:

$$
\begin{align}
\dd\inv{g} &= -\inv{g}.\dd g.\inv{g} \\
           &= -\dd L_{\inv{g}}.\dd R_{\inv{g}}.\dd g \\
\end{align}
$$

The left/right trivializations are given by:

$$\begin{align}
\db\inv{g} &= -g.\db g.\inv{g} = -\Ad_g.\db g \\
\ds\inv{g} &= -\inv{g}.\ds g.g = -\Ad_{\inv{g}}.\ds g\\
\end{align}
$$

## Product 

Product is bilinear, hence:

$$
\begin{align}
\dd\block{a.b} &= \dd a.b + a.\dd b \\
&= \dd R_b.\dd a + \dd L_a.\dd b \\
\\
\db\block{a.b} &= \Ad_{\inv{b}}.\db a + \db b \\
\ds\block{a.b} &= \ds a + \Ad_a.\ds b \\
\end{align}
$$



# Lie Algebra

- TODO Lie bracket properties (skew-symmetric, Jacobi identity)
- TODO Lie algebra properies, classifications

# Exponential

- TODO 

# Second Derivatives

Let us consider a matrix Lie group $$G$$ as a subgroup of $$GL(n)$$, a group
element $$g \in G$$ and two derivation directions $$\dd g_1, \dd g_2 \in
\alg{g}$$. (TODO two vector fields instead) By Schwartz's theorem, the following
holds in $$GL(n)$$ *taken as a vector space*:

$$\dd^2 g_1.\dd g_2 = \dd^2 g_2.\dd g_1$$

Now, consider two left trivializations of tangent vectors $$\dd g_1, \dd g_2$$
as:

$$\begin{align}
\dd g_1 & = g \omega_1 \\
\dd g_2 & = g \omega_2 \\
\end{align}$$

We get: 

$$\dd \omega_1.\dd g_2 = \dd \block{\inv{g}.\dd g_1}.\dd g_2 = -\inv{g}\dd g_2 \inv{g} \dd g_1 + \inv{g} \dd^2 g_1.\dd g_2$$

Or, after left-trivialization:

$$\dd \omega_1.\omega_2 = -\omega_2 \omega_1 + \inv{g} \dd^2 g_1.\dd g_2$$

Similarly: 

$$\dd \omega_2.\omega_1 = -\omega_1 \omega_2 + \inv{g} \dd^2 g_2.\dd g_1$$

The right-most terms being equal in the two equations, we obtain:

$$\dd \omega_1.\omega_2 + \omega_2 \omega_1 = \dd \omega_2.\omega_1 + \omega_1 \omega_2$$

Or, equivalently:

$$\begin{align}
\dd \omega_1.\omega_2 &= \dd \omega_2.\omega_1 + \omega_1 \omega_2 - \omega_2 \omega_1\\
&= \dd \omega_2.\omega_1 + [\omega_1, \omega_2]\\
\end{align}$$

So the Lie bracket gives the correction to apply when switching the derivation
order.

