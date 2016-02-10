---
title: Y Combinator
categories: [math]
---

The $$Y$$ combinator makes it possible to define recursive function
using only anonymous functions. This page derives the $$Y$$ combinator
through the factorial function in the Scheme language, with some
comments.

{% include toc.md %}

```scheme
;; 0. our trusty factorial
(define fact
  (lambda (n)
    (if (= 0 n) 1
        (* n (fact (- n 1))))))

(echo (fact 10))

;; 1. but what if we don't make use of named functions?
(define fact
  (lambda (n)
    (if (= 0 n) 1
        (* n (??? (- n 1))))))

;; 2. ok let's say we pass ourselves as an extra argument then
(define fact
  (lambda (f n)
    (if (= 0 n) 1
        (* n (f f (- n 1))))))

(echo (fact fact 10))

;; 3. first, we curry the self parameter out
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

;; 5. equivalent lambda formulation: pass "s" as an argument
(define fact
  (lambda (f)
    ((lambda (s)
       (lambda (n)
         (if (= 0 n) 1
             (* n (s (- n 1))))))
     (lambda (x) ((f f) x)))))

(echo ((fact fact) 10))

;; 6. now we name "g" the nice lambda that defineines factorial the way
;; we want. in this representation, the self parameter will be
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
;; wrap it in a function that does it for us
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

;; 13. (lambda (x) (f x)) <=> f
(define Y
  (lambda (h)
    ((lambda (f) (lambda (x) ((f f) x))) 
     (lambda (f)
       ((lambda (g)
          (g (lambda (x) ((f f) x))))
        h)))))

(echo ((Y fact) 10))

;; 14. ((lambda (x) (f x)) y) <=> (f y) (here we substitute g for h to
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

# Comments

Ok so what did we just do? We ended up wrapping recursive functions
into lambdas to access their own name:

```scheme
;; v1
(lambda (self) (lambda (args) ... (self...)))
```

We could not use this definition directly in actual compuatations
however, since we had to self-apply our function to itself prior to
calling it:

```scheme
;; v2
(lambda (self) (lambda (args) ... ((self self)...)))
```

So, somewhere along the way we used the $$\Omega$$ combinator, which
self-applies a function to itself:

```scheme 
(define omega (lambda (f) (f f)))
```

In some sense, the $$Y$$ combinator obtained above is equivalent to:

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

But remember that $$v_2$$ is not really convenient to work with since
we don't want to self-apply ourselves all the time, so we can again
wrap the self-application to obtain, again, something equivalent to
$$v_1$$:

```scheme
(define v1 (omega v2))
```

But really, what we just did by converting back-and-forth between
$$v_1$$ and $$v_2$$ was exactly what the $$Y$$ combinator does:

```scheme
(define Y
  (lambda (v1)
    (omega (lambda (self) (v1 (omega self))))))
```

So this is what the $$Y$$ combinator is really doing: it wraps/unwraps
self-applications of a recursive function for us.

# Fixpoint 


