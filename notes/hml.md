---
title: HML
categories: [prog]
---

## split

- sépare un contexte selon des variables
- généralement on splitte uniquement selon le domaine du préfixe
  ambiant, donc on peut se baser sur le rang des variables pour le faire

## var

- trivial
- renvoie juste le type selon le contexte

## let

- trivial
- infère le type de la variable (met à jour le préfixe)
- infère le type du corps avec le type de la variable dans le contexte (met à jour le préfixe)
- renvoie le type du corps avec le préfixe à jour

## abs

- infère le type flexible $$\varphi_1$$ du corps de l'abstraction en
  ajoutant la borne et le type du paramètre (borné trivialement)
- échec si on infère un type polymorphe pour le paramètre
- `split` le préfixe mis à jour $$Q_1$$ entre la partie qui concerne le
  contexte ambiant $$Q_2$$ et le reste $$Q_3$$ (i.e. les bornes qui
  concernent le corps de l'abstraction)
- `extend` une borne $$\beta \geq \varphi_1$$ pour le type de retour à $$Q_3$$
  et renvoie $$\alpha \to \beta$$ avec ces bornes.

## abs-ann

- idem sauf qu'on passe directement $$x: \sigma$$ lors de l'inférence du
  corps de l'abstraction


## app

- infère le type de la fonction, met à jour le préfixe
- infère le type de l'argument, met à jour le préfixe
- ajoute les bornes $$\alpha_1 \geq \varphi_1$$ et $$\alpha_2 \leq \varphi_2$$
  (modulo sub) + $$\beta \geq \bot$$ pour le type du résultat
- `unify` $$\alpha_1$$ et $$\alpha_2 \to \beta$$ sous ce préfixe
- `split` le préfixe résultant entre ce qui concerne le préfixe
  ambiant (mis à jour) et ce qui concerne purement cette application
  $$Q_5$$
- ce dernier préfixe est quantifié dans le type du résultat

## extend

- ajoute une borne à un préfixe en maintenant l'invariant d'avoir
  uniquement des bornes avec quantificateurs
- si la forme normale de la borne est un type sans quantificateur, on
  inline la borne via la substitution de retour
- sinon ajoute simplement la borne au contexte

## subsume

- essaie d'instancier un scheme $$\varphi=\forall Q_2.\rho_2$$ en un type
  $$\sigma=\forall \bar{\alpha}.\rho_1$$ sous un préfixe $$Q$$
- on suppose que $$Q_2$$ et $$Q$$ sont disjoints

----

- skolemize $$\sigma$$ et instancie $$\varphi$$ (en ajoutant ses bornes au
  contexte), puis `unify` les deux (sous ce contexte)
- `split` le préfixe et la substitution résultant pour ne garder que
  ce qui concerne $$Q$$ 
- vérifie qu'on n'a pas de skolems qui s'échappent dans la
  substitution et le préfixe résultat

## unify-schemes

- trouve le scheme $$\varphi$$ le plus général qui soit une instance de
  $$\varphi_1$$ et $$\varphi_2$$ (i.e. least upper bound, join), sous un préfixe
  $$Q$$
- les deux schemes sont en forme normale
- les domaines des deux schemes sont disjoints

----

- si l'une des bornes est triviale, renvoie l'autre
- sinon $$\varphi_i = \forall Q_i.\rho_i$$, et alors on `unify` les types
  $$\rho_i$$ en prenant en compte *toutes* les bornes 
- comme d'hab, on `split` le préfixe résultat selon ce qui concerne
  $$Q$$ et on quantifie le reste


## update

- met à jour un préfixe avec une substitution $$[\alpha := \sigma]$$
  ou une borne $$\alpha \geq \varphi$$
- maintient l'invariant sur le prefixe $$Q$$
- on suppose que le préfixe contient une borne sur $$\alpha \geq \hat{\phi}_1$$
- on suppose que $$\alpha \notin ftv(\varphi)$$ (resp $$ftv(\sigma)$$)
  - l'appel est toujours protégé par un `occurs-check`
- on suppose que  $$\hat{\varphi_1} \leq \sigma$$ (resp) $$\hat{\varphi_1} \leq \varphi$$
  - l'appel suit `subsume` (resp `unify-schemes`)
---

- pour une substitution $$[\alpha := \sigma]$$
  - on `split` selon les `ftv(\sigma)` (TODO pourquoi???)
  - on vire la borne et on substitue
  
- pour une borne $$\alpha \geq \varphi$$
  - on `split` selon les `ftv(\varphi)` (TODO pourquoi???)
  - dans ce qui reste:
    - si $\varphi$ n'est pas quantifié, on vire la borne et on
      substitue pour maintenir l'invariant
    - sinon, on remplace simplement la borne
	

## occurs-check

- attention: il faut aussi prendre en compte toutes les variables
  apparaissant dans les bornes du contexte qui concernent le type
  cible
  
--- 
  
- TODO

## unify

- unifie deux types F sous un préfixe de bornes
- les deux types sont supposés en forme normale
- renvoie une substitution vers des types F ainsi qu'un préfixe
  mis-à-jour
  
----

- cas triviaux
  - variable avec elle-même
  - application d'un constructeur

- variable bornée $$\alpha \geq \varphi$$ avec type $$\sigma$$
  - `occurs-check` $$\alpha \notin \sigma$$
  - `subsume` le type en la borne (essaie d'instantier la borne pour obtenir le type)
  - `update` le prefixe pour incorporer la substitution $$[\alpha := \sigma]$$
  
- variable $$\alpha_1 \geq \varphi_1$$ avec variable $$\alpha_2 \geq \varphi_2$$
  - `occurs-check` $$\alpha_1$$ dans $$\varphi_2$$ et réciproquement
  - `unify-schemes` $$\varphi_1, \varphi_2$$ pour donner $$\varphi$$
  - `update` le préfixe avec $$\alpha_2 := \alpha_1$$ et  $$\alpha_2 \geq \varphi$$
  
- type $$\forall \alpha.\sigma_1$$ avec type $$\forall \beta.\sigma_2$$
  - ajoute un skolem $$c$$
  - `unify` $$\sigma_1, \sigma_2$$ skolemisés avec $$c$$
  - verifie que le skolem ne s'échappe ni dans le préfixe ni dans la
    substitution
  - (suppose que les quantificateurs sont dans l'ordre standard)





