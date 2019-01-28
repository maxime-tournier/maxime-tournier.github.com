---
title: Continuation Passing Style
categories: [functional programming]
---

# Lambda-calculus

$$\begin{align}
    e :=&
       \quad x&\quad\textrm{(var)} \\
    &|\ \   \lambda x. e&\quad\textrm{(abs)} \\
    &|\ \   (e\ e)&\quad\textrm{(app)} \\
\end{align}$$


# CPS (call-by-value)

$$\newcommand{cps}[1]{[\![#1]\!]}
\newcommand{app}[2]{\left(#1\ #2\right)}
\begin{align}
    \cps{x} &= \lambda k.\app{k}{x} \\
    \cps{\lambda x.e} &= \lambda k.\block{\app{k}{\lambda x.\cps{e}}} \\
    \cps{\app{f}{e}} &= \lambda k.\app{\cps{f}}{\lambda f.\app{\cps{e}}{\lambda e.\block{f\ e\ k}}} \\
\end{align}$$

