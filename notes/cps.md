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
administrative redexes whenever possible. For instance, when
encountering $$\app{\cps{x}}{k}$$, we would like to $$\beta$$-reduce
$$\app{\lambda \kappa.\app{\kappa}{x}}{k}$$ to $$\app{k}{x}$$ directly
during translation.

To do so, the conversion function needs to return *static*
abstractions instead of CPS terms. Its type becomes: `cps : expr ->
(expr -> expr) -> expr`, where the function parameter is to be
replaced with static continuations (denoted $$\kappa$$ in the
above). Applications of static continuations will become static
too. Converting variables is straightforward:

> ```cps (x: var) kappa = kappa x```

For the rest, we mostly need to keep the type-checker happy. When
converting abstractions, we bump into the following:

> ```
cps ({x; e}: abs) kappa = kappa abs{x; cps e ????}
```

We just need to name the extra continuation parameter `k` and wrap
its application into a static continuation passed recursively to
`cps`:

> ```
cps ({x; e}: abs) kappa = 
    let k = gensym () in 
        kappa abs{x; abs{k; cps e (x => app{k; x})}}
```

Similarly, when converting applications: 

> ```
cps ({f; e}: app) kappa = 
    cps f (f => cps e (e => f e (kappa ????)))
```

In this case, the application of static continuation `kappa` must be
wrapped into a dynamic continuation:

> ```
cps ({f; e}: app) kappa = 
    cps f (f => cps e (e => f e 
        (let r = gensym() in abs{r; kappa r})))
```


We obtained what is known as the single-pass, *higher-order* cps
conversion.

## Properly tail-recursive CPS

In the case of a *tail-call* (that is, when converting an abstraction
whose body is an application), we see that we first introduce a static
continuation wrapping a named dynamic continuation (when converting
the abstraction), only to wrap it back into a dynamic continuation
(when converting the application).

Instead of wrapping twice, we could simply pass along the named
dynamic continuation `k` when converting the application:

> ```cps' ({f; e}: app) k = cps f (f => cps e (e => f e k))```

and have the `cps` handle this special case when converting
abstractions:

> ```
cps ({x; e}: abs) kappa = 
    let k = gensym () in 
        kappa abs{x; cps' e k}
```

The rest is pretty straightforward:

> ```cps' (x: var) k = app{k; x}```

> ```
cps' ({x; e}: abs) k = let c = gensym() in 
     app{k; abs{x; abs{c; cps' e c}}}
```


# TODO Partitioned CPS

Since CPS effectively destroys the stack (which may or may not be
desirable), it is often useful to separate CPS terms originating from
source abstractions (for which we may want to keep a call stack) and
the ones introduced by the conversion (which we may implement as
jumps).

# References 

[^1]: Danvy, O., & Filinski, A. (1992). [Representing control: A study of the CPS transformation](http://dotat.at/tmp/danvy-filinski-mscs92.pdf). Mathematical structures in computer science, 2(4), 361-391.
  
[^2]: Matt Might: [How to compile with continuations](http://matt.might.net/articles/cps-conversion/)
