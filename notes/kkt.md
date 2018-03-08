---
title: Saddle-Point Systems
categories: [math]
---

General saddle-point systems factor as:

$$\mat{K & -A^T\\ -A & -C} = 
\mat{I & \\ -A\inv{K} & I} \mat{K & \\ & -A\inv{K}A^T -C } \mat{I & -\inv{K}A^T \\ & I}$$

We denote the Schur complement by $$S=A\inv{K}A^T +C$$. Letting $$C =
0$$, a general dual preconditionner $$W$$ acts on $$-A\inv{K}A^T$$ and
expands to:

$$\mat{I & \\ -A\inv{K} & I} \mat{I & \\ & W} \mat{I & \\ A\inv{K} & I} = \mat{I & \\ (W - I) A\inv{K} & W}$$

The preconditioned saddle-point system has dual part
$$-WA\inv{K}A^T$$, which expands to:

$$\mat{I & \\ (W - I) A\inv{K} & W}\mat{K & -A^T\\ -A & 0} = \mat{K & -A^T \\ -A & (I-W)A\inv{K}A^T}$$

It is positive definite whenever $$-W A\inv{K}A^T > 0$$

## Cholesky

From the above, a $$LDL^T$$ Cholesky decomposition of a KKT system is the following:

$$\mat{K & -A^T\\ -A & -C} = 
\mat{I & \\ -A\inv{K} & I} \mat{L_K D_K L_K^T & \\ & -L_S D_S L_S^T } \mat{I & -\inv{K}A^T \\ & I}$$

In other words, we get:

$$\begin{align}

\mat{K & -A^T\\ -A & -C} &= 
  \mat{L_K & \\ -A\inv{K}L_K & L_S} \mat{D_K & \\ & -D_S}  \mat{L_K^T & -L_K^T \inv{K}A^T \\ & L_S^T} \\
  &=   \mat{L_K & \\ -A L_K^{-T} & L_S} \mat{D_K & \\ & -D_S}  \mat{L_K^T & -L_K^{-1} A^T \\ & L_S^T} \\
\end{align}
$$


## Misc.

The inverted system is:

$$\mat{K & -A^T\\ -A & -C}^{-1} = 
\mat{I & \inv{K}A^T \\ & I} \mat{K^{-1} & \\ & -\block{A\inv{K}A^T + C}^{-1} } \mat{I & \\ A\inv{K} & I}$$

Also:

$$\mat{K & -A^T \\ -A & -C} \mat{0 \\ -z} = \mat{A^Tz \\ Cz}$$

can be used to optimize solves where the right-hand side has the above
form in order to optimize $$A^Tz$$ computation.
