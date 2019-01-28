---
title: Continuation Passing Style
categories: [functional programming]
---

$$\begin{align}
    e :=&
       \quad x&\quad\textrm{(var)} \\
    &|\ \   \lambda x. e&\quad\textrm{(abs)} \\
    &|\ \   (e\ e)&\quad\textrm{(app)} \\
\end{align}$$
