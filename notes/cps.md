---
title: Continuation Passing Style
categories: [prog]
---

Quick notes on basic CPS conversion after struggling to understand
[^2] and finally getting it thanks to [^1] (+countless others).

{% include toc.md %}


# Lambda-calculus

We start with the pure lambda calculus syntax with variables,
abstractions and applications:

$$\begin{align}
\newcommand{app}[2]{\left(#1\ #2\right)}
    e :=\hspace{-.5em}&
       \quad x&\quad\textrm{(var)} \\
    &|\ \   \lambda x. e&\quad\textrm{(abs)} \\
    &|\ \   \app{e}{e}&\quad\textrm{(app)} \\
\end{align}$$

which we implement as:

```haskell
data Expr = Var String | Abs String Expr | App Expr Expr
```

# CPS (call-by-value)

Converting from the Direct Style (DS) call-by-value lambda calculus to
Continuation-Passing-Style (CPS) is pretty straightforward: each converted term
is an abstraction that accepts an extra *continuation* parameter $$\kappa$$
representing the rest of the program, and applies it where needed:

$$\newcommand{cps}[1]{[\![#1]\!]}

\begin{align}
    \cps{x} &= \lambda \kappa.\app{\kappa}{x} \\
    \cps{\lambda x.e} &= \lambda \kappa.\app{\kappa}{\lambda x.\cps{e}} \\
    \cps{\app{f}{e}} &= \lambda \kappa.\app{\cps{f}}{\lambda f.\app{\cps{e}}{\lambda e.\block{f\ e\ \kappa}}} \\
\end{align}$$

This conversion procedure can be given the following type:

```haskell
cps :: Expr -> Expr
```

However, we will need some bookkeeping for generating fresh variables
during the conversion. For this reason, our conversion will require a
`State` monad:

```haskell
-- cps state monad
import Control.Monad.State

type CPS a = State Int a

-- generate unique name
gensym :: String -> CPS String
gensym prefix = do
  counter <- get
  put (counter + 1)
  return (prefix ++ (show counter))
```

With that out of the picture, our conversion procedure becomes:

```haskell
-- continuation-passing-style conversion (naive)
cps :: Expr -> CPS Expr

-- var
cps (Var name) = do
  k <- gensym "k"
  return (Abs k (App (Var k) (Var name)))

-- abs
cps (Abs arg body) = do
  k <- gensym "k"
  b <- cps body
  return (Abs k (Abs arg b))

-- app
cps (App func arg) = do
  k <- gensym "k"
  f <- gensym "f"
  a <- gensym "a"
  func <- cps func
  arg <- cps arg
  return (Abs k
          (App func (Abs f
                     (App arg (Abs a
                               (App (App (Var f) (Var a)) (Var k)))))))
```

Unfortunately, this *naive* conversion introduces quite a lot of
so-called *administrative redexes*, which we may get rid of by
separating *static* (translation-time) and *dynamic* (run-time)
abstractions/applications, then $$\beta$$-reducing static
administrative redexes whenever possible. For instance, when
encountering $$\app{\cps{x}}{k}$$, we would like to $$\beta$$-reduce
$$\app{\lambda \kappa.\app{\kappa}{x}}{k}$$ to $$\app{k}{x}$$ during
translation.

To do so, the conversion function needs to return *static*
abstractions instead of CPS terms: these static abstractions will
produce the terms given a static continuation $$\kappa$$. The
conversion function type thus becomes:

```haskell
cps :: Expr -> (Expr -> CPS Expr) -> CPS Expr
```

Converting variables is straightforward: we just need to apply the
static continuation to our variable:

```haskell
cps (Var name) k = k (Var name)
```

For the rest, we mostly need to keep the type-checker happy. When
converting abstractions, we bump into the following issue:

```haskell
cps (Abs arg body) k = k (Abs arg (cps body ????))
```

The static continuation `k` is applied to the converted abstraction,
but we still need a (static) continuation to convert the function
body. The converted function body will pass its result `r` to whatever
(dynamic) continuation gets passed to the converted function on
runtime, so we simply give it a name `c` and wrap its application in
a static continuation:

```haskell
cps (Abs arg body) k = do
  c <- gensym "c"
  body <- cps body (\r -> return (App (Var c) r))
  k (Abs arg (Abs c body))
```

Similarly, when converting applications, we need to apply our static
continuation `k` to the application result. So again, we name this result
`r` and construct a dynamic continuation that will apply `k` to it:


```haskell
cps (App func arg) k = do
  r <- gensym "r"
  rest <- k (Var r)
  cps func (\f -> cps arg (\a -> return (App (App f a) (Abs r rest))))
```

We just obtained what is known as the single-pass, *higher-order* cps
conversion.

## Properly tail-recursive CPS

In the case of a *tail-call* (that is, when converting an abstraction
whose body is an application), we see that the conversion first
introduces a static continuation wrapping a named dynamic continuation
(when converting the abstraction), only to wrap it a second time into
a dynamic continuation (when converting the application).

Instead of wrapping twice, we could simply pass along the named
dynamic continuation `k` when converting the application:

```haskell
cps_tail :: Expr -> Expr -> CPS Expr
cps_tail (App func arg) k = 
  cps func (\f -> cps arg (\a -> return (App (App f a) k)))
```

and have `cps` handle this special case when converting abstractions:

```haskell
cps (Abs arg body) k = do
  c <- gensym "c"
  body <- cps_tail body (Var c)
  k (Abs arg (Abs c body))
```

The rest is pretty straightforward:

```haskell
cps_tail (Var name) k = return (App k (Var name))

cps_tail (Abs arg body) k = do
  c <- gensym "c"
  body <- cps_tail body (Var c)
  return (App k (Abs arg (Abs c body)))
```


# Extensions

Let us add a few constructs to our language:

$$\begin{align}
\newcommand{if}[3]{\block{\text{if}\ #1\ #2\ #3}}
    e := \hspace{-.5em}&
       \quad x&\quad\textrm{(var)} \\
    &|\ \  \lambda x. e&\quad\textrm{(abs)} \\
    &|\ \  \block{e\ e}&\quad\textrm{(app)} \\
    &|\ \  \if{e}{e}{e} &\quad\textrm{(cond)}\\
\end{align}$$

And its implementation:

```haskell
-- lambda calculus + conditionals
data Expr
  = Var String
  | Abs String Expr
  | App Expr Expr
  | Cond Expr Expr Expr
```

## Conditionals

Conditionals are straightforward: we convert the condition, test its
value, and convert branches continuing with our initial continuation:

$$\cps{\if{p}{c}{a}} = \lambda \kappa.\app{\cps{p}}
    {\lambda p.\if{p}{\app{\cps{c}}{\kappa}}{\app{\cps{a}}{\kappa}}}$$

The implementation follows closely, but we do not want to duplicate
the dynamic continuation (which may be large) in both branches of the
test. At least, we do not want to duplicate it *yet*. So we give it a
name:

```haskell
cps_tail (Cond pred conseq alt) k =
  cps pred (\p -> do
               c <- gensym "c"
               conseq <- cps_tail conseq (Var c)
               alt <- cps_tail alt (Var c)
               return (App (Abs c (Cond p conseq alt)) k))
```

For `cps`, we simply delegate to `cps_tail` by wrapping our static
continuation behind a dynamic continuation, just like we did earlier
for applications:

```haskell
-- wrap static continuation into dynamic continuation
wrap :: (Expr -> CPS Expr) -> CPS Expr
wrap k = do
  r <- gensym "r"
  rest <- k (Var r)
  return (Abs r rest)

cps (Cond pred conseq alt) k =
  cps pred (\p -> do
               k <- wrap k
               conseq <- cps_tail conseq k
               alt <- cps_tail alt k
               return (Cond p conseq alt))
```

In terms of `wrap`, converting applications becomes simply:

```haskell
cps (App func arg) k = do
  k <- wrap k
  cps_tail (App func arg) k
```

# Partitioned CPS

In CPS form all calls are tail calls, which can be implemented as
jumps: we can execute CPS programs without a stack. However, we may
want to keep a stack *e.g.* for separate compilation. In this case, it
is often useful to separate CPS terms originating from source
abstractions (for which we may want to keep a call stack) and the ones
introduced by the conversion, representing control flow (which we may
implement as jumps). Symmetrically, applications can be partitioned
into functions calls and continuation calls (jumps).


# References 

[^1]: Danvy, O., & Filinski, A. (1992). [Representing control: A study of the CPS transformation](http://dotat.at/tmp/danvy-filinski-mscs92.pdf). Mathematical structures in computer science, 2(4), 361-391.
  
[^2]: Matt Might: [How to compile with continuations](http://matt.might.net/articles/cps-conversion/)
