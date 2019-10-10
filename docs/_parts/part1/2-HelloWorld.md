---
layout: step
title:  Hello World
part: Implémentation Javascript
order: 2
---

## Premiers pas

Partons du code HTML statique suivant :

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello Dynamic Web</title>
  </head>
  <body>
    <h2>Hello...</h2>

    <p id="dynamic-content">
      <!-- Le contenu dynamique sera inséré ici par le code Javascript -->
      ???
    </p>

    <script type="application/javascript">
      // Le code Javascript sera inséré ci-dessous par le développeur
    </script>
  </body>
</html>
```

Tout texte ajouté entre les deux balises `<script>` sera considéré comme étant du `Javascript`, et sera interpreté par le navigateur au chargement de la page.
Si nous voulons modifier le contenu de notre balise `<p>`, nous allons devoir procéder en deux étapes :
1. Sélectionner l'élément html de notre document ayant l'identifiant `dynamic-content`
2. Remplacer le contenu textuel de cet élement

En Javascript, un élément html est représenté par un `objet` du type `HTMLElement`. Pour faire "simple", ce sont des valeurs utilisables par Javascript et représentant l'ensemble des élements de notre `Document` (chaque balise html a son élément js associé). Ces `objets` sont des valeurs spéciales qui présentent diverses `propriétés`, tout comme un `objet` *crayon* pourrait avoir des `propriétés` *taille* et *couleur* par exemple.<br/>
On peut accéder à ces propriétés avec la notation `.`, soit pour notre exemple parler de `crayon.couleur` ou de `crayon.taille`.

Chaque type de balise html a en réalité un type d'objet Javascript particulier associé, mais toutes présentent la propriété `textContent`, représentant leur contenu textuel. C'est cette propriété que l'on va utiliser pour notre point *2*.

Il nous reste à savoir comment "sélectionner" l'élément html voulu. Nous venons de voir que Javascript avait en mémoire une représentation de l'ensemble des éléments html de notre `Document`. Et bien de la même façon, nous allons pouvoir accéder à une représentation de ce même `Document`, par le mot clef tout à propos `document`. Ce `document` est un autre type d'objet, qui nous offre son propre lot de `propriétés`.

Parmi ces `propriétés` se trouvent quelque-unes particulières que l'on nomme `fonctions`. On les dinstingue des autres en ce qu'elles vont représenter une `action`, et non une `valeur`.<br/>
On ne va pas chercher à *récupérer* ou *modifier* les `valeurs` de ces `propriétés`, mais nous dirons que nous *appellons* ces `fonctions` afin de déclancher des `actions` ou des `procédures`.

Enfin, parmi ces `fonctions`, certaines sont utilisées pour récupérer des `valeurs`, par exemple en réalisant le calcul de celles-ci avant de `retourner` le résultat. On parle alors de `valeur de retour`, et c'est ce que nous allons utiliser pour notre point *1* grace à la fonction bien nommée `getElementById` proposée par le `document`.

Traduisons tout ce Français en Javascript :

```javascript
// 1. Sélectionner l'élément html de notre document ayant l'identifiant `dynamic-content`
var $paragraph = document.getElementById("dynamic-content");
// 2. Remplacer le contenu textuel de cet élement
$paragraph.textContent = "World !";
```

Insérez ce code dans la balise `<script>` sous le commentaire `// Le code Javascript sera inséré ci-dessous par le développeur`, et rafraîchissez votre page. Le texte devrait s'afficher correctement.

> Si cela ne fonctionne pas, si vous avez des erreurs, vérifiez bien l'ensemble de votre syntaxe, ainsi que l'orthographe de 'dynamic-content' des deux cotés Javascript et HTML.
> 
> Si le problème persiste, le code complet est présent en bas de cette page.


C'est donc un premier exemple de **code dynamique**, dans lequel le contenu n'est pas présent directement dans le code (du moins ici, hors du HTML principal...).

## Refactoring

Une grosse partie du travail d'un développeur dynamique est de rendre flexible le code d'une application. Si nous avons régulièrement à effectuer un ensemble d'actions identiques, nous allons chercher à **éviter la répétition (DRY)** en `factorisant` une partie de ces actions dans des `procédures`, des `fonctions`.

Si nous souhaitons rendre l'action de modifier le texte de ce paragraphe plus facilement reproductible, nous pouvons en faire une `fonction`. Nous sommes maîtres des choix que nous allons faire, mais nous pouvons déjà estimer que si nous souhaitons répéter ce comportement, c'est probablement que nous voudrons pouvoir l'utiliser pour pouvoir afficher des *contenus différents*. Le *contenu*, soit le texte à afficher, ne doit donc pas faire partie de la `fonction` en elle même, mais devra être fourni à celle-ci afin de voir ce-même contenu s'afficher au bon endroit. C'est la notion de `paramètre`, ou d'`argument` de `fonction`.

Pour faire une analogie, votre téléphone possède une `fonction` "passer un appel", que d'autres développeurs auront nécéssairement créée bien avant que vous ne puissiez même passer vos appels avec cet appareil. Et c'est ensuite vous qui lui donnez en `paramètre` le numéro à appeler.

Réécrivons notre code précédent en gardant le même résultat final, c'est ce que l'on appelle le `refactoring` du code. Vous apprendrez à aimer ces étapes, car ce sera souvent le plus gros de votre travail.

Plus précisément ici, créons une `fonction` *setParagraph*, que nous `appellerons` en lui fournissant la valeur *"World !"* en `argument`. Cette fonction utilisera directement le code que nous avons écrit plus tôt, mais de façon `paramétrée`. Cela pourrait s'écrire comme suit :

```javascript
// A. Définition de la fonction
function setParagraph(newContent){
  // 1. Sélectionner l'élément html de notre document ayant l'identifiant `dynamic-content`
  var $paragraph = document.getElementById("dynamic-content");
  // 2. Remplacer le contenu textuel de cet élement, par la valeur passée en argument
  $paragraph.textContent = newContent;
}

// B. Appel de la fonction
setParagraph("World !");
```

En replaçant le code Javascript précédent par celui-ci, vous devriez obtenir le même résultat.<br/>
"A quoi bon" me demanderez vous ? Oui, nous avons gardé le même résultat. Mais nous avons énormément gagné en flexibilité !

Pour vous en rendre compte, ajoutez le code suivant dans le `body` de votre document html :

```html
<button onclick="setParagraph('World !')">World</button>
<button onclick="setParagraph('Dynamic Web !')">Dynamic Web</button>
```

Rafraîchissez votre page, puis cliquez sur les boutons. Voilà "à quoi bon" ;)

> Ca ne fonctionne pas ? Vérifiez bien (deux, trois, ?? fois) la syntaxe utilisée, l'exactitude des noms utilisés, et méfiez vous des intéractions entre guillemets simples et doubles.
> 
> Le code complet est disponible ci-dessous en cas de besoin.

**Code complet**

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello Dynamic Web</title>
  </head>
  <body>
    <h2>Hello...</h2>

    <button onclick="setParagraph('World !')">World</button>
    <button onclick="setParagraph('Dynamic Web !')">Dynamic Web</button>

    <p id="dynamic-content">
      <!-- Le contenu dynamique sera inséré ici -->
      ???
    </p>

    <script type="application/javascript">
      // Le code Javascript sera inséré ci-dessous par le développeur
      // A. Définition d'une fonction pour changer le texte du paragraphe dans le document html
      function setParagraph(newContent){
        // 1. Sélectionner l'élément html de notre document ayant l'identifiant `dynamic-content`
        var $paragraph = document.getElementById("dynamic-content");
        // 2. Remplacer le contenu textuel de cet élement
        $paragraph.textContent = newContent;
      }

      // B. Appel de la fonction au chargement de la page
      setParagraph("World !");
    </script>
  </body>
</html>
```
