# Sass

Liste des fonctionnalités Sass.

# Les variables
Nous allons pouvoir stocker une valeur dans une variable et pouvoir la réutiliser partout dans la suite de notre code.

#### CSS :
```css
h1{
  color: red;
}
p{
  color: red;
}
```

#### SCSS :
```scss
// Création d'une variable color
$color : red;

// Utilisation de la variable color
h1{
  color: $color;
}
p{
  color: $color;
}
```
# Concaténation


## Signe "&"
Sass a un signe spécifique pour concaténer les sélecteurs parent et enfant : l’esperluette (&).

Pour les pseudosélecteurs (ex: hover, before, etc)

#### SCSS :
```scss
.button {
  background: red;
  &:hover{
    background: blue;
  }
  &:before{
    content: ">";
  }
}
```
#### CSS : 

```css
.button{
  background: red;
}
.button:hover {
   background: blue;
}
.button:before {
   content: ">";
}
```

### Pour la méthode BEM 

#### SCSS :

```scss
.c-product {
  padding : 0.5rem;
  &__title {
    text-transform: uppercase;
  }
  &__desc{
    margin-top: 1rem;
  }
}
```
#### CSS : 

```css
.c-product{
  padding : 0.5rem;
}
.c-product__title {
   text-transform: uppercase;
}
.c-product__desc {
   margin-top: 1rem;
}
```

### Utilisation avancée du signe "&"
On peut utiliser le signe "&" pour concaténer le sélecteur parent après le sélecteur sur lequel nous sommes en train de travailler.

Un exemple très utile : Le darkmode

#### HTML : 
```HTML
<html class="dark-mode">
...
  <section class="page">
    <button class="button">Button</button>
  </section>
...
</html>
```

#### SCSS :
```scss
.button {
  background : black;
  .dark-mode &{ 
    background : white; 
  }
}
.page {
  background : white;
  .dark-mode &{ 
    background : black; 
  }
}
```
#### CSS :
```css
.button {
  background : black;
 }
.dark-mode .button{ 
  background : white; 
}

.page {
  background : white;
 }
.dark-mode .page{ 
  background : black; 
}

```




On peut stocker le signe "&" dans une variable pour pouvoir l'utiliser en dehors du "scope" du sélecteur parent

#### SCSS :

```scss
.c-product {
  $this: &;
  &__title {
    background: red;
  }
  &:hover{
    #{$this}__title{
      background: blue;
    }
  }
}
```

#### CSS :

```css
.c-product__title {
  background: red;
}
.c-product:hover .c-product__title{
  background: blue;
}

```




## signe ">", "+", "~", etc
```scss
.parent {
  > .enfant {
    padding: 1rem;
  }
}
.element {
  + suivant{
    padding: 1rem;
  }
}
```
#### CSS : 

```css
.parent > .enfant {
  padding: 1rem;
}
```




# Découpage des fichiers
Sass va nous permettre de découper notre code CSS en plusieurs fichiers. Cette fonctionnalité est très utile car nous pourrons créer des fichiers pour un composant en particulier.

#### SCSS : 
```scss
// /components/_product.scss
.c-product {
  padding: 0.5rem;
  &__title {
    margin-top: 1rem;
  }
}
```

```scss
// /components/_nav.scss
.c-nav {
  display: flex;
  list-style: none;
  &__item {
    padding: 0.5rem;
  }
}
```

```scss
// main.scss
@import "/components/nav";
@import "/components/product";
```
Attention, l'ordre des import est très important pour utiliser des variables ou des fonctions. Vous devez impérativement les importer avant le reste.

# Mixins
Les variables nous permettent de stocker une valeur réutilisables dans notre code. Mais quelques fois nous aimerions pouvoir stocker plusieurs attributs et valeurs pour les réutiliser. C'est le rôle du "Mixin".

### Définir un mixin avec "@mixin"

#### SCSS :
```scss
@mixin title {
  font-weight: bold;
  text-transform: uppercase;
}
```

### Utilisation d'un mixin avec @include

#### SCSS :
```scss
.title {
  font-size: 1rem;
  @include title();
}

.sous-titre {
  font-size: 0.8rem;
  @include title();
}
```
#### CSS :
```css
.title {
  font-size: 1rem;
  font-weight: bold;
  text-transform: uppercase;
}

.sous-titre {
  font-size: 0.8rem;
  font-weight: bold;
  text-transform: uppercase;
}
```

### Utilisation d'un mixin avec du contenu dynamique grace à "@content"
Un mixin permet de stocker plusieurs attributs mais il permet également d'ajouter du contenu dynamique. Très utile pour les mediaqueries, par exemple.

#### SCSS :
```scss
@mixin small-up {
   @media all and (min-width: 640px) {
      @content;
   }
}

.title{
  font-size: 1rem;
  @include small-up(){
    font-size: 2rem;
  }
}
.text {
 font-size: 0.8rem;
  @include small-up(){
    font-size: 1.6rem;
  }
}
```

```css
.title{
  font-size: 1rem;
}
@media all and (min-width: 640px) {
  .title{
    font-size: 2rem;
  }
}
.text{
  font-size: 0.8rem;
}
@media all and (min-width: 640px) {
  .text{
    font-size: 1.6rem;
  }
}
```
