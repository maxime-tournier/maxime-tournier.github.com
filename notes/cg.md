---
title: Conjugate Gradient
categories: [math]
---

Over time, I found myself checking the
[Wikipedia](http://en.wikipedia.org/wiki/Conjugate_gradient_method)
page for Conjugate Gradient over and over again, and finally got tired
of it. So here is a complete, self-contained and hopefully correct
derivation of the method, including non-standard inner products and
preconditioning, up to the Conjugate Residuals variations. The
alternate Lanczos formulation can be found in the notes for
[Krylov methods](krylov.md).

{% include toc.md %}

# Gradient Descent

  When a matrix $$A$$ is positive definite, the solution to $$Ax = b$$ is
  given by the *unique* minimizer of the following convex quadratic
  form: 
  
  $$ E(x) = \half x^T A x - b^T x $$

  The gradient descent algorithm proceeds by minimizing $$E$$ along
  successive *descent directions* $$p_k$$, given in this case by the
  negative gradient $$r_k = b - A x_k$$ of $$E$$:

  $$ x_{k+1} = x_k + \alpha_k p_k $$

  where $$p_k = r_k$$ is known as the *steepest descent*
  direction. $$\alpha_k$$ is found by the following *line-search*
  procedure, a one-dimensional minimization problem (convex in this
  case):

  $$ \alpha_k = \argmin{\alpha \in \RR} \quad f_k(\alpha) =  E\block{x_k + \alpha p_k} $$

  Stationarity conditions give:

  $$
  \begin{align}
  \dd f_k(\alpha) = 0 &\iff  \block{x_k + \alpha p_k}^T A p_k - b^Tp_k = 0 \\
  & \iff \quad p_k^T A p_k = \block{b - A x_k}^T p_k \\
  & \iff \quad \alpha = \frac{\block{b - A x_k}^T p_k}{p_k^T A p_k} 
  \end{align}
  $$
  
  The step size $$\alpha$$ is chosen such that the gradient becomes
  orthogonal to the search line. Geometrically, the line-search can
  also be seen in terms of the $$A$$-projection of $$x_k$$ along
  $$p_k$$:
  
  $$ x_{k+1} = x_k - p_k \frac{p_k^TAx_k}{p_k^TAp_k} \quad + \quad p_k \frac{p_k^T b}{p_k^TAp_k} $$
  
  At each iteration, the $$A$$-projection of $$x_k$$ along $$p_k$$ is
  replaced with that of the solution $$\inv{A}b = x^\star$$ to obtain
  $$x_{k+1}$$.
  
  Consequently, an interesting choice for descent directions
  $$\block{p_k}_k$$ is to use mutually $$A$$-orthogonal directions, so
  that the solution is found in maximum $$n$$ steps (in exact
  arithmetic). This is precisely the idea behind the Conjugate
  Gradient (CG) method.

# Conjugate Directions

  The CG algorithm chooses $$p_{k+1}$$ by $$A$$-orthogonalizing the
  gradient $$r_{k+1}$$ against the previous $$p_k$$, so that the family
  $$\block{p_k}_k$$ is $$A$$-orthogonal:

  $$ p_{k+1} = r_{k+1} - \sum_{i=1}^k p_i \frac{p_i^TAr_{k+1}}{p_i^T A p_i} $$

  We let $$\beta_{ik} = p_i^TAr_k$$ if $$i < k$$, $$\beta_{ii} = 1$$
  and $$\beta_{ik} = 0$$ if $$i > k$$, so that the recurrence relation
  becomes $$r_{k+1} = \sum_{i=1}^{i = k+1} p_i \beta_{ik}$$ or, in
  matrix form:

  $$R_k = P_k B_k$$

  where the matrix $$B_k = \block{\beta_{ij}}_{i, j \leq k}$$ is
  upper-triangular with unit diagonal (hence invertible). Now, since
  the $$p_i$$ are now mutually $$A$$-orthogonal, and from the
  geometric interpretation above (or simply by induction over the
  update formula for $$x_k$$), the $$A$$-projection of $$x_{k+1}$$
  over the previous $$\block{p_i}_{i\leq k} = P_k$$ gives:

  $$ P_k^T A x_{k+1} = P_k^T b $$

  Grouping both sides, we obtain:
  
  $$\underbrace{P_k^T}_{B_k^{-T} R_k^T} r_{k+1} = 0$$

  Which implies the following important property:
  
  $$ R_k^T r_{k+1} = 0 $$

  That is, the $$r_k$$ are mutually orthogonal. Before moving on, we
  also note that $$p_k^T r_k = \norm{r_k}^2$$. At this point, let us
  rewrite the $$r_k$$ update equations in block form, to get a clearer
  view of the situation. From each $$r_k - r_{k+1} = \alpha_k A p_k$$,
  we obtain:

  $$ R_{k+1} \underbrace{\mat{
  1& \\
  -1 & 1\\
  & -1 & 1 \\
  & & \ddots & \ddots \\
  & & & -1 & 1 \\
  & & & & -1 \\
  }}_{\underline{D}_k} = A P_k \block{\alpha_k}_k $$

  Projecting onto $$R_k$$, and introducing $$\phi_k = \norm{r_k}^2$$, gives:

$$
  \begin{align}
  
  R_k^T R_{k+1} \underline{D}_k &= R_k^T A P_k \block{\alpha_k}_k \\

  \mat{ \diag\block{\phi_k} & 0_k} \underline{D}_k &= R_k^T A P_k \block{\alpha_k}_k \\

  \diag\block{\phi_k} D_k &= R_k^T A P_k \block{\alpha_k}_k \\

  \end{align}
$$
  
  where $$D_k$$ is the upper $$k\times k$$ (upper Hessenberg) block from
  $$\underline{D}_k$$. More precisely:

  $$ \diag\block{\phi_k} D_k = \mat{
  \phi_1 & \\
   -\phi_2 & \phi_2\\
  & -\phi_3 & \phi_3 \\
  & & \ddots & \ddots \\
  & & & -\phi_k & \phi_k \\
  }
  $$
  
  From this, we finally get:

  $$ r_i^T A p_j \alpha_j = \cases{ 
  \phi_i  & if $i = j$ \\
  -\phi_i & if $j = i - 1$ \\
  0 & otherwise
  } $$

  Since $$\alpha_k \neq 0$$ (unless we are finished), $$p_i^T A r_{k+1} =
  0$$ for $$i < k$$ and the update for $$p_{k+1}$$ reduces to:

  $$ p_{k+1} = r_{k+1} - p_k \frac{p_k^TAr_{k+1}}{p_k^T A p_k} $$
  
  This is called a *short-recurrence* relation: each $$p_k$$ only needs
  to be conjugated with the previous one. Some alternate formula
  before finishing:

$$
  \begin{align}
  \beta_k &= \frac{p_k^TAr_{k+1}}{p_k^T A p_k} \\
  &= -\frac{\phi_{k+1}}{\alpha_k p_k^T A p_k } = -\frac{\phi_{k+1}}{r_k^T p_k} \\
  &= -\frac{\phi_{k+1}}{\phi_k} \\
  \\ 
  \alpha_k &= \frac{r_k^T p_k}{p_k^T A p_k} \\
  &= \frac{\phi_k}{p_k^T A p_k} \\
  \end{align}
  $$
  
# Standard Algorithm

  - Initialisation:
  
  $$ p_0 = r_0 = b - Ax_0 $$

  - Iteration: 

  $$
  \begin{align}
  x_{k+1} &= x_k + \alpha_k p_k  &\alpha_k = \frac{r_k^Tp_k}{p_k^T A p_k} &= \frac{r_k^T r_k}{p_k^T A p_k} \\
  r_{k+1} &= r_k - \alpha_k Ap_k & & \\
  p_{k+1} &= r_{k+1} - \beta_k p_k &\beta_k = \frac{r_{k+1}^T Ap_k}{p_k^T A p_k} &= -\frac{ r_{k+1}^T r_{k+1} }{r_k^T r_k} \\
  \end{align}
  $$
  
  While the initial search direction $$p_0$$ is set to the initial
  residual $$r_0$$, any other initial direction could be chosen. Some
  references mention, however, that the speed of convergence can be
  affected ( *i.e.* linear instead of superlinear) when the initial
  residual is not chosen for $$p_0$$.

  The normalized $$r_k$$ provide an orthogonal basis for the Krylov
  subspace $$\Krylov_k(A, b)$$ and can be related to the Lanczos
  vectors. If $$p_0 = r_0$$, then the $$p_k$$ vectors also provide an
  $$A$$-orthogonal basis for $$\Krylov_k(A, b)$$.

# (TODO) Convergence Analysis
  
# Non-Standard Inner Product

Let $$A$$ be self-adjoint with respect to an inner product $$M$$, and be
also positive-definite for $$M$$, that is:

- $$A^T M = M A$$,
- $$x^TMAx > 0$$ for all $$x > 0$$. 

Or simpler: $$MA$$ is symmetric positive definite. Then the following
quadratic form is positive-definite (thus convex):

$$
\begin{align}
E_M(x) &= \half \inner{x}{A x - b}_M \\
&= \half x^TMAx - x^TMb \\
\end{align}
$$
  
  So again, we look for its minimum. We apply the CG algorithm to the
  modified system $$MA x = Mb$$, letting $$z_k = M r_k$$ and denoting the
  $$MA$$-conjugate directions by $$q_k$$. We still obtain:
  
  - $$\alpha_k = \frac{z_k^T q_k}{q_k^TMAq_k}$$,
  - $$Q_k^Tz_{k+1} = Q_k^T M r_{k+1} = 0$$,
	
  Now we are faced with a choice regarding who should span the
  $$q_k$$. If the $$q_k$$ are spanned by $$r_k$$, we get:

  - $$R_k^T M r_{k+1} = 0$$,
  - $$q_0 = r_0 \Rightarrow x_k \in \Krylov_k\block{A, b}$$.

  In contrast, if the $$q_k$$ are spanned by the $$z_k$$ (which would be
  vanilla CG on the modified system), we get:

  - $$Z_k^T z_{k+1} = 0$$,
  - $$R_k^T M^T M r_{k+1} = 0$$,
  - $$q_0 = z_0 \Rightarrow x_k \in \Krylov_k\block{MA, Mb}$$.
	
  It is not immediately clear why one should adopt one over the other
  in the general case: it all depends on the problem at hand. However,
  the definition of the *gradient*, which depends on the metric $$M$$
  (in this case, the gradient of $$E_M$$ is the residual), plays in
  favor of the first definition. Some particular choices of $$A$$ and
  $$M$$ also allow to minimize the original quadratic form, by taking
  alternate paths in order to improve convergence (*cf.*
  preconditioning).
  
  Intuitively, the gradient descent follows directions that are
  *orthogonal* to level-sets in order to minimize the function, and
  orthogonality depends on the metric. Different metrics will yield
  different trajectories across level-sets, hopefully to obtain faster
  convergence towards the minimum. Anyways, since the second case was
  already described, let us continue with the first option and obtain
  the following algorithm:

  - Initialization:

  $$ p_0 = r_0 = b - Ax_0 $$

  - Iteration:

$$
  \begin{align}
  x_{k+1} &= x_k + \alpha_k p_k  & \alpha_k &= \frac{p_k^TMr_k}{p_k^TMAp_k} &= \frac{r_k^TMr_k}{p_k^TMAp_k}\\
  r_{k+1} &= r_k - \alpha_k Ap_k & & & \\
  p_{k+1} &= r_{k+1} - \beta_k p_k & \beta_k &= \frac{r_{k+1}^TMAp_k}{p_k^TMAp_k} &= -\frac{ r_{k+1}^T M r_{k+1} }{r_k^T M r_k} \\
  \end{align}
  $$
  
  which is exactly the standard CG algorithm using $$M$$ as
  inner-product.

## Note {#note}
   
   In the above, we never actually assumed that $$M$$ was an inner
   product. All that matters is that $$MA$$ remains positive definite,
   so $$M$$ could as well be indefinite, or not even symmetric,
   couldn't it? The problem however is that $$r_k$$ is not guaranteed
   to be a descent direction of the quadratic form when $$M$$ is not
   an inner product: it might happen that the quadratic form simply
   stagnates along $$r_k \neq 0$$:

   $$r_{k+1}^T M r_{k+1} = 0 $$ 

   Should this happen, the subsequent $$p_{k+1}$$ will also be a
   direction of stagnation since it always holds that $$P_k^T M
   r_{k+1} = 0$$, implying that:

   $$p_{k+1}^T M r_{k+1} = 0 $$ 

   At this point, the algorithm will stop makin any progress since
   $$\alpha$$ will be zero. It
   [seems](https://en.wikipedia.org/wiki/Positive-definite_matrix#Extension_for_non_symmetric_matrices)
   that it is necessary and sufficient to require that the symmetric
   part of the metric is positive definite, so that the above
   breakdown scenario can never happen.
	

# Preconditioning
  
  Preconditioning can be seen as a special case of the above:
  $$\inv{B}A$$ is PSD for the inner-product $$B$$ and the above
  algorithm then becomes the Preconditioned Conjugate Gradient. The
  $$p_k$$ will be $$A$$-conjugate, and $$A$$ will be minimized along
  the way, but the residuals $$r_k$$ will now be $$B$$-conjugate,
  hopefully to follow a faster path towards the solution. A
  straightforward adaptation of the above gives:
  
  - Initialization:
  
  $$ p_0 = r_0 = \inv{B}\block{b - Ax_0} $$

 - Iteration:

$$
  \begin{align}
  x_{k+1} &= x_k + \alpha_k p_k  & \alpha_k &= \frac{p_k^T B r_k}{p_k^TAp_k} \\
  r_{k+1} &= r_k - \alpha_k \inv{B}Ap_k & & \\
  p_{k+1} &= r_{k+1} - \beta_k p_k & \beta_k &= \frac{r_k^TAp_k}{p_k^TAp_k} \\
  \end{align}
$$

  It is probably more readable to use the residual for the original
  system instead, and introduce an extra variable $$z = \inv{B} r$$

 - Initialization:

  $$ r_0 = b - A x_0, \quad p_0 = z_0 = \inv{B} r_0 $$

 - Iteration:

$$
  \begin{align}
  x_{k+1} &= x_k + \alpha_k p_k  & \alpha_k &= \frac{p_k^Tr_k}{p_k^TAp_k} &= \frac{z_k^Tr_k}{p_k^TAp_k} \\
  r_{k+1} &= r_k - \alpha_k Ap_k & & \\
  z_{k+1} &= \inv{B} r_{k+1} \\
  p_{k+1} &= z_{k+1} - \beta_k p_k & \beta_k &= \frac{z_{k+1}^TAp_k}{p_k^TAp_k} &= -\frac{z_{k+1}^Tr_{k+1}}{z_k^T r_k} \\
  \end{align}
  $$
  

  Intuitively, the metric $$B$$ should ease motion where the curvature
  is low, and damp it where the curvature is high. By assigning a high
  cost to highly-curved directions, the gradient in these directions
  will become small (since you don't have to move a lot to get energy
  decrease, due to the high associated energy cost), and
  conversely. In this respect, the diagonal preconditioner $$B =
  \diag(A)$$ approximates the curvature along each axis in a
  Levernberg-Marquardt fashion, and damps the corresponding
  displacement proportionally.


# Conjugate Residuals

  This is exactly CG on $$A$$ with non-standard metric $$M =
  A^T$$. The residual norm is minimized, and the residuals are
  $$A^T$$-conjugate.
  
  - Initialization:
  
  $$ p_0 = r_0 = b - Ax_0 $$
  
  - Iteration:

$$
  \begin{align}
  x_{k+1} &= x_k + \alpha_k p_k  & \alpha_k &= \frac{p_k^T A^T r_k}{p_k^TA^TAp_k} &= \frac{r_k^TAr_k}{p_k^TA^TAp_k}\\
  r_{k+1} &= r_k - \alpha_k Ap_k & & \\
  p_{k+1} &= r_{k+1} - \beta_k p_k & \beta_k &= \frac{r_{k+1}^TA^TAp_k}{p_k^TA^TAp_k} &= -\frac{ r_{k+1}^T A r_{k+1} }{r_k^T A r_k} \\
  \end{align}
$$

  This algorithm yields the same iterates as MINRES (and is cheaper to
  compute). $$Ap$$ can be obtained from $$Ar$$ at each iteration to
  keep only one multiplication by $$A$$ per iteration.


# Preconditioned Conjugate Residuals

  This is the one found on [wikipedia](https://en.wikipedia.org/wiki/Conjugate_residual_method#Preconditioning). Again,
  this is CG on $$\inv{B}A$$ with metric $$A^T$$. The $$\inv{B}$$-norm
  of the residual is minimized, and the residuals are again
  $$A^T$$-conjugate.

  - Initialization:

  $$ p_0 = r_0 = \inv{B}\block{b - Ax_0} $$
  
  - Iteration:

$$
  \begin{align}
  x_{k+1} &= x_k + \alpha_k p_k  & \alpha_k &= \frac{p_k^T A^T r_k}{p_k^TA^T\inv{B} Ap_k} &= \frac{r_k^T A r_k}{p_k^TA^T\inv{B} Ap_k}\\
  r_{k+1} &= r_k - \alpha_k \inv{B} Ap_k & & \\
  p_{k+1} &= r_{k+1} - \beta_k p_k & \beta_k &= \frac{r_{k+1}^TA^T \inv{B} A p_k}{p_k^TA^T \inv{B} A p_k} &= -\frac{ r_{k+1}^T A r_{k+1} }{r_k^T A r_k} \\
  \end{align}
  $$
  
$$Ap$$ can be obtained from $$Ar$$ at each iteration to keep only one
multiplication by $$A$$ per iteration.


# Preconditioned Conjugate Residuals (alternate)

  This one is CG on $$\inv{B}A$$ using metric $$A^TB$$. In praticular,
  $$A$$ does not need to be symmetric, only $$A^TB$$ has to be
  positive definite. The norm of the residual is minimized, and the
  residuals are $$A^TB$$-conjugate:

  - Initialization:

  $$ p_0 = r_0 = b - \inv{B}Ax_0 $$

  - Iteration:

$$
\begin{align}
  x_{k+1} &= x_k + \alpha_k p_k  & \alpha_k &= \frac{p_k^T A^T Br_k}{p_k^TA^TAp_k} &= \frac{r_k^TA^TBr_k}{p_k^TA^TAp_k}\\
  r_{k+1} &= r_k - \alpha_k \inv{B}Ap_k & & & \\
  p_{k+1} &= r_{k+1} - \beta_k p_k & \beta_k &= \frac{r_{k+1}^TA^TAp_k}{p_k^TA^TAp_k} &= -\frac{ r_{k+1}^T A^TB r_{k+1} }{r_k^T A^TB r_k} \\
  \end{align}
  $$

  Using an extra variable $$z = \inv{B} r$$:

  - Initialization:

  $$ r_0 = b - Ax_0, z_0 = p_0 = \inv{B} r_0 $$

  - Iteration:

$$
\begin{align}
  x_{k+1} &= x_k + \alpha_k p_k  & \alpha_k &= \frac{p_k^T A^T r_k}{p_k^TA^TAp_k} &= \frac{z_k^TA^Tr_k}{p_k^TA^TAp_k}\\
  r_{k+1} &= r_k - \alpha_k Ap_k & & & \\
  z_{k+1} &= \inv{B}r_{k+1} & & & \\
  p_{k+1} &= z_{k+1} - \beta_k p_k & \beta_k &= \frac{z_{k+1}^TA^TAp_k}{p_k^TA^TAp_k} &= -\frac{ z_{k+1}^T A^T r_{k+1} }{z_k^T A^T r_k} \\
  \end{align}
  $$
