---
title: Type Inference
categories: [prog]
---

# HMF 

[HMF](https://www.microsoft.com/en-us/research/publication/hmf-simple-type-inference-for-first-class-polymorphism-2/)

## unification

- normal form for $$\sigma$$ types: all quantifiers are bound (appear
  in the type), ordered by appearance
  - not entirely clear why this is needed/practical
- type variables $$\alpha$$ unify with $$\sigma$$ types under standard
  occurs check
- type applications unify as usual
  - unclear whether higher-kinded types can be easily added
  - using kind-preserving substitutions should do the trick
- unification of $$\sigma$$ types: 
  - instantiate outer-most quantifiers to the same fresh skolem 
  - unify instantiated types 
  - fail the skolem escape, *i.e.* if some free type variable
    $$\alpha$$ gets unified with it
  - basically, only $$\sigma$$ types that are equivalent modulo
	variable renaming and substitution will unify.

## subsumption

- $$\sigma_1$$ is the expected function argument type, $$\sigma_2$$ is
the actual function argment type
- $$\sigma_2$$ should be "at least as polymorphic" as $$\sigma_1$$,
  possibly more
- roughly: "$$\sigma_2$$ should have at least all the quantifiers of $$\sigma_1$$"
- expressed as $$\sigma_2 \sqsubseteq \sigma_1$$
  - $$\sigma_1 = \forall \alpha. \alpha \to \alpha$$ and
    $$\sigma_2 = \forall \alpha\beta. \alpha \to \beta$$
  
  - `(forall s. St s a)` and `(forall s. St s (\forall a. a -> a)`
- in practice:
  - skolemize outermost quantifiers and instantiate $$\sigma_1$$ with them
  - instantiate $$\sigma_2$$ with fresh $$\alpha$$ variables 
	- these may unify with the skolems, which is fine (quantified in $$\sigma_2$$)
	- actually, skolems in $$\sigma_1$$ *must* unify with these variables for unification to succeed
  - unify instantiated types
  - check that no skolem escape through other $$\alpha$$ variables
- $$\sigma_2$$ will get $$\sigma_1$$ skolems occurring at the same
  places as in $$\sigma_1$$ possibly elsewhere too as long unification
  agrees.
	
### example

- $$\sigma_1 = \forall s.\mathrm{St}\ s\ a$$ and 
  $$\sigma_2 = \forall s.\mathrm{St}\ s\ (\forall a.a\to a)$$
- $$\sigma_1$$ instantiates $$s$$ to a skolem $$c$$, giving $$\mathrm{St}\ s\ a$$
- $$\sigma_2$$ instantiates $$s$$ to a fresh type variable $$\beta$$, which unifies with the skolem
- the free ($$\alpha$$-)type variable $$a$$ in $$\sigma_1$$ unifies
  with $$(\forall a.a\to a)$$


## inference

### var

- lookup $$\sigma$$ type for variable in the typing context

### abs

- fresh $$\alpha$$ type variable for parameter
- infer $$\sigma$$ type for function body with augmented typing context
- instantiate body type with fresh outer quantifiers as $$\rho$$
- generalize $$\alpha \to \rho$$



### app

- infer function type $$\sigma_1$$
- instantiate $$\sigma_1$$ outermost quantifiers to fresh $$\alpha$$
  type variables
- infer argument type $$\sigma_2$$
- subsume $$\sigma_1$$ to $$\sigma_2$$
- check to prevent free $$\alpha$$-variables from unifying with a $$\sigma$$-type:
  - split substitution into a polymorphic part and monomorphic part
  - check that no free type variable appear in the domain of the
    polymorphic part
