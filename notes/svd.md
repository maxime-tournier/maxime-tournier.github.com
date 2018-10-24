---
title: Singular Value Decomposition
categories: [math]
---

Mostly about the SVD/polar decomposition differentiation.

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

from which we already obtain $$\dd S$$. Let us now call $$R=\db U\ S + S\ \db
V^T$$ and decompose $$R$$ orthogonally over symmetric and skew-symmetric
matrices as $$R = R_+ \oplus R_-$$. For the symmetric part $$R_+$$, we obtain:

$$\begin{align}
R_+ &= \half\block{R + R^T}\\
    &= \half\block{\db U\ S + S\ \db V^T + \db V S + S \db U^T}\\
    &= \half\block{\underbrace{\block{\db U + \db V}}_{2W}S + S\block{\db U + \db V}^T}\\
    &= \block{WS + SW^T}\\
\end{align}$$

where $$W=\half\block{\db U + \db V}$$. Similarly, the skew-symmetric part is:

$$R_- = \block{ZS - SZ^T}$$

where $$Z = \half\block{\db U - \db V}$$. We can now identify $$W, Z$$ from the values of
$$R_+, R_-$$, from which we obtain $$\db U, \db V$$ as:

$$\begin{align}
    \db U &= W + Z \\
    \db V &= W - Z \\
\end{align}$$

## Symmetric Identification $$(n = 3)$$

$$R_+ = \mat{0 & \block{s_1 - s_2} w_3 & \block{s_3 - s_1} w_2 \\
             \star & 0 & \block{s_2 - s_3} w_1\\ 
             \star & \star & 0}$$


## Skew-Symmetric Identification $$(n = 3)$$

$$R_- = \mat{0 & \block{-s_1 - s_2} w_3 & \block{s_3 + s_1} w_2 \\
             -\star & 0 & \block{-s_2 - s_3} w_1\\ 
             -\star & -\star & 0}$$

## Jacobian Matrix
TODO

# Polar Decomposition

We want to compute $$f: A \mapsto UV^T$$ (for instance to get the closest
orientation to a given invertible linear map).

$$\begin{align}
\dd f(A).\dd A &= \dd U\ V^T + U\ \dd V^T \\
&= 2 U W V^T \\
\end{align}$$

where $$W$$ is as defined previously. Therefore we only have to identify the
symmetric part of $$U^T\ \dd A\ V$$.
