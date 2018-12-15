---
title: Lie Groups
categories: [math, lie group]
---


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

