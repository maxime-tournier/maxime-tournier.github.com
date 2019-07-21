---
title: Dual Quaternions
categories: [math]
---

Rigid transformations may be blended efficiently by the use of dual
quaternions. This can be used for skinning 3D models on an articulated rigid
body[^kavan07]. Before reading these notes you'll probably want to get familiar
with [regular quaternions](quaternions) and [rigid transformations](rigids)
first.

# Dual Numbers

Consider numbers of the form:

$$a = a_0 + \epsilon a_\epsilon$$

for some magic constant $$\epsilon$$ satisfying $$\epsilon^2 = 0$$, with
sum/product distributive as usual. $$\epsilon$$ may be seen as spanning
*infinitesimal numbers*, *i.e.* cancelling anything of order 2 and more (which
forms the basis of *synthetic* differential geometry). For instance, consider
the product of dual numbers:

$$\block{a + \epsilon \dd a}\block{b + \epsilon \dd b} = a b + \epsilon \block{a.\dd b + \dd a.b}$$

The above gives exactly the formula for the derivative of a
product. In fact, any *analytic* function $$f$$ extends naturally to
dual numbers:

$$f\block{a + \epsilon \dd a} = f\block{a} + \epsilon \dd f(a).\dd a$$

In short, dual numbers provide an algebra for tangent vectors.

## Dual Inverse

From the usual formula for the derivative of the inverse, we get:

$$\block{x + \epsilon \dd x}^{-1} = \inv{x} - \epsilon \frac{\dd x}{x^2}$$

# Dual Quaternions

Again, consider numbers of the form:

$$q + \epsilon \dd q$$

with $$q, \dd q \in \HH$$ quaternions and the dual unit as
before. Again, functions on quaternions extend naturally on dual
quaternions by differentiation.

## Dual Quaternion Norm

From the usual norm [formula](differential-geometry#norm):

$$\norm{q + \epsilon \dd q} = \norm{q} + \epsilon \frac{q^T\dd q}{\norm{q}}$$

# Unit Dual Quaternions

If we normalize a dual quaternion $$q + \epsilon \dd q$$ by extending the
quaternion normalization, we will obtain a real part with unit norm. Therefore,
the corresponding dual part will be a tangent vector to the unit quaternion
sphere $$S^3$$:

$$\begin{align}
q + \epsilon \dd q &= q + \epsilon q.\omega^b \\
    &= q + \epsilon \omega^s.q\\
\end{align}$$ 

where the dual part is left-(resp. right)-trivialized using the body
velocity $$\omega^b$$ (resp. spatial velocity $$\omega^s$$), taken in the Lie
algebra $$\mathfrak{s^3}\simeq\RR^3$$ of pure imaginary quaternions. (see
[Quaternions](quaternions.html) and [Lie Groups](lie-groups.html) for details).

In summary: normalizing regular quaternions extends to dual quaternions by
differentiation as before, giving so-called unit dual quaternions. The real part
is a unit regular quaternion, while the dual part is a tangent vector at the
real part. This tangent vector has body/spatial coordinates in $$\RR^3$$, which
can be used to encode translations.

## Connection with Rigid Transformations

The spatial derivative of the unit quaternion product gives (as for any Lie
group):

$$\dd^s a.b = \dd^s a + \Ad_a \dd^s b$$

where in this case $$\Ad_a$$ is the rotation corresponding to quaternion
$$a$$. The above formula is exactly the translation part of the rigid
composition of $$\block{a, \dd^s a} \simeq \mat{\Ad_a & \dd^s a\\ 0 & 1}$$ with
$$\block{b, \dd^s b} \simeq \mat{\Ad_b & \dd^s b\\ 0 & 1}$$. Therefore, if we
encode our rigid transformations as spatial derivatives of unit quaternions
(expressed as dual quaternions accordingly), we can obtain the composition of
rigid transformations from the product of unit dual quaternions:

$$\block{a + \epsilon \dd^s a. a}\block{b + \epsilon \dd^s b.b} = a.b + \epsilon \block{ \dd^s a + \Ad_a \dd^s b}ab$$

In other words, there is a nice Lie group homomorphism between unit dual
quaternions and rigid transformations.

## Dual Quaternion Normalization

Now, the whole point of using unit dual quaternions is the cheap projection
operator on $$SE(3)$$ by means of dual quaternion normalization: from a series
of unit dual quaternions $$g_i = q_i + \epsilon t_i q_i$$ we may blend them
however we like to obtain some (possibly non-unit) dual quaternion $$\tilde{g} =
f\block{g_i}$$, which we can then normalize to obtain a unit dual quaternion,
and a corresponding rigid transformation:

$$g = \frac{\tilde{g}}{\norm{\tilde{g}}}$$

From the [usual](differential-geometry#normalization) formula, dual
quaternion normalization is: 

$$\frac{g}{\norm{g}} = \frac{q}{\norm{q}} + \epsilon \frac{1}{\norm{q}}\block{I - \frac{q q^T}{\norm{q}^2}}\dd q$$

Expanding the dual part gives:

$$
\begin{align}
\frac{1}{\norm{q}}\block{I - \frac{q q^T}{\norm{q}^2}}\dd q 
&= \block{\dd q.\inv{q} - \frac{q^T \dd q}{\norm{q}^2}} \frac{q}{\norm{q}}\\
&= \frac{1}{\norm{q}^2}\block{\dd q.\bar{q} - q^T \dd q} \frac{q}{\norm{q}}\\
&= \frac{1}{\norm{q}^2}\mathrm{Im}\block{\dd q.\bar{q}} \frac{q}{\norm{q}}\\
&= \mathrm{Im}\block{\dd q.\inv{q}} \frac{q}{\norm{q}}\\
\end{align}
$$

As expected, the dual part is that of a unit dual quaternion *i.e.* a
pure imaginary quaternion representing the translation, times the real
part. The translation part of the blended rigid transform is the
imaginary part of the blended quaternion spatial velocity, in the sense
of the full multiplicative quaternion Lie group $$\HH$$ (watch out:
the Lie algebra is the whole quaternion space $$\HH$$ in this case).

## Blending Algorithm

From the above, the general algorithm for dual quaternion blending
goes as follows:

1. encode rigid transformations $$(q, t)_i$$ as unit dual quaternions $$g_i = q_i + \epsilon \underbrace{t_i q_i}_{\dd q_i}$$

2. blend real/dual parts[^blending] to obtain a dual quaternion $$g =
   q + \epsilon \dd q$$

3. normalize the real part to obtain the blended rotation, and compute
   the imaginary part of the spatial velocity $$\mathrm{Im}\block{\dd
   q.\inv{q}}$$ to obtain the blended translation.

## Jacobian Matrix

For a linear blending $$q = \sum_i \alpha_i q_i$$, the dual part is:

$$\dd q = \sum_i \alpha_i \dd q_i = \sum_i \alpha_i t_i q_i$$

Let $$a=\dd q, b=q$$, the blended rotation is $$\frac{b}{\norm{b}}$$ and the
blended translation is $$t=\mathrm{Im}\block{a \inv{b}}$$. As before, we have:

$$
\begin{align}
\dd^s \frac{b}{\norm{b}}.\dd b &= \mathrm{Im}\block{\dd b.\inv{b}}\\
&= \mat{0 && I} R_{\inv{b}} \dd b\\
\end{align}
$$

for the rotation part, where $$R_q$$ is the [matrix form](quaternions#product) of
the right-multiplication by $$q$$. Now, the translation part is a little messier:

$$\begin{align}
\dd \block{a \inv{b}} &= \dd a.\inv{b} - a.\inv{b}.\dd b.\inv{b}\\
&= R_{\inv{b}}\block{\dd a - L_{a \inv{b}}\dd b} \\
\\
\dd a &= \sum_i \alpha_i \block{\dd t_i q_i + t_i \dd q_i} \\
&= \sum_i \alpha_i \block{\dd t_i q_i + t_i \omega^s_i q_i} \\
&= \sum_i \alpha_i \block{\dd t_i + t_i \omega^s_i}q_i \\
&= \sum_i \alpha_i R_{q_i}\block{\mat{0^T\\I} \dd t_i + L_{t_i}\mat{0^T\\I}\omega^s_i} \\
\\
\dd b &= \sum_i \alpha_i \dd q_i \\
&= \sum_i \alpha_i \omega^s_i q_i \\
&= \sum_i \alpha_i R_{q_i}\mat{0^T\\I}\omega^s_i\\
\end{align}$$

In the end, the $$i$$-th translation Jacobian block is:

$$
\begin{align}
\ddd{t}{t_i} &= \alpha_i \mat{0 && I}R_{\inv{b}} R_{q_i}\mat{0^T\\ I} \\
&= \alpha_i \mat{0 && I}R_{q_i\inv{b}}\mat{0^T\\ I} \\
\\
\ddd{t}{\omega^s_i} &= \alpha_i \mat{0 && I}R_{\inv{b}}\block{R_{q_i} L_{t_i}\mat{0^T\\ I} - L_{a\inv{b}}R_{q_i}\mat{0^T\\ I}}\\
&= \alpha_i \mat{0 && I}R_{\inv{b}} R_{q_i} \block{L_{t_i} - L_{a\inv{b}}}\mat{0^T\\ I}\\
&= \alpha_i \mat{0 && I}R_{q_i\inv{b}} L_{t_i - a\inv{b}}\mat{0^T\\ I}\\
\end{align}
$$

and the $$i$$-th rotation block is:

$$
\begin{align}
\ddd{^s\frac{b}{\norm{b}}}{\omega^s_i} &= \alpha_i \mat{0 && I} R_{\inv{b}} R_{q_i} \mat{0^T\\I} \\
&= \alpha_i \mat{0 && I} R_{q_i\inv{b}} \mat{0^T\\I} \\
\end{align}
$$


# Notes & References

[^blending]: For the formula to make sense, the dual blending must be
    the derivative of the real blending. Otherwise, the blended
    dual part will *not* be a tangent vector at $$q$$.

[^kavan07]: Kavan, Ladislav, et al. *"Skinning with dual quaternions."*
    Proceedings of the 2007 symposium on Interactive 3D graphics and
    games. ACM, 2007.

