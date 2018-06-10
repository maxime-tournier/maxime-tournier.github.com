---
title: Projective Geometry
categories: [math]
---

# Projective Space

Take a real vector space $$E\simeq\RR^n$$, quotient it with the
*colinearily* equivalence relation:

$$x \sim y \iff \exists \lambda \in \RR^\star,\quad x = \lambda y$$

The projective space $$P(E)$$ is the quotient space for $$\sim$$. In
 other words, $$P(E)$$ is the space of vector lines of $$E$$. The
 *dimension* of the projective space $$P(E)$$ is $$n-1$$. In
 particular:
 
 - the projective *line* is $$P\block{\RR^2}$$
 - the projective *plane* is $$P\block{\RR^3}$$
 

# Affine Decomposition

Consider an affine hyperplane $$H$$ of $$E$$ that does not contain the
origin. Then, any vector line of $$E$$ that is not parallel to $$H$$
(*i.e.* vector lines of $$\vec{H}$$) can be uniquely associated with a
point of $$H$$. The vector lines parallel to $$\vec{H}$$ are elements
of $$P\block{\vec{H}}$$. We obtain the following decomposition:

$$P\block{\RR^n} \simeq \RR^n \cup P\block{\RR^{n - 1}}$$


# Homogeneous Coordinates

For $$E\simeq\RR^n$$, consider the affine hyperplane where the last
coordinate is $$1$$:

$$H=\{x \in E, \ e_n^Tx = 1\}$$

where $$e_n = (0, \dots, 1)^T$$ is the $$n$$-th canonical basis
vector. In particular, the direction space $$\vec{H} = \{x\in E,\ x_n
= 0\}$$ is called the *points at infinity*, and any vector line not in
$$\vec{H}$$ corresponds to a unique point of $$H$$, obtained by
dividing by the last coordinate.

