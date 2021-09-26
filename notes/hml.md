---
title: HML
categories: [prog]
---

([article](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/hml.pdf))

{% include toc.md %}

# notes

- on a donc les types de System-F $$\sigma$$ + les types *flexibles*
  $$\varphi$$ qui ont des bornes (flexibles également) sur les
  quantificateurs prénex. pour éviter certains problèmes (*cf* section
  5.4) on s'assure que les bornes $$\hat{\varphi}$$ ont bien des
  quantificateurs et que les variables quantifiées sont bien présentes
  dans le corps du type
- l'existence de types principaux assure que chaque nouvelle
  utilisation est nécéssairement une instance du type inféré jusqu'ici
  (ou une erreur de type)
  - les types F sont insuffisants dans le cas général pour avoir des
	types principaux en présence de types de rang supérieur: on peut
	donner à `(single id)` les deux types $$\forall \alpha.[\alpha \to
	\alpha]$$ et $$[\forall \alpha. \alpha \to \alpha]$$ sans qu'aucun
	ne soit une instance de l'autre au sens de System-F.
  - en revanche ces deux types sont instances du type flexible
    $$\forall (\beta \geq \forall \alpha \to \alpha).[\beta]$$
- en plus du contexte habituel $$\Gamma$$ on maintient également les
  bornes $$\hat{\varphi}$$ les plus spécifiques inférées pour les
  variables libres dans un *préfixe* $$Q$$, qu'on raffinera lors des
  unifications. les restrictions sur les bornes forcent à être
  soigneux lors des mises-à-jour du préfixe de manière à maintenir les
  invariants.
- le crux se situe au niveau de l'application pour laquelle on doit
  unifier deux types, $$\gamma$$ et $$\alpha \to \beta$$ avec les
  bornes suivantes dans le préfixe $$Q$$:
    - $$\gamma \geq \varphi_1$$ le type de la fonction 
	- $$\alpha \geq \varphi_2$$ le type de l'argument
	- $$\beta \geq \bot$$ le type de retour
- le problème est donc ramené à l'[unification](#unify) de deux types
  F sous un préfixe $$Q$$, avec les cas suivants:
    - types quantifiés: on skolemise en forme normale et on
      [unifie](#unify) + escape check
	- applications: on recurse en unifiant deux-à-deux les arguments
	- variable contre variable: [occurs check](#occurs-check) de chaque
      variable sur l'autre borne, calcul de la [borne
      supérieure](#unify-schemes) des bornes puis mise à jour du
      préfixe avec les bornes raffinées + substitution
	- variable contre type F:
	   - [occurs check](#occurs-check) pour éviter les types infinis
	   - on [subsume](#subsume) le type en la borne *i.e.* on cherche
         une instantiation de la borne qui s'unifie avec le type
	   - on met à jour le préfixe pour incorporer la substitution
- ca nous laisse donc avec les deux briques suivantes:
  - [borne supérieure](#unify-schemes) de deux types flexibles
  - [subsumer](#subsume) un type F en un type flexible
- borne supérieure de deux types flexibles:
  - si une borne est triviale, on renvoie l'autre
  - sinon $$\phi_i = \forall Q_i \rho_i$$ et on unifie $$\rho_0,
    \rho_1$$ sous le préfixe $$Q, Q_0, Q_1$$ (ce qui met à jour $$Q$$)
  - on quantifie l'unificateur $$\rho$$ selon les bornes dans $$Q_0,
    Q_1$$
- subsumption de $$\sigma$$ en $$\varphi$$:
  - instancie $$\varphi$$ en $$Q', \rho$$
  - skolémise $$\sigma$$ et unifie avec $$\rho$$ sous le préfixe $$Q, Q'$$
  - escape check sur les skolems
  - idem que la subsumption de types sigma, sauf que l'unification
    s'effectue sur un préfixe qui incorpore les bornes

ca donne l'allure générale de l'algo. il y a quelques aspects
techniques liés à la mise-à-jour du préfixe avec une substitution
et/ou une nouvelle borne pour maintenir les invariants.

# type rules 

## var

- trivial
- renvoie juste le type flexible selon le contexte

## let

- trivial
- infère le type de la variable (met à jour le préfixe)
- infère le type du corps avec le type de la variable dans le contexte (met à jour le préfixe)
- renvoie le type du corps avec le préfixe à jour

## abs

- infère le type flexible $$\varphi_1$$ du corps de l'abstraction en
  ajoutant le type du paramètre $$\alpha$$ au préfixe/contexte (borné
  trivialement)
- échec si on infère un type polymorphe pour le paramètre
- [`split`](#split) le préfixe mis à jour $$Q_1$$ entre la partie qui concerne le
  contexte ambiant $$Q_2$$ et le reste $$Q_3$$ (i.e. les bornes qui
  concernent le corps de l'abstraction)
  - normalement $$Q_3$$ devrait concerner uniquement $$\alpha$$ ?
- puisque le type inféré pour le type de retour est un type flexible,
  la seule manière de l'exprimer dans le type de la fonction c'est
  d'introduire une borne sur la variable correspondant au type de
  retour:
  - on [`extend`](#extend) $$Q_3$$ avec une borne $$\beta \geq
  \varphi_1$$ pour le type de retour, et quantifie sous ce préfixe le
  type de l'abstraction $$\alpha \to \beta$$.
  - en particulier le type retourné incorpore les bornes inférées
    pour le type du paramètre $$\alpha$$
	
## abs-ann

- idem que [abs](#abs) sauf qu'on passe directement $$x: \sigma$$ dans
  le contexte lors de l'inférence du corps de l'abstraction


## app

- infère le type de la fonction $$\varphi_1$$, met à jour le préfixe
- infère le type de l'argument $$\varphi_2$$, met à jour le préfixe
- [`extend`](#extend) le préfixe avec les bornes $$\alpha_1 \geq
  \varphi_1$$ et $$\alpha_2 \geq \varphi_2$$ (modulo substitution) +
  $$\beta \geq \bot$$ pour le type du résultat. ce préfixe est utilisé
  pour l'unification.
- [`unify`](#unify) $$\alpha_1$$ et $$\alpha_2 \to \beta$$ sous ce
    préfixe (le coeur du poulet)
- [`split`](#split) le préfixe résultant entre ce qui concerne le
  préfixe ambiant (qu'on mettra à jour en passant) et ce qui concerne
  purement cette application $$Q_5$$
    - normalement ce dernier préfixe devrait concerner uniquement
      $$\alpha_1, \alpha_2, \beta$$
- on quantifie le type de retour $$\beta$$ sous ce préfixe
  - en particulier le type de retour incorpore les bornes sur
    $$\alpha_1, \alpha_2$$

# functions 

## split

- sépare un contexte selon des variables: renvoie la partie qui
  concerne les variables, puis le reste
- les bornes sur les variables libres de la borne sur une variable
  vont dans la partie qui concerne la variable
- *i.e.* "toutes les bornes qui concernent une variable, possiblement transitivement"
- on peut se baser sur l'optimization habituelle selon le rang/level
  des variables pour le faire

## extend

- ajoute une borne à un préfixe en maintenant l'invariant d'avoir
  uniquement des bornes avec quantificateurs
- si la forme normale de la borne est un type sans quantificateur, on
  inline la borne via la substitution de retour
- sinon ajoute simplement la borne au contexte

## unify

- unifie deux types F sous un préfixe de bornes
- les deux types sont supposés en forme normale
- renvoie une substitution vers des types F ainsi qu'un préfixe
  mis-à-jour
  
----

- cas triviaux
  - variable avec elle-même
  - application d'un constructeur (on recurse sur les arguments deux-à-deux)
- variable bornée $$\alpha \geq \varphi$$ avec type $$\sigma$$
  - [`occurs-check`](#occurs-check) $$\alpha \notin \sigma$$
  - [`subsume`](#subsume) le type en la borne (*i.e.* essaie
    d'instantier la borne en le type)
  - [`update`](#update) le prefixe pour incorporer la substitution $$[\alpha := \sigma]$$
  
- variable $$\alpha_1 \geq \varphi_1$$ avec variable $$\alpha_2 \geq \varphi_2$$
  - [`occurs-check`](#occurs-check) $$\alpha_1$$ dans $$\varphi_2$$ et réciproquement
  - [`unify-schemes`](#unify-schemes) $$\varphi_1, \varphi_2$$ pour donner $$\varphi$$
  - [`update`](#update) le préfixe avec $$[\alpha_2 := \alpha_1]$$ et  $$\alpha_2 \geq \varphi$$
  
- type $$\forall \alpha.\sigma_1$$ avec type $$\forall \beta.\sigma_2$$
  - ajoute un skolem $$c$$
  - [`unify`](#unify) $$\sigma_1, \sigma_2$$ skolemisés avec $$c$$
  - verifie que le skolem ne s'échappe ni dans le préfixe ni dans la
    substitution
  - (suppose que les quantificateurs sont dans l'ordre standard)


## subsume

- essaie d'instancier un scheme $$\varphi=\forall Q_2.\rho_2$$ en un type
  $$\sigma=\forall \bar{\alpha}.\rho_1$$ sous un préfixe $$Q$$
- on suppose que $$Q_2$$ et $$Q$$ sont disjoints
- renvoie un préfixe et une substitution qui permettent l'instantiation

----

- skolemize $$\sigma$$ et instancie $$\varphi$$ (en ajoutant ses bornes au
  contexte), puis [`unify`](#unify) les deux (sous ce contexte)
- [`split`](#split) le préfixe et la substitution résultant pour ne garder que
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
- sinon $$\varphi_i = \forall Q_i.\rho_i$$, et alors on [`unify`](#unify) les types
  $$\rho_i$$ sous en prenant en compte *toutes* les bornes ($$Q, Q_i, \ldots$$)
- (comme d'hab) on [`split`](#split) le préfixe résultat selon ce qui concerne
  $$Q$$ et on quantifie le reste


## update

- met à jour un préfixe avec une substitution $$[\alpha := \sigma]$$
  ou une borne $$\alpha \geq \varphi$$
- maintient l'invariant sur le prefixe $$Q$$
- on suppose que le préfixe contient une borne sur $$\alpha \geq \hat{\varphi}_1$$
- on suppose que $$\alpha \notin ftv(\varphi)$$ (resp $$ftv(\sigma)$$)
  - l'appel est toujours protégé par un [`occurs-check`](#occurs-check)
- on suppose que $$\hat{\varphi_1} \leq \sigma$$ (resp) $$\hat{\varphi_1} \leq \varphi$$
  - l'appel suit [`subsume`](#subsume) (resp [`unify-schemes`](#unify-schemes))
  
---

- pour une substitution $$[\alpha := \sigma]$$
  - on [`split`](#split) selon les $$ftv(\sigma)$$
     - pas totalement clair pourquoi (efficacité?)
     - *i.e.* on s'interdit de toucher aux bornes qui concernent les
       variables libres dans $$\sigma$$
	 - de toute manière on voit mal comment on pourrait y toucher vu
       que $$\alpha$$ n'est pas dans les variables libres de
       $$\sigma$$
  - on vire la borne et on substitue les occurrences de $$\alpha$$
  
- pour une borne $$\alpha \geq \varphi$$
  - on [`split`](#split) selon les $$ftv(\varphi)$$
    - idem, pas totalement clair pourquoi
  - dans ce qui reste:
    - si $$\varphi$$ n'est pas quantifié, on vire la borne et on
      substitue pour maintenir l'invariant (de toute façon il sera
      impossible d'instantier encore cette borne)
    - sinon, on remplace simplement la borne existante par la nouvelle
      borne, qui par hypothèse est une instance de la précédente
	
---

- probablement on va pas tout substituer le préfixes splitté, mais
  simplement substituer les bornes à chaque fois qu'on en a besoin

## occurs-check

- attention: il faut aussi prendre en compte toutes les variables
  apparaissant dans les bornes du contexte qui concernent le type
  cible
  
--- 
  
- TODO






