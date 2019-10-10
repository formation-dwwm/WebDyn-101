---
layout: step
title:  Contexte d'exécution
part: Implémentation Javascript
order: 1
---

Dans cette série nous allons découvrir les notions du Web Dynamique en utilisant du code Javascript.

Parmi les langages présentés en formation pour le Web Dynamique, c'est historiquement le dernier à avoir eu réellement la capacité à remplir ce rôle.

Du fait que le code Javascript s'exécute chez le `client`, c'est un cas de figure un peu particulier du `Web Dynamique`, représentatif de ce que l'on nomme souvent le `Web 2.0`. Dans la plupart des autres contextes d'exécution, c'est le `serveur web` qui génèrera la page html.

Vous verrez que les techniques à employer sur les machines `clients` sont donc par certains traits fondamentalement différentes que celles utilisées sur les `serveurs`.

En effet, étant le `client` et donc dans un `navigateur`, nous commencerons toujours par recevoir un document HTML, associé à un programme Javascript. C'est au chargement du document que le script sera exécuté. Le contexte est donc fondamentalement différent de l'exécution côté `serveur` qui lui est justement chargé de générer ce même code HTML que nous `clients` reçevons. 

Ce n'est donc pas le langage typique du développement `Web Dynamique` à proprement parler, mais il nous permettra d'aborder les notions générales de celui-ci, sans avoir à développer de connaissances supplémentaires sur les `serveurs` au préalable.

Pour l'ensemble des exemples présentés dans cette section, vous pourrez travailler comme vous l'avez fait jusqu'ici: A partir d'un fichier html sur votre disque dur, à ouvrir avec le navigateur.