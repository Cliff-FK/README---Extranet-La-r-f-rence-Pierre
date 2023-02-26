# Extranet -La référence Pierre 

Ce dépôt une base de départ pour des projets HTML / CSS et JS à destination de développeurs.
Les fichiers sont configurés pour fonctionner dans un environnement axé sur les outils Gulp, Sass et Nunjucks. **A défaut, le dossier /public contient la version finale et peut-être utilisé pour en extraire les éléments souhaités**.

- **Ressources**:
- Blendid! : https://github.com/vigetlabs/blendid
- Bootstrap : https://getbootstrap.com/docs/4.3/getting-started/introduction/
- poppers : https://popper.js.org/tooltip-documentation.html (utilisé par bootstrap)
- Nunjucks : https://mozilla.github.io/nunjucks/fr/getting-started.html
- Boostrap-slider : https://github.com/seiyria/bootstrap-slider
- jQuery : https://api.jquery.com


##  :page_facing_up: Architectures des dossiers

  - `/public` : contient les fichiers compilés finaux. Les fichiers sont optimisés (concaténation, minification, gzip).
  - `/src` : contient les fichiers de développement. Les fichiers sont découpés par type (html/css-js). Le html est découpé en différents templates de pages, modules et composants.
  - Les autres fichiers et dossier sont utilisé en développement par le starter kit "Blendid".
- Les fichiers css finaux (dans le dossier `/public`), sont compilé en un unique fichier app.css. Celui-ci comprend donc le css personnalisé by Indexel, plus ceux de Boostrap. En développement (dossier `/src`), les fichiers css et scss sont découpés en différents modules ci-après.

  
### 1- Fichiers HTML
`src/html/layouts` 
- **Pages** :  Les templates de pages html. Elles se construisent en plugant différents modules dessus.
- **Layouts** : Contient les éléments utilisés en templating, découpés ainsi:
  - `card-`  : Modules qui prennent principalement l'apparence de "carte" ou "tuile".
  - `table-` : Regroupent les différents tableaux de l'extranet.
  - `modal-` : Les éléments visibles en pop-up avec un filtre devant le layout à l'action sur un bouton.
  - `header-`: Présentent les principaux designs des titres de pages.
  - `lay-`   : Le reste des modules, diverses et globaux. (Ex: header et footer).


### 2- Fichiers CSS
`src/stylesheets` 
- `app.scss` : Regroupe les appels à tous les modules CSS nécessaires.
- `*_module*.scss` : L'ensemble des modules CSS utilisés découpés en plusieurs fichiers (ex: _nav.scss).
- `_variables.scss` : Dans le dossier de Bootstrap, contient les variables utilisé pour tweeker le framework.


### 3- Fichiers JS
`src/javascripts` 
- `app.scss` : Custom jQuery de départ by indexel. A modifié selon les besoins de développement en back-end.
- `modules-backup` : Les différents frameworks & plugins utilisés. Les fichiers ne sont pas appelé mais sont présent. _(l'utilisation de ces élément se fait au travers d'appel ```<script>``` via leurs CDN respectifs)_.
.&nbsp;
.&nbsp;
##  :page_facing_up: TEMPLATING GENERAL

:warning: **Les `{{ .. }}` sont des variables nunjucks allant chercher des informations dans la bdd ou dans la page directement. Si l'application est développé en php, une suggestion est de remplacer ces accolades par  `<?= .. ?>`. Vous aurez ainsi bon nombre de variables déjà prête à l'emploi.**

### 1- lay-squelette.html

```nunjucks
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ headTitle }}</title>
    <link rel="stylesheet" href="stylesheets/app.css">
    <meta name="viewport" content="width=device-width, initial-scale=1">
  </head>
  <body id="{{ bodyID }}">
  {% include "layouts/commons/lay-header.html" %}
```
```nunjucks
    <main>
      {% block main %}{% endblock %}
    </main>
```
``` nunjucks
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bootstrap-slider/10.6.2/bootstrap-slider.min.js" integrity="sha256-oj52qvIP5c7N6lZZoh9z3OYacAIOjsROAcZBHUaJMyw=" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
    <script src="javascripts/app.js"></script>
  </body>
</html>
```
Fichier de base présent dans `src/layouts/commons` et comprend le doctype, les balises body head meta etc. chaque page fait appel à ce fichier. Il se décompose en trois sections:
  - Le début du template, avec le docype et un premier include de module, le header général.
  - Le corps du template, qui est repris sur le principe des @extend de nunjucks dans les pages.
  - La fin du template, avec entre autres l'appel aux scripts utilisés. Tous les scripts sont appelé non pas en dur dans le dossier, mais via des cdn. Libre choix de rajouter des callbacks au besoin. Fichier à laisser tel quel si le principe des extends est repris pour construire l'application, ou à découper en trois parties pour faire des includes plus basisques dans les pages. _A noter que des includes de modules "modals" ont été ajouté dans la dernière partie pour les besoins de la livraison. Libre choix de les déplacer si nécessaire._

### 2- lay-header.html, lay-footer.html

Présent dans `src/layouts/commons`. Header et footer généraux de l'application. le header comprend le menu de navigation principal et sa construction est basé sur la doc bootstrap. Les deux modules sont appelés dans chaque page.
&nbsp;
&nbsp;
##  :page_facing_up: TEMPLATING DES PAGES

### 1- example.html

```nunjucks
{% extends 'layouts/commons/lay-squelette.html' %}

{% set headTitle = "Team" %}
{% set bodyID = "layout-Team" %}


{% block main %}
<div class="container">
    <h1>Mes interlocuteurs</h1>
    <section class="row">
        {% for profil in team %}
        <div class="col-md-12 col-lg-6 col-sm">
            {% include "layouts/card-team.html" %}
        </div>
        {% endfor %}
    </section>
</div>
{% endblock %}

```
_Exemple de structure d'une page présent dans `/src`. Elle se décompose en deux principales sections:_
  - Les variables propres à la page (`headTitle`, `bodyID`). Libre d'en ajouter avec pour référence `lay-squelette.html`
  - Le corps du template `<main>`, qui est repris sur le principe des @extend de nunjucks dans les pages.
  - _D'autres pages sont un peu plus complexe et possède leurs propres sections à découvrir._
  - _A nouveau, ce sont des boucles et variables sur la syntaxe nunjucks, mais il est possible de les transformer en une syntaxe de boucle/variable php (si c'est la techno back-end retenue)._
  - _Les templates de pages respectent au mieux le principe suivant: La grille Bootstrap pour construire les pages uniquement dans les templates de pages. Pas de grille dans les modules (sauf pour structurer un module mais de l'intérieur)_.
..&nbsp;
..&nbsp;
##  :page_facing_up: TEMPLATING DES MODULES

:warning: **Les templates de modules ne possède pas de dimensions et sont par défaut en 100%. Les dimensions sont à définir via leurs emplacements dans les pages, au travers de la grille bootstrap _(row, col, container etc)_.

### 1-card-_[module]_.html

``` nunjucks
<div class="card-team-member bg-light-grey" data-team-member-id="{{ profil.id }}">
    <h2>{{ profil.role }}</h2>
    <div class="d-flex">
        <img class="card-team-member-img" src="{{ profil.imgProfil }}" />
        <div class="right-content">
            <h3 class="team-member-name like-h4">{{ profil.prenom }} {{ profil.nom }}</h3>
            <p class="team-member-adress">{{ profil.adresse }}<br/>{{ profil.codePostal }} {{ profil.ville }}<p>
            <p class="team-member-phone">{{ profil.telMob }}<p>
            <p class="team-member-mail">{{ profil.mail }}<p>
        </div>
    </div>
</div>
```
Reprend les éléments de type cartes/tuiles.

### 2-header-_[module]_.html

``` nunjucks
<section class="container-fluid no-pad bg-head">
    <img src="{{ programmes[0].imgLink }}" class="bg-head-img" alt="{{ programmes[0].imgAlt }}"/>
    <div class="container">
        <h1 class="bg-head-title">{{ programmes[0].name }}</h1>
        <p class="bg-head-chapo">{{ programmes[0].ville }} | {{ programmes[0].departement }}</p>
        <button class="btn icon favori"></button>
    </div>
</section>
```
Reprend les éléments de type sous-header/titre de page. **:warning: Quand un titre de page ne se construit qu'en ajoutant une unique balise h1, il n'a pas été juger nécessaire d'en faire un module.**

### 3- modal-_[module]_.html

``` nunjucks
<section class="container-fluid no-pad bg-head">
    <img src="{{ programmes[0].imgLink }}" class="bg-head-img" alt="{{ programmes[0].imgAlt }}"/>
    <div class="container">
        <h1 class="bg-head-title">{{ programmes[0].name }}</h1>
        <p class="bg-head-chapo">{{ programmes[0].ville }} | {{ programmes[0].departement }}</p>
        <button class="btn icon favori"></button>
    </div>
</section>
```
Reprend les éléments de type pop-up/modal sur le principe de bootstrap. Pour rappel avec Bootstrap les modals sont liés à un élément html (bouton ou lien généralement) au travers d'attribut data-_[example]_. Les détails sur https://getbootstrap.com/docs/4.0/components/modal. Exemple de lien entre un bouton et une modale avec l'attribut id:
``` nunjucks
<button type="button" data-toggle="modal" data-target="#modal-sell">Vendre</button>
```
``` nunjucks
<div class="modal" id="modal-sell" tabindex="-1">Contenu de la modale</div>
```

### 4- table-_[module]_.html

``` nunjucks
<div class="table-item details col-md-12" id="table-item">
    <div class="row header">
      <div class="item field icon right-icon arrow font-weight-bold" data-field="lot">Lot</div>
      <div class="item field icon right-icon arrow" data-field="typologie">Typologie</div>
      <div class="item field icon right-icon arrow" data-field="surface">Surface</div>
      <div class="item field icon right-icon arrow" data-field="prix">Prix</div>
      <div class="item field icon right-icon arrow" data-field="etat">Etat</div>
      <div class="item field icon right-icon arrow" data-field="orientation">Orientation</div>
      <div class="item field" data-field="icons"></div>
    </div>

    {% for lot in programmes[1].programmeLots %}
        <div class="row post" data-lot-id="{{ lot.id }}">
          <div class="item" data-lot>{{ lot.id }}</div>
          <div class="item" data-typologie>{{ lot.typologie }}</div>
          <div class="item" data-surface>{{ lot.surface }}</div>
          <div class="item" data-prix>{{ lot.prix }}</div>
          <div class="item {{ lot.etatColor }}" data-etat>{{ lot.etat }}</div>
          <div class="item" data-orientation>{{ lot.orientation }}</div>
          <div class="item" data-icons>
              <div class="table-icon">
                  <a class="icon medium-icon map" href="#"></a>
                  <a class="icon medium-icon denounce" data-toggle="modal" data-target="#modal-denounce" href="#"></a>
                  <a class="icon medium-icon option" data-toggle="modal" data-target="#modal-option" href="#"></a>
                  <a class="icon medium-icon simul" data-toggle="modal" data-target="#modal-simul" href="#"></a>
                  <a class="icon medium-icon sell" data-toggle="modal" data-target="#modal-sell" href="#"></a>
                  <a class="icon medium-icon arrow" data-toggle="modal" data-target="#modal-lot-details" href="#"></a>                     
              </div>
          </div>
        </div>
    {% endfor %}
  </div>
```
Reprend les éléments de type tableau.
 - 1- Remarquer que pour l'exemple ci-dessus, des liens possède `data-toggle="modal" data-target="#modal-denounce"`, donc des liens vers des modals.
 - 2- Le choix de construction des tableaux s'est porté sur une structure en `<div>` et non en `<table>` pour des raisons de simplification du changement de vue tableau-carte.
 - 3- La partie entre  `{% for lot in programmes[1].programmeLots %}` et `{% endfor %}` représente une ligne de tableau. Puis une boucle nunjucks va créer autant de ligne que de data existe sur le même principe que les boucles PHP.

:warning: **Du fait que les colonnages sont fait manuellement en `<div>`, cela  signifie que pour qu'un filtre et ses cellules possèdent la même largeur, le CSS fait appel à eux et les stylise en fonction. Example:**

``` css
.table-item.view1 [data-field="lots"], .table-item.view1 [data-lots] { width: 15%; }

```
**Le code CSS faire référence à un fltre spécifique du tableau, puis aux cellules portant le même attribut pour styliser leurs largeurs**.




