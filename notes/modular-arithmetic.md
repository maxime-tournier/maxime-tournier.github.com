---
title: Modular Arithmetic
categories: [math]
---

A few results in modular arithmetic.

# Greatest Common Divisors

For all integers $$a, b \in \ZZ$$ there exists a unique positive
integer $$d = \gcd(a, b) \in \NN$$ such that:

- $$a, b$$ are both multiples of $$d$$: $$\exists\,p,q\in\ZZ\mid a = pd,\, b = qd$$
- any common divisor $$c$$ of $$a, b$$ is also a divisor of $$d$$: $$\exists\,r\in\ZZ\mid d = r c$$

Note: "greatest" is in the sense of the order relationship "is divisible
by".

# Euclid's Algorithm

Given the Eulidean division $$a = b q + r, 0 \leq r < b$$, one can
easily show that $$\gcd(a, b) = \gcd(b, r)$$ by showing that $$\gcd(a,
b)$$ divides $$\gcd(b, r)$$ and conversely. This gives a simple
algorithm for computing $$\gcd(a, b)$$ using successive Euclidean
divisions:

$$r_{k-1} = r_k q_{k+1}  + r_{k+1},\  0\leq r_{k+1} < r_k$$

The last non-zero remainder is the GCD.


# Bezout's Theorem

Let $$a, b \in \ZZ$$ be two integers, then the following holds:

$$d = \gcd(a, b) \Rightarrow \exists\, x, y \in \ZZ \mid ax + by = d$$

Let $$S =\left\{ ax + by \mid x, y \in \ZZ \right\} = a\ZZ + b\ZZ$$
and $$c$$ a common divisor of $$a, b$$. Then for any $$n \in S$$,
$$c$$ divides $$n$$:

$$n = ax + by = pcx + qcy = c(px + qy)$$

Now, all we need to find is an element of $$S$$ that divides $$a, b$$
to prove the result. Let us consider the smallest positive integer $$d > 0 \in S$$ 
and the Euclidean division of $$n \in S$$ by $$d$$:

$$(a x' + b y') = n = qd + r$$

We may rewrite $$r$$ as:

$$r = ax' + by' - qd = a(x' - qx) + b(y' - qy)$$

so that $$r \in S$$.  By the definition of $$d$$, we are left with the
only possibility $$r = 0$$, otherwise we would have found a smaller
positive element in $$S$$. Therefore, $$d$$ divides $$n$$ for all $$n
\in S$$. In particular, $$a, b \in S$$ so $$d$$ is a common divisor of
$$a, b$$ that lies in $$S$$, which concludes the proof.

Note: we just shown that $$S = a\ZZ + b\ZZ = \gcd(a, b)\ZZ$$. This
shows that converse of the theorem is false, but that the following
holds: if $$a x + by = d$$, then $$\gcd(a, b)$$ divides $$d$$.

# Extended Euclid's Algorithm

Rather than discarding the $$q_k$$ in Euclid's algorithm, one can
exploit them to construct Bezout's decomposition. The $$k$$-th
iteration is given by:

$$r_{k-1} = r_k q_{k+1} + r_{k+1},\  0\leq r_{k+1} < r_k$$

Now, assuming we have the decomposition $$r_k = s_k a + t_k b$$, we
obtain the new decomposition for $$r_{k+1}$$ as:

$$r_{k+1} = r_{k-1} - r_k q_{k+1} = a\underbrace{\block{s_{k-1} - s_k q_{k+1}}}_{s_{k+1}} + b \underbrace{\block{t_{k-1} - t_k q_{k+1}}}_{t_{k+1}}$$

Euclid's algorithm starts with $$s_0 = 1, t_0 = 0, s_1 = 0, t_1 = 1$$.


# Coprime Integers

Two integers $$a, b \in \ZZ$$ are said *coprimes* if and only if
$$\gcd(a, b) = 1$$. In this case, Bezout's theorem works both ways:
$$a, b$$ are coprimes if and only if there exists integers $$x, y$$
such that $$ax + by = 1$$.

The Bezout decomposition gives the modular inverses: $$y = \inv{b}
\mod a$$ and $$x = \inv{a} \mod b$$

# Chinese Remainder Theorem

Let $$\block{n_i}_{i\leq k}$$ be $$k$$ *pairwise coprime* integers,
then there exists an integer $$x$$ such that, for any
$$k$$ integers $$\block{a_i}_{i\leq k}$$, the following holds:

$$x = a_i \mod n_i,\ \forall\, i \leq k $$

Moreover, $$x$$ is unique modulo $$n = \prod_i n_i$$. In other words,
given remainders modulo coprime integers, it is possible to
"reconstruct" the initial integer $$x$$. 

The reconstruction goes as follows: let us consider $$\hat{n}_i =
\frac{n}{n_i}$$, then we have $$\gcd\block{n_i, \hat{n_i}} = 1$$
and by Bezout's theorem, there exists $$u_i, v_i$$ such that:

$$1 = u_i n_i + \underbrace{v_i \hat{n}_i}_{e_i}$$

So we have $$e_i = 1 \mod n_i$$, and also $$e_i = 0 \mod n_j$$ for $$j
\neq i$$ since $$n_j$$ appears in $$\hat{n}_i$$. Finally, the $$e_i$$
provide a basis for expressing the solution as:

$$x = \sum_i a_i e_i$$


# TODO

- quotient ring, ideals

