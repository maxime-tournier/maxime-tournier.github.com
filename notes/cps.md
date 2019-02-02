---
title: Continuation Passing Style
categories: [functional programming]
---

Quick notes on basic CPS conversion after struggling to understand
[^2] and finally getting it thanks to [^1] (+countless others).

# Lambda-calculus

We start with the pure lambda calculus syntax with variables,
abstractions and applications:

$$\begin{align}
    e :=&
       \quad x&\quad\textrm{(var)} \\
    &|\ \   \lambda x. e&\quad\textrm{(abs)} \\
    &|\ \   (e\ e)&\quad\textrm{(app)} \\
\end{align}$$


# CPS (call-by-value)

Converting from the Direct Style (DS) call-by-value lambda calculus to
Continuation-Passing-Style (CPS) is pretty straightforward: each
converted term is an abstraction that accepts a *continuation*
$$\kappa$$ representing the rest of the program, and applies it where
needed:

$$\newcommand{cps}[1]{[\![#1]\!]}
\newcommand{app}[2]{\left(#1\ #2\right)}
\begin{align}
    \cps{x} &= \lambda \kappa.\app{\kappa}{x} \\
    \cps{\lambda x.e} &= \lambda \kappa.\app{\kappa}{\lambda x.\cps{e}} \\
    \cps{\app{f}{e}} &= \lambda \kappa.\app{\cps{f}}{\lambda f.\app{\cps{e}}{\lambda e.\block{f\ e\ \kappa}}} \\
\end{align}$$

This conversion can be given type `cps : expr -> expr`. Unfortunately,
it introduces many so-called *administrative redexes*, which we may
get rid of by separating *static* (translate-time) and *dynamic*
(runtime) abstractions/applications, and $$\beta$$-reducing static
administrative redexes whenever possible. For instance, in the
conversion for variables, when encountering $$\app{\cps{x}}{\kappa}$$,
we want to $$\beta$$-reduce $$\app{\kappa}{\lambda k.\app{k}{x}}$$ to
$$\app{\kappa}{x}$$ directly.

To do so, the conversion function will need to return *static*
abstractions instead of CPS terms. Its type becomes: `cps : expr ->
(expr -> expr) -> expr`, where the function parameter is to be
replaced with static continuations (denoted $$\kappa$$ in the
above). Applications of static continuations will become static
too. Variables are easy:

- ```cps (x: var) k = k x```

For the rest, we just need to keep the type-checker happy. When
converting abstractions, we need to name the extra continuation
parameter $$k$$ and wrap it into a static continuation to be given to
`cps`:

(TODO)

Finally, the static continuation $$\kappa$$ must be wrapped into a
dynamic continuation when converting applications:

(TODO)

We obtained what is known as the single-pass, *higher-order* cps
conversion.

# References 

[^1]: Danvy, O., & Filinski, A. (1992). Representing control: A study of the CPS transformation. Mathematical structures in computer science, 2(4), 361-391.
  
[^2]: Matt Might: [How to compile with continuations](http://matt.might.net/articles/cps-conversion/)
