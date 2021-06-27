---
title: HML
categories: [prog]
---

([article](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/hml.pdf))

{% include toc.md %}

# notes

- on a en plus des types $$\sigma$$ de System-F des *types flexibles*
  $$\varphi$$ qui correspondent à des types $$\sigma$$ avec des bornes
  sur les quantificateurs. *les bornes sont elle-mêmes des types
  flexibles.*
  - l'existence de types principaux assure que chaque nouvelle
    utilisation (via le contexte) est nécéssairement une instance du
    type inféré jusqu'ici (ou une erreur de type)
  - les $$\sigma$$-types sont insuffisants dans le cas général pour
	avoir des types principaux en présence de types de rang supérieur:
	on peut donner à `(single id)` les deux types $$\forall
	\alpha.[\alpha \to \alpha]$$ et $$[\forall \alpha. \alpha \to
	\alpha]$$ sans qu'aucun ne soit une instance de l'autre au sens de
	System-F.
  - en revanche ces deux types sont instances du type flexible
    $$\forall (\beta \geq \forall \alpha \to \alpha).[\beta]$$
  - TODO est-ce que les types flexibles forment un genre de (semi-)treillis?
- on a comme d'habitude un *contexte* $$\Gamma$$ qui associe une
  variable à son type flexible $$\varphi$$
- on a également un *préfixe* $$Q$$ qui contient pour toutes les
  variables libres du contexte la borne la plus spécifique inférée à
  ce stade sur cette variable (i.e. cette variable devra être
  instanciée avec un type qui sera une instance de la borne)
  - pour éviter des problèmes (5.4) on maintient l'invariant que
	toutes les bornes doivent être des types quantifiés
	- ca permet d'éviter que l'équivalence entre types soit dépendante
      d'un préfixe, car sinon on pourrait "cacher" des types
      polymorphes dans les bornes et se retrouver avec des types
      d'arguments non-annotés équivalents à des types polymorphes
	- si l'équivalence se fait sans référer au préfixe, les variables
      libres n'ont plus aucun moyen de se retrouver équivalentes à des
      types polymorphes
	- pour ca il suffit d'empêcher que les bornes soient des types
      non-quantifiés, car c'est la règle qui élimine ces bornes qui
      est dépendante du préfixe (et c'est la seule)
  - les types dans les bornes sont dénotés par des types chapeaux:
    $$\hat{\varphi}$$
  - de toute manière si une borne ne contient pas de quantificateur on
    peut l'inliner dans la substituion (il existe une seule
    instantiation possible, de fait la plus générale)
  - donc en pratique, plutôt que d'avoir des types non-quantifiés dans
    les bornes on peut tout aussi bien substituer le type
    non-quantifié dans le préfixe
  - au final ca semble surtout une simplification bien pratique
- en fonction de l'utilisation des variables libres on devra parfois
  raffiner les bornes, donc mettre à jour le préfixe qui sera retourné
  par l'inférence
  - ca correspond à une monade `reader` sur le contexte et `state` sur le
	préfixe/substitution
 - on a également une opération pour quantifier *"sous un préfixe"*,
   qui quantifie un les variables libres d'un type en y incorporant
   les meilleures bornes connues d'après le préfixe.

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






