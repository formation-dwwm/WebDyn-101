---
layout: home
list_title: Cheminement
---

## Introduction au Web Dynamique

Afin d'introduire la notion, faisons déjà la distinction suivante afin de mieux caractériser ce que l'on appelle un fonctionnement "dynamique".
- Dans un développement statique, si le contenu d'une page web doit être changé, c'est directement son code HTML qui doit être edité, ce qui requiert un développeur.
- Dans un développement dynamique, le contenu des pages est initialement séparé des documents HTML créés par le développeur. Ce contenu est en fait enregistré dans un format particulier, et pourra être modifié avec les outils adaptés, rendant possible son édition sans connaissances aucunes en développement.

C'est en fait à nouveau une notion de **séparation des responsabilités** qui est en oeuvre ici.
Souvenez vous, pour constituer nos pages web, nous avons :

> - Le Document HTML, responsable de la structure (le squelette et la chair)
> - Le Style CSS, responsable de la présentation (la peau, cheveux, ...)<br>
> Le **navigateur** pour faire la *glue* entre les deux.

Nous pouvons voir la notion de web dynamique de la même façon :

> - Le HTML **statique**, responsable de la structure (le squelette)(le moule)
> - Les données, responsables du contenu (la chair)(la pâte)<br>
> Le **code dynamique** pour faire la *glue* entre les deux.

Nous verrons au fur et à mesure de notre formation et de nos carrières la multitude (*une* multitude...) d'implémentations possible de cette idée très générique. En formation, nous le verrons notament à travers des langages dits de *templating* (Jekyll, Twig, Blade), ainsi que des langages dits *procéduraux* (PHP, Javascript, Typescript).

Ce cours présentera graduellement différents exemples dans quelques-uns de ces langages.