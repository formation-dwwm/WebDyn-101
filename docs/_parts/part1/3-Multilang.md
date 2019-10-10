---
layout: step
title:  Multilingue
part: Implémentation Javascript
order: 3
---

## Avertissement

L'aspect multilingue d'un site ou d'une application web est un aspect important du développement web. Ce qui est présenté dans cette section donne **un aperçu** des développements multilingues, mais dans une approche très simplifiée et plutôt basique. <br/>
**La technique présentée ici n'est pas à utiliser en production en l'état**.

## Notion générale

L'idée de base d'un développement multilingue est de réaliser des applications qui pourront être présentées en plusieurs langues. Dans un développement dynamique complet, il y a typiquement au moins deux types de textes à traduire: le contenu, et les interfaces.

Les outils et techniques utilisées pour ces deux types de textes diffèrent, mais le principe reste le même: Proposer chacun de ces textes dans autant de versions que l'on aura de langages visés, dans un format qui permettra à l'ordinateur de passer d'une version à l'autre. Typiquement, constituer un `dictionnaire` des textes utilisés, avec leurs traductions dans les langues choisies.

Prenons un exemple avec deux langages, le Français et l'Anglais, et donnons deux termes à notre dictionnaire, TexteIntro et Presentation, qui seront en quelques sortes des abréviations, des symboles représentants les textes à traduire dans les deux langues.

Sur papier on pourrait avoir cette représentation :

```
TexteIntro:
  fr: Bonjour à tous
  en: Hello World

Presentation:
  fr: Ceci est mon premier programme multilingue
  en: This is my first multilingual program
```

De façon un peu plus juste, nous aurions en fait un dictionnaire par langue, définissant l'ensemble des termes utilisés. Par exemple ici deux dictionnaires :

```
// Fr
TextIntro: Bonjour à tous
Presentation: Ceci est mon premier programme multilingue
```
```
// En
TextIntro: Hello World
Presentation: This is my first multilingual program
```

Avec ce type de représentation accessible à nos programmes, nous pourrions alors créer un code HTML du type :

```html
<h1>TextIntro</h1>
<p>Presentation</p>
```

Et utiliser ensuite du code dynamique pour remplacer *dynamiquement* les termes utilisés dans le HTML par leur traduction dans le langage choisi par l'utilisateur.

## Dead Simple Implementation

Etendons ce que nous avons découvert avec le *Hello World* précédent pour s'essayer à la notion de multilingue. L'implémentation que nous allons faire est **simplement stupide**, et est loin de satisfaire de réels besoins de développement multilingues. Mais elle nous permettra d'aborder la notion tout en étendant notre compréhension du Web Dynamique à notre niveau.

### Structure

Commençons avec le code html suivant - je ne place que le corps de la page, soit le `<body>`, il faudra bien évidemment "habiller" ce corps :

```html
<header>
  <button onclick="setLang('fr')">FR</button>
  <button onclick="setLang('en')">EN</button>
</header>

<main id="dynamic-content">
  <h1>TextIntro</h1>
  <p>Presentation</p>
</main>

<script type="application/javascript">

</script>
```

### Données

Idéalement nous utiliserions des données présentes dans des fichiers séparés, voire même stockées d'autres façons, comme par exemple dans une base de données. Afin de simplifier la présentation, nous placerons ces données directement avec notre code. 

Nous allons commencer par créer nos dictionnaires. Afin de simplifier leur accès ensuite par Javascript, nous allons même créer un "dictionnaire de dictionnaires" comme vous allez le voir. Les `objets` dont nous avons parlé précédemment sont de parfaits candidats pour créer ces dictionnaires en Javascript. Chaque entrée de dictionnaire se traduira en fait par une `propriété`: 
- Le "super dico" aura deux entrées par exemple avec le Français et l'Anglais, soit deux `propriétés` "fr" et "en" par exemple, qui contiendront comme valeurs les deux dictionnaires Français et Anglais respectivement.
- Le dictionnaire Français aura deux `propriétés` "TextIntro" et "Presentation" et en valeurs leurs traductions associées
- Le dictionnaire Anglais aura deux `propriétés` "TextIntro" et "Presentation" et en valeurs leurs traductions associées

Traduisons ceci en Javascript, que vous pourrez inclure dans la balise `<script>` :

```javascript
const dict = {
  "fr": {
    "TextIntro": "Bonjour à tous",
    "Presentation": "Ceci est mon premier programme multilingue"
  },
  "en": {
    "TextIntro": "Hello World",
    "Presentation": "This is my first multilingual program"
  },
};
```

### Code

Il nous faut maintenant implémenter le code qui fera la "glue" entre la structure et nos données. Celui-ci sera un peu plus fourni que le précédent, et j'utiliserai plus les commentaires pour vous l'expliquer. 

Et déjà dans un premier temps, sachez que tout comme tous les `HTMLElement` ont tous une propriété `textContent` permettant de récupérer et modifier leur contenu textuel, ils ont aussi une propriété `innerHTML` permettant de récupérer et modifier leur contenu HTML directement, et donc d'y utiliser des `<balises>`.

Mais au delà du code et de l'implémentation, parlons déjà de l'`algorithme` que nous allons utiliser, avec nos propres termes.

- Nous allons déjà devoir garder une information en mémoire, le langage demandé par l'utilisateur. Nous l'appelerons `currentLanguage`.
- Il nous faudra ensuite une fonctionnalité permettant de mettre à jour le code HTML par sa version traduite dans le `currentLanguage`. Nous l'appellerons `translate`.
- Enfin, nous pourrons utiliser une fonctionnalité supplémentaire qui permettra à l'utilisateur de changer de langue. Nous l'appellerons `setLang`, et elle pourra être paramétrée par la nouvelle langue choisie par l'utilisateur, que nous dénommerons `newLang`.

**translate**

Afin d'effectuer la traduction de la page, nous passerons par les étapes suivantes :
- Obtenir le dictionnaire à utiliser en fonction de la valeur de `currentLanguage`
- Pour chaque `terme` présent dans le dictionnaire :
  - Récupérer sa traduction
  - Remplacer toutes les occurences du `terme` courant dans le code HTML initial par sa traduction
- Remplacer le contenu HTML d'un élément de notre page web par le nouveau code HTML généré.

**setLang**

Cette fonctionnalité est beaucoup plus simple. On lui donnera en paramètre une valeur `newLang`, que l'on utilisera pour modifier la valeur de `currentLanguage`. Il nous suffira ensuite d'appeler notre fonctionnalité `translate` pour finir de mettre à jour la page web.

En Javascript, cela pourrait s'écrire :

```javascript
/**
 *  Code applicatif
 *  Exécuté au chargement de la page
 */
// On récupère une référence vers notre élément HTML dont l'on veut
// pouvoir mettre à jour le contenu
const $main = document.getElementById("dynamic-content");
// On en profite pour garder immédiatement en mémoire l'état initial
// du code HTML à l'intérieur de cet élément. Ce sera le texte à traduire.
const initialHTML = $main.innerHTML;

// Stockage en mémoire du langage demandé par l'utilisateur. 
// Par défaut, nous choisirons le Français
let currentLanguage = "fr";

// Traduisons cette page dès son chargement !
translate();

/**
 *  Définition de nos fonctionnalités
 */
// Notre fonctionnalité `translate`, responsable de traduire
// le HTML initial et mettre à jour le contenu de notre
// élément dynamique
function translate(){
  // Récupération du dictionnaire adapté au langage demandé
  const langDict = dict[currentLanguage];
  // Constitution d'une liste des termes présents dans le dictionnaire
  const terms = Object.keys(langDict);
  // Création d'un emplacement mémoire pour le code HTML traduit
  // dans la langue choisie. On l'initialise avec la valeur
  // du code HTML initial stockée au chargement de la page
  let finalHTML = initialHTML;
  
  // Pour chaque terme présent dans le dictionnaire...
  for(const term in terms){
    // ... récupérer la traduction de ce terme
    const trad = langDict[term];
    // ... remplacer le terme par sa traduction dans le code html final
    finalHTML = finalHTML.replace(term, trad);
  }
  
  // Enfin, mettre à jour le contenu de notre élément dynamique
  // en utilisant le nouveau code HTML généré.
  $main.innerHTML = finalHTML;
}

// Notre fonctionnalité `setLang`, permettant à l'utilisateur de
// changer la langue utilisée par la page. Elle utilise un
// `paramètre` que l'on nomme ici `newLang` qui représente le
// nom du dictionnaire à utiliser
function setLang(newLang){
  // On commence par mettre à jour le langage demandé par l'utilisateur
  currentLanguage = newLang;
  // Puis on utilise notre fonctionnalité `translate`
  translate();
}
```

### Démos

L'implémentation présentée est disponible dans le [CodePen](https://codepen.io) suivant :

[DeadSimpleI18n-replace](https://codepen.io/Gatthias/pen/JjjYJGe)

Et j'ajoute une variante utilisant une autre approche pour identifier et remplacer les termes à traduire. Elle a pour avantage d'éviter des interférences avec le code HTML - imaginez par exemple si l'on utilisait un terme nommé "h1" dans notre dictionnaire... Et allez plus loin, essayez et comparez !

[DeadSimpleI18n](https://codepen.io/Gatthias/pen/YzzyZLx)