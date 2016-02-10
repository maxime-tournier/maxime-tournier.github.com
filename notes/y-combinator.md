---
title: Y Combinator
categories: [math]
---

The $$Y$$ combinator makes it possible to define recursive function
using only anonymous functions. This note derives the $$Y$$ combinator
through the factorial function in the Scheme language, then presents
some comments adapted from various (excellent) sources.[^ayanonagon]
[^cannata].

{% include toc.md %}

# Factorial Example 

```scheme
;; 0. start with our trusty factorial
(define fact
  (lambda (n)
    (if (= 0 n) 1
        (* n (fact (- n 1))))))

(echo (fact 10))

;; 1. but what if we don't make use of named functions?
(define fact
  (lambda (n)
    (if (= 0 n) 1
        (* n (????? (- n 1))))))

;; 2. ok let's say we pass ourselves as an extra argument "f" then:
(define fact
  (lambda (f n)
    (if (= 0 n) 1
        (* n (f f (- n 1))))))

(echo (fact fact 10))

;; 3. and let's also curry the "f" parameter out
(define fact
  (lambda (f)
    (lambda (n)
      (if (= 0 n) 1
          (* n ((f f) (- n 1)))))))

(echo ((fact fact) 10))

;; 4. then we name the self-application function "s"
(define fact
  (lambda (f)
    (let ((s (lambda (x) ((f f) x))))
      (lambda (n)
        (if (= 0 n) 1
            (* n (s (- n 1))))))))

(echo ((fact fact) 10))

;; 5. transform the let into a lambda: pass "s" as an argument
(define fact
  (lambda (f)
    ((lambda (s)
       (lambda (n)
         (if (= 0 n) 1
             (* n (s (- n 1))))))
     (lambda (x) ((f f) x)))))

(echo ((fact fact) 10))

;; 6. now we name "g" that nice lambda defining factorial the way
;; we like. in this representation, the self parameter will be
;; replaced with the self-application function.
(define fact
  (lambda (f)
    (let ((g (lambda (s)
              (lambda (n)
                (if (= 0 n) 1
                    (* n (s (- n 1))))))))
      (g (lambda (x) ((f f) x))))))

(echo ((fact fact) 10))

;; 7. again, replace let with a lambda
(define fact
  (lambda (f)
    ((lambda (g)
       (g (lambda (x) ((f f) x))))
     (lambda (s)
       (lambda (n)
         (if (= 0 n) 1
             (* n (s (- n 1)))))))))

(echo ((fact fact) 10))

;; 8. now we don't want to call ourselves (fact fact) all the time, so we
;; wrap everything in a function that does it for us
(define fact
  (lambda (x)
    (let ((res (lambda (f)
                ((lambda (g)
                   (g (lambda (x) ((f f) x))))
                 (lambda (s)
                   (lambda (n)
                     (if (= 0 n) 1
                         (* n (s (- n 1))))))))))
      ((res res) x))))
      
(echo (fact 10))

;; 9. we name the self-application, then let->lambda again
(define fact
  (lambda (x)
    (let ((s (lambda (f) (lambda (x) ((f f) x)))))
      ((s 
        (lambda (f)
          ((lambda (g)
             (g (lambda (x) ((f f) x))))
           (lambda (s)
             (lambda (n)
               (if (= 0 n) 1
                   (* n (s (- n 1)))))))))
       x))))
       
(echo (fact 10))

;; 10. let->lambda once more
(define fact
  (lambda (x)
    (((lambda (f) (lambda (x) ((f f) x))) 
      (lambda (f)
        ((lambda (g)
           (g (lambda (x) ((f f) x))))
         (lambda (s)
           (lambda (n)
             (if (= 0 n) 1
                 (* n (s (- n 1)))))))))
     x)))
     
(echo (fact 10))

;; 11. now we isolate the actual definition of factorial, and pass it as a
;; parameter
(define fact
  (lambda (s)
    (lambda (n)
      (if (= 0 n) 1
          (* n (s (- n 1)))))))

;; 12. this is the Y-combinator in disguise !
(define Y
  (lambda (h)
    (lambda (x)
      (((lambda (f) (lambda (x) ((f f) x))) 
        (lambda (f)
          ((lambda (g)
             (g (lambda (x) ((f f) x))))
           h)))
       x))))

(echo ((Y fact) 10))

;; 13. (lambda (x) (f x)) is simply f
(define Y
  (lambda (h)
    ((lambda (f) (lambda (x) ((f f) x))) 
     (lambda (f)
       ((lambda (g)
          (g (lambda (x) ((f f) x))))
        h)))))

(echo ((Y fact) 10))

;; 14. ((lambda (x) (f x)) y) is simply (f y) (here we substitute g for h to
;; get rid of the enclosing lambda)
(define Y
  (lambda (h)
    ((lambda (f) (lambda (x) ((f f) x))) 
     (lambda (f) (h (lambda (x) ((f f) x)))))))

(echo ((Y fact) 10))

;; 15. similarly, simplify self-application, and we're done!
(define Y
  (lambda (h)
    ((lambda (f) (f f))
     (lambda (f) (h (lambda (x) ((f f) x)))))))

(echo ((Y fact) 10))
```

# WTF?!

Ok, so what did we just do? We ended up wrapping recursive functions
into lambdas in order to access their own name:

```scheme
;; v1
(lambda (self) (lambda (args) ... (self...)))
```

This is nice, however we could not use this definition directly in
actual computations, since we had to self-apply our function to itself
prior to calling it:

```scheme
;; v2
(lambda (self) (lambda (args) ... ((self self)...)))
```

So, somewhere along the way we used the $$\Omega$$ combinator, which
self-applies a function to itself:

```scheme 
(define omega (lambda (f) (f f)))
```

In some sense, the $$Y$$ combinator as defined above is equivalent to:

```scheme
(define Y
  (lambda (h)
    (omega (lambda (f) (h (omega f))))))
```

If we consider the two versions of our function $$v_1$$ and $$v_2$$
above, then we could as well have said:

```scheme
(define v2 (lambda (self) (v1 (omega self)))
```

But as we saw, $$v_2$$ is not really convenient to work with since we
don't want to self-apply ourselves all the time, so we can again wrap
the self-application to obtain something which behaves like the
recursive function *described* by $$v_1$$:

```scheme
;; v3
(define fact (omega v2))

;; now I can use it as intended
(fact 10)
```

But really, what we just did by converting between $$v_1$$, $$v_2$$
and $$v_3$$ is exactly what the $$Y$$ combinator does:

```scheme
(define Y
  (lambda (v1)
    (omega (lambda (self) (v1 (omega self))))))
```

So this is what the $$Y$$ combinator is doing: it takes the nice
definition of a recursive function wrapped in a lambda (à la $$v_1$$),
plugs-in the self-applications required for actual concrete uses (à la
$$v_2$$), then adds an extra self-application so that we are left with
something convenient to work with, and which behaves exactly like the
recursive function we intended to define in the first place. And it
does so without ever naming a single function.

Given a function $$f$$, the $$Y$$ combinator *applies self-application
to the application of $$f$$ to self-application.*[^cannata] Now
hopefully this last sentence makes perfect sense.

# Evaluation Issues

But if we try and use the following definition for the $$Y$$
combinator:

```scheme
(define Y
  (lambda (h)
    (omega (lambda (f) (h (omega f))))))
```

then the program gets stuck in an infinite recursion! This is due to
the fact that Scheme uses call-by-value (*i.e.* evaluates the
arguments *before* the function call). By replacing $$\Omega$$:

```scheme
(lambda (f) (f f))
```

with: 

```scheme
(lambda (f) (lambda (x) ((f f) x)))
```

we add an extra layer of indirection that delays the evaluation of
```(f f)``` until we actually need it. If we don't do so, the
evaluation of ```(f f)``` triggers an infinite recursion and the
program eventually runs out of memory.

# Fixpoint

If we describe a recursive function by wrapping it in a lambda (à la
$$v_1$$):

```scheme
;; recursive function description
(define g (lambda (self) (lambda (args) ... (self...))))
```

Then the $$Y$$ combinator returns the function described by ```g```:

```scheme
;; Y computes the actual recursive function from its description
(define f (Y g))
```

In other words, ```f``` behaves exactly like the ```self``` parameter
 inside ```g```. So if we pass ```f``` as ```self``` in
```g```, we again obtain something that computes ```f```:

$$ f = g(f) $$

Replacing $$f$$ with its definition, we end up with:

$$ Y(g) = g\block{Y(g)} $$

In other words, $$Y(g)$$ is a fixpoint of $$g$$, for all $$g$$. $$Y$$
is called a *fixpoint combinator* since it computes a fixpoint of its
argument. If the argument happens to be a description of a recursive
function wrapped in a lambda (here ```g```), the fixpoint of the
description is the recursive function itself (here ```f```, which acts
like ```self```), which is what $$Y$$ returns.

# TODO Typing

# References 

[^ayanonagon]: [The Y Combinator (no, not that one)](https://medium.com/@ayanonagon/the-y-combinator-no-not-that-one)
[^cannata]: [Sketchy LISP: Lambda Calculus and the Y Combinator](http://www.cs.utexas.edu/~cannata/cs345/Class%20Notes/27%20Lambda%20Calculus%20and%20the%20Y%20Combinator.html)
