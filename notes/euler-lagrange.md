---
title: Euler-Lagrange Equations
categories: [math, phys]
---

# Lagrangian

$$\LL(t, q, v) : \RR \times \RR^n \times \RR^n \to \RR$$

Let us consider a smooth curve $$q: \RR \to \RR^n$$, the Lagrangian
$$\LL$$ is evaluated along $$q$$ by composing it with:

$$t \mapsto \block{t, q(t), \dot{q}(t)}$$

The *action* $$S$$ of $$\LL$$ along $$q$$ is a functional that
integrates $$\LL$$ along the path $$q$$ between end-points $$q_0 =
q(0)$$ and $$q_1 = q(1)$$:

$$S: \mathcal{C}^\infty\block{[0, 1], \RR^n} \to \RR, \quad S(q) = \int_{t=0}^{t=1}\LL\block{t, q(t), \dot{q}(t)}.\dd t$$

The action $$S$$ computes the cost associated with a given path
$$q$$. Assuming the end-points are kept fixed, it is natural to ask
which trajectory minimizes the cost. As usual, a necessary condition
for a minimizer is to be a critical point of the action:

$$ \delta S(q) = 0 $$

# Calculus of Variations

Let us now consider the space of smooth functions
$$\mathcal{C}^\infty\block{[0, 1], \RR^n}$$ with fixed end-points
$$q_0, q_1$$ in more details: it is a vector space of infinite
dimension, and it is not too difficult to see that its tangent space
is the space of smooth functions with zero end-points. Let $$\delta
q$$ be such a tangent vector at $$q$$, called a *variation* of $$q$$.

By definition, $$\delta S(q).\delta q$$ is equal to the directional
derivative of $$S$$ in direction $$\delta q$$, which can be obtained
as:

$$\delta S(q).\delta q = \left.\frac{\dd}{\dd \epsilon}S\block{q + \epsilon \delta q} \right|_{\epsilon = 0}$$

Expanding the right-hand side a little bit:

$$
\begin{align}
\frac{\dd}{\dd \epsilon}S(q + \epsilon \delta q) &=\frac{\dd}{\dd \epsilon} \int_0^1\LL\block{t, q(t) + \epsilon \delta q(t), \dot{q}(t) + \epsilon \delta \dot{q}(t)}.\dd t \\
&= \int_0^1 \frac{\dd}{\dd \epsilon} \LL\block{t, q(t) + \epsilon \delta q(t), \dot{q}(t) + \epsilon \delta \dot{q}(t)}.\dd t \\
&= \int_0^1 \block{\ddd{\LL}{q}.\delta q(t) + \ddd{\LL}{v}.\delta \dot{q}(t)}.\dd t \\
&= \int_0^1 \ddd{\LL}{q}.\delta q(t).\dd t + \int_0^1\ddd{\LL}{v}.\delta \dot{q}(t).\dd t \\
\end{align}
$$

Let us now integrate the second term by parts:

$$
\begin{align}
\int_0^1\ddd{\LL}{v}.\delta \dot{q}(t).\dd t &= \underbrace{\left[\ddd{\LL}{v}.\delta q(t) \right]_0^1}_0 - \int_0^1\frac{\dd}{\dd t}\ddd{\LL}{v}.\delta q(t).\dd t \\
\end{align}
$$

So that we finally obtain:

$$\delta S(q).\delta q = \int_0^1\block{\ddd{\LL}{q} -\frac{\dd}{\dd t}\ddd{\LL}{v} } \delta q(t).\dd t$$

But since we look for a critical point, we have $$\delta S(q).\delta q = 0$$
for all variations $$\delta q$$, which implies[^1]:

$$\ddd{\LL}{q} -\frac{\dd}{\dd t}\ddd{\LL}{v} = 0$$

This equation is known as the
[Euler-Lagrange](https://en.wikipedia.org/wiki/Euler%E2%80%93Lagrange_equation)
equation, rewritten below with more details:

$$\frac{\dd}{\dd t}\ddd{\LL}{v}\block{t, q(t), \dot{q}(t)} = \ddd{\LL}{q}\block{t, q(t), \dot{q}(t)}$$

# Notes

[^1]: by the [fundamental lemma of calculus of variations](https://en.wikipedia.org/wiki/Fundamental_lemma_of_calculus_of_variations)

