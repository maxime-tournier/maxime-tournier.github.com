---
title: HMF
categories: [prog]
---


## var

- trivial
- renvoie juste le type selon le contexte

## let

- trivial
- infère le type de la variable
- infère le type du corps avec le type de la variable dans le contexte
- renvoie le type du corps avec le préfixe à jour

## abs

- obtient une variable fraiche $$\alpha$$ pour le type du paramètre
- infère le type du corps avec le contexte mis à jour avec le paramètre
- instancie le type du corps $$\sigma$$ en $$\rho$$
- `generalize` $$\alpha \to \rho$$ et retourne
  - (remonte les quantificateurs en position positive dans les fonctions)

## abs-ann

- idem sauf qu'on passe directement $$x: \sigma$$ lors de l'inférence du
  corps de l'abstraction

## app

- toute la viande est là
- infère le type de la fonction, instancie en $$\rho$$
- matche $$\rho = \sigma_e \to \sigma$$
  - normalement on a déjà $$\sigma = \rho_2$$ car les quantificateurs
    sont remontés (par contre attention aux annotations utilisateur
    qui ne respectent pas forcément cela)
- infère le type de l'argument $$\sigma_o$$
- `subsume` $$\sigma_e$$ (expected) en $$\sigma_o$$ (offered)
   - *i.e.* trouve une substitution la plus générale possible telle
     que $$\theta \sigma_o \leq \theta \sigma_e$$
- `split` la substitution résultat en mono/polymorphe et vérifie que
  la substitution polymorphe ne concerne aucune variable du contexte
  - alternativement, on peut vérifier que le type de l'argument dans
    `abs` est monomorphe, sans splitter la substitution
- `generalize` le type de retour et retourne
	
## subsume

- instancie le type $$\sigma_o$$ pour matcher $$\sigma_e$$
- suppose que les deux arguments sont en forme normale
  (quantificateurs dans l'ordre d'apparition des variables)
- skolemize $$\sigma_e$$ en $$\rho_e$$
- instancie $$\sigma_o$$ en $$\rho_o$$
- `unify` $$\rho_e$$ et $$\rho_o$$
- erreur si la substitution résultat (sauf les variables instanciées
  pour $$\rho_o$$) continent des skolems
  - on doit pouvoir améliorer en ayant des variables qui peuvent ou
    non unifier avec des skolems

## unify

- on suppose que les arguments sont en forme normale

- variable avec elle-même: trivial
- deux applications de types: trivial
- variable avec type
  - occurs check
- deux types quantifiés
  - skolémise les deux types avec le *même* skolem
  - `unify` les deux types skolémisés
  - erreur si la substitution continent le skolem
    - possibilité d'optimisation (cf plus haut)

