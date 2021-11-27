---
title: Singular Value Decomposition
categories: [math]
---

Mostly about the SVD/polar decomposition differentiation.
{% include toc.md %}

# SVD

Any square matrix $$A$$ can be decomposed as:

$$A = USV^T$$

where $$U, V \in O(n)$$ are orthogonal matrices and $$S$$ is diagonal and
positive. It is unique up to a permutation of the unique diagonal values of
$$S$$, called the *singular values*.

If $$A$$ is not square, the SVD still exists but $$S$$ is no longer square.
    
# Derivative

From $$A = USV^T$$, we get:

$$
\begin{align}
\dd A &= \dd U\ S V^T + U\ \dd S\ V^T + U S\ \dd V^T\\
    &= U\ \db U\ S V^T + U\ \dd S\ V^T + U S\ \db V^T\ V^T\\
\end{align}$$

where $$\dd U = U.\db U = \ds U.U$$ is the (left/right) trivialization of
tangent vector $$\dd U \in T_U O(n)$$, in this case with $$\db U, \ds U \in
\alg{o(n)}$$ *i.e.* skew-symmetric matrices (see [Lie groups](lie-groups) for
details).

From the above we get:

$$
\begin{align}
U^T \dd A V &= \db U\ S + \dd S + S\ \db V^T\\
&= \underbrace{\dd S}_{\text{diagonal}} \oplus \underbrace{\block{\db U\ S + S\ \db V^T}}_{\text{zero diag.}}\\
\end{align}
$$

from which we may already obtain $$\dd S$$ from the diagonal. Let us now call
$$R = \db U\ S + S\ \db V^T = \db U\ S - S \db V$$ and fiddle around a bit to
obtain:

$$\begin{align}
RS &= \db U\ S^2 - S\ \db V\ S \\
(RS)^T &= S\ \db V\ S - S^2 \db U \\
\end{align}$$

From this we obtain:

$$RS + (RS)^T = \db U\ S^2 - S^2 \db U$$

which we may rewrite as:

$$RS + (RS)^T = \hat{S} \circ \db U$$

where $$\hat{S} = \block{s_j^2 - s_i^2}_{i, j}$$ is skew-symmetric. We then
obtain $$\db U$$ as:

$$\db U = \check{S}\circ \block{RS + (RS)^T}$$

where $$\check{S}_{i,j} = \begin{cases}\frac{1}{s_j^2 - s_i^2} & i \neq j \\ 0 & i = j\end{cases}$$

TODO what if $$s_i = s_j$$

Similarly, we get $$\block{SR + (SR)^T} = \hat{S} \circ \db V$$ and obtain $$\db V$$ as

$$\db V = \check{S}\circ \block{SR + (SR)^T}$$

# Polar Decomposition
 
TODO REDO 
 
We want to compute $$f: A \mapsto UV^T$$ to get the closest orientation to a
given invertible linear map. The derivative gives:

$$\begin{align}
\dd f(A).\dd A &= \dd U\ V^T + U\ \dd V^T \\
&= U\ \db U V^T + U\ \db V^T V^T \\
&= U\underbrace{\block{\db U + \db V^T}}_Y V^T \\
\end{align}$$

from the above, we obtain $$Y$$ as:

$$\begin{align}
Y &= W + Z + \block{W - Z}^T\\
    &= 2Z
\end{align}
$$

so that we only have to identify $$Z$$ from the skew-symmetric part $$R_-$$,
which is well-defined when $$S > 0$$. We end up with:

$$\dd f(A).\dd A = 2 U Z V^T$$

which we can express as a spatial derivative as:

$$\ds f(A).\dd A = 2 U Z U^T = 2 \Ad_U Z$$

## Jacobian Matrix

The whole sequence of computations can be decomposed as follows:

$$\dd A \overset{G}{\mapsto} U^T\ \dd A\ V \overset{\pi}{\mapsto} R_- \overset{F}{\mapsto} Z \overset{2\Ad_U} \mapsto \ds f(A).\dd A$$

The operator $$G$$ is simply $$\dd L_{U^T}\ \dd R_V$$. The operator $$\pi$$
projects orthogonally over the set of skew-symmetric matrices, and the operator
$$F$$ selects/identifies $$z_1, z_2, z_3$$ from the coordinates of $$R_-$$.



