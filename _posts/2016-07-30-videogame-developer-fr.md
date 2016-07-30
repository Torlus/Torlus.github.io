---
layout: post
title: "Dans la peau d'un développeur de jeux vidéo [FR]"
description: "Récit de quelques jours passés au sein d'un studio de développement de jeux vidéo"
category:
tags: []
---
{% include JB/setup %}

# Preambule

Il y a de cela une semaine, j'ai pu travailler quelques jours en tant que développeur au sein d'un studio de développement de jeux vidéo.

La concurrence est féroce dans l'industrie du jeu vidéo, et j'ai dû signer un accord de confidentialité pour cette mission particulière.
En conséquence de quoi, je ne donnerai pas d'informations ni sur la société, ni sur les personnes impliquées, qui pour la peine, seront affublées de noms ridicules. Tout ce que je peux dire, c'est que le titre en cours de développement est de premier ordre, et sortira normalement avant la fin de cette année sur PC/Windows, Xbox One et PlayStation 4.

Tout est parti d'une discussion avec mon vieil ami *Heckle*, responsable de l'équipe de développement, qui m'a fait part de problèmes de performances. Après des semaines passées à optimiser les fonctions les plus coûteuses, l'état du code actuel était selon ses propres termes: *uniformément lent*. Il avait néanmoins repéré quelques pistes d'amélioration, et m'a demandé de les étudier.

Grosso modo, j'avais quelques jours pour voir comment je pouvais optimiser des choses sur une base de code de *1,6 millions* de lignes de  **C++** (pour le moteur principal uniquement, sans compter les divers middleware et autres frameworks) qui était le résultat de 2 ans de développement réalisés par une équipe de plus de 50 personnes...

![barney](/images/barney.jpg)

# Dans le collimateur

*Heckle* avait remarqué qu'une famille de fonctions réalisant des *interpolations numériques* étaient utilisés à de nombreux emplacements dans le code, mais en particulier au niveau du *générateur de particules*. Il me montre vite fait comment utiliser un outil de *profiling* fait maison, qui peut entre autres mesurer le nombre d'appels à des fonctions données. Après quelques tentatives, nous avons mesuré que *ces fonctions d'interpolation étaient appelées plus de 10.000 fois par trame*.

Etant persuadés qu'il y avait matière à creuser le sujet, *Heckle* et moi avons commencé à regarder comment celles-ci étaient implémentées. Et c'est à ce moment précis que nous avons réalisé que notre "double facepalm" inaugural serait le premier d'une longue série...

Toutes ces fonctions étaient génériques, avec utilisation massive de *templates*. Chaque template avait au minimum 3 paramètres (type de la valeur de retour, type de *courbe*, stratégie d'allocation mémoire). Les *courbes* étaient (bien entendu) implémentées sous forme d'une classe générique avec templates, qui elles-mêmes utilisaient des instances de classe de type *liste*...
Et vous avez déjà deviné comment sont implémentées ces listes, n'est-ce pas?

Ironie du sort, l'abstraction (qui aurait pu être intéressante dans ce cas, selon moi) que pourrait proposer le type *liste* tourne court: la seule implémentation d'une *liste* proposée est sous forme d'un tableau de valeurs. Et certaines fonctions *publiques*, telles "donne-moi un pointeur vers le tableau utilisé en interne" sonnent le glas d'une ouverture potentielle à d'autres implémentations, telle une liste chaînée, car la probabilité que plus rien ne fonctionne est extrèmement haute.

Pour résumer à quoi on a affaire: du *meta-programming* à haute dose de templates, de la *redéfinition d'opérateurs*, des *directives de compilation* telles *reinterpret_cast* un peu partout, et une série de *macros* de haute voltige syntaxique à destination du *préprocesseur* , afin "d'améliorer la lisibilité" de tout l'ensemble. Mon estomac commençait à faire des tours sur lui-même...

Je prends une longue et profonde inspiration, et je plonge dans ce code.

![cppmp](/images/cppmp.jpg) ![aspirin](/images/aspirin.jpg)

> Choisir avec soin ses armes...

# S'intéresser au "Pourquoi", plutôt qu'au "Comment"

Cette dernière phrase résume assez bien ma stratégie, et c'est que je réponds à ceux qui me demandent des informations sur la méthodologie et les techniques que j'emploie pour faire du reverse-engineering.

Dans ce cas, je ne cherche donc pas pour le moment à optimiser ce code, je cherche plutôt à savoir "qui" l'utilise, et "pourquoi".

L'utilisateur principal de ces fonctions est le *générateur de particules*, ce qui n'est pas une surprise en soi.

Je me suis donc mis dans la peau d'un développeur qui aurait eu à écrire ce générateur, et j'en suis arrivé au schéma suivant:

- Les particules sont générées (en grand nombre) à partir d'un événement donné.
- Quelques passages par le *moteur physique* permet de générer des *points* clés, que l'on stocke dans une *courbe*.
- Et on finit par utiliser les *fonctions d'interpolation*, dans l'espoir qu'elles soient moins gourmandes en ressources que le *moteur physique*.

A première vue cela semble une approche raisonnable, mais après réflexion je me rends compte d'un potentiel problème de fond: *le calcul d'une trajectoire de particule soumise à la gravité est parfaitement simple et déterministe*.

Pourquoi s'encombrer de *courbes de Bézier*, *B-splines* ou encore de *NURBS*, *Catmull-Rom splines* d'ordres cubiques et au-delà, alors que la trajectoire suivie par la grande majorité des particules est *parabolique*?

Une autre bizarreté qui a retenu mon attention est la répartition du type de valeurs utilisées dans les templates: je m'attendais à une utilisation massive de *vec3* et *vec4* mais ce n'était pas le cas... La plupart des appels étaient effectués avec des *floats*!

Après un temps de réflexion dubitative, je sollicite un entretien avec *Jeckle*, l'utilisateur principal de ces fonctions.

# Know your data

*Jeckle* m'a apporté de précieux éclaircissements sur l'utilisation de ces fonctions d'interpolation:

- Elles sont appelables (et appelées) partout dans le code du jeu.
- Elles sont utilisées pour des usages très différents, certains n'étant pas liés à des calculss de trajectoire, par exemple la *génération de dégradés de couleur*.
- Pour de la physique triviale, comme des particules ou objets soumis à la gravité, alors seul l'axe des Z a besoin d'interpolation, ce qui explique l'utilisation massive de *floats* (je ne l'avais pas vu venir, celle-là, je me suis fait avoir, ça m'apprendra à sous-estimer les gens).

Mais ce qui m'a mis la puce à l'oreille, est l'explication qu'il a fournie sur le cycle de vie des particules: au fur et à mesure du développement du jeu, les interactions sont devenues de plus en plus complexes. De ce fait, le générateur de particules devait s'appuyer de plus en plus sur le moteur physique, car le précalcul des trajectoires était de moins en moins possible, et donc que le nombre de points utilisables de manière "sereine" diminuait.

Je me précipite donc sur mon clavier, je rajoute des logs vite fait pour mesurer la longueur (le nombre de points) des courbes passées en paramètre des fonctions d'interpolation, fais tourner le jeu pendant quelques trames et... ma machoire tomba par terre.

**Plus de 90% des courbes contenaient un seul et unique point**.

Un. Seul. Point.

![picard-wtf](/images/picard-wtf.jpg)

# Que le Forth soit avec toi

Pour résumer, la bonne nouvelle est que le nombre de calculs effectués pour rien est un faux problème. La mauvaise nouvelle est que du coup, aucun progrès n'a été accompli dans le sens de l'optimisation.

A ce momemt, je me suis rappelé que les *courbes* utilisaient au moins une *liste* (deux, lorsque l'interpolation des tangentes était demandée).

En terme de stockage de données, la classe *liste* est composée d'une partie *statique* (stockage d'informations telles la longueur, l'utilisation mémoire réelle, le dernier index utilisé, etc.) et d'une partie *dynamique* (les éléments de la liste).
La gestion de la partie dynamique est lourde, car conçue pour accueillir un grand nombre d'éléments, ce qui implique l'utilisation d'algorithmes complexes dont le but est de gérer efficacement l'allocation mémoire, et réduire la fragmentation. On désigne ces algorithmes et leur implémentation sous le terme *allocateur*.

L'allocation mémoire est un sujet difficile, rendu particulièrement ardu dans un environnement multiprocesseur, où chaque coeur accède de manière concurrente à la RAM, qui est une ressource partagée.

D'un coup, je me suis souvenu (pourquoi? comment? aucune idée...) d'une technique d'optimisation utilisée dans de multiples implémentations du *langage de programmation Forth*: dédier un registre du processeur pour stocker le premier élément de la pile de données. On appelle alors ce registre le "*Top-of-stack* register". Pour en savoir plus, consulter les [publications "Moving Forth" de Bradford Rodriguez](http://www.bradrodriguez.com/papers/moving1.htm).

J'attaque donc l'implémentation d'une nouvelle *liste*, dont le *premier élément* est stocké dans la partie *statique*, en lieu et place de la partie *dynamique*, ce qui permet de ne réaliser l'allocation que quand un deuxième élément se présente. Il n'était pas nécessaire de réimplémenter toutes les méthodes dans ce cas, car l'utilisation était prédictible: quelques appels à *Append*, suivis d'accès en lecture au tableau (via la redéfinition *operator[]*) et pour finir quelques appels à *Clear*.

# Conclusion

Je n'ai pas réussi à tester mon code dans le temps imparti, ayant recontré des problèmes dans le "build". J'espère cependant que les pistes que j'ai explorées donneront des résultats utiles.

Préalablement à ma venue, *Heckle* m'avait dressé un tableau un peu étrange de ce qu'était la vie d'un studio de jeu vidéo. Il s'avère que finalement, il exagérait un peu. :)

Tout d'abord, je m'attendais à beaucoup plus de "code smell" (code de qualité inférieure). L'équipe est contituée de développeurs passionnés et talentueux, et le nombre de jurons à la minute est étonnament faible pour une équipe constituée à 95% d'hommes plutôt jeunes, informaticiens de leur état, regroupés dans un grand open-space.

Je m'attendais à une organisation approximative et un manque de procédures, et une fois encore, je me suis trompé. L'outillage mis en place est impressionnant, depuis les outils d'analyse très fine fournis par les constructeurs, jusqu'aux outils développés en interne, on y trouve un système de gestion de builds automatiques, distribués, et monitorés, ainsi que tout l'arsenal professionnel couvrant la gestion de code source, les tests, "issue management" et j'en passe. Tous les voyants sont au vert.

La communication entre les membres de l'équipe est excellente, avec un sens du respect et des responsabilités auquel je ne m'attendais pas. Les réunions sont courtes, et leur nombre est restreint. Peu de niveaux et de poids hiérarchiques. C'est une équipe agile dans son essence même, qui pratique l'agilité naturellement, sans idéologie, sans tambours ni trompettes.

Cette expérience m'a régalé, et j'espère pouvoir la réitérer un jour, sur une période plus longue.

Un grand merci à *Heckle* pour cette expérience enrichissante.

> Message personnel: cet article t'est dédicacé, Oriane. Avec toute mon amitié.
