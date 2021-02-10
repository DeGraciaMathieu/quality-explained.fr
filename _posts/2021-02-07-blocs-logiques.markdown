---
layout: post
title:  "Les blocs logiques"
description:  "Un bloc logique est un ensemble de lignes partageant un sens commun."
date:   2021-02-07 22:59:04 +0100
categories: jekyll update
---

Un bloc logique est un ensemble de lignes partageant un sens commun.

Considérons le code suivant :

```php
$fooService = new FooService();
$fooService->setThing();
$barService = new BarService();
Event::fire(new FiringManyThings());
$fooService->setOtherThing();
return View::make('index', compact($barService));
```

Ce code fonctionne mais ces quelques lignes forment un ensemble compact dont <b>les intentions sont laborieuses à identifier</b>.

Il est pourtant facile de le clarifier en découpant ces lignes en plusieurs sous-ensembles partageant un sens commun :

- La création de class
- L’appel des méthodes
- L’assignation des variables...

En ajoutant une simple distinction visuelle, tel qu’un saut de ligne, nous pouvons <b>cloisoner ce code en plusieurs paragraphes</b>.

```php
$fooService = new FooService();
$barService = new BarService();

$fooService->setThing();
$fooService->setOtherThing();

Event::fire(new FiringManyThings());

return View::make('index', compact($barService));
```

Notre code gagne ainsi en lisibilité, la lecture en diagonale est en facilité et on appréhende désormais rapidement les différentes intentions.