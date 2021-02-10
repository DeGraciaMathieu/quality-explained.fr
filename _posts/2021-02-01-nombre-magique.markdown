---
layout: post
title:  "Nombre magique"
description:  "Un nombre magique est une valeur possedant un sens métier et présent en dur dans votre code."
date:   2021-02-04 22:59:04 +0100
categories: jekyll update
---

Un nombre magique est une valeur, le plus souvent un integer, possedant un sens métier et présent en dur dans votre code.

{% highlight php %}
public function setOrder(Order $order)
{
    if ($order->state === 2) {
        throw new Exception();
    }

    $this->order = $order;
}
{% endhighlight %}

À quoi correspond la valeur `2` ? Pourquoi cette valeur doit lever une exception ? 

<b>La comprehension d’un nombre magique n’est pas instinctive</b>, elle nécessite des connaissances particulières ou un effort supplémentaire pour en saisir le sens.

Cette <b>incertitude est délétère</b> pour la lisibilité et la simplicité de votre code.

La démultiplication d’un même nombre magique au sein d’une application est également <b>problématique pour sa maintenabilité</b>.

Le moindre changement de valeur se devra d’être répercuté sur l’intégralité de l’application, c'est un travail qui peut s'averer fastidieux et source d'erreurs.

Un nombre magique aura donc tendance à <b>rendre votre code plus cryptique et moins maintenable</b> sur le long terme ou pour un nouveau développeur intégrant le projet.

<hr>

<h1>How to fix</h1>

Pour mitiger cette problématique nous allons chercher à <b>encapsuler et expliciter</b> ce nombre magique.

Si ce nombre est statique qu'importe le contexte d'utilisation alors l’écriture d'une [class de constantes](https://www.php.net/manual/fr/language.oop5.constants.php) peut s’avérer judicieux.

Par la même occasion <b>ce nombre est désormais nommé</b>, rendant notre situation bien plus explicite.

{% highlight php %}
public function setOrder(Order $order)
{
    if ($order->state === OrderStateConstants::BLOCKED) {
        throw new UnexpectedStateException();
    }

    $this->order = $order;
}
{% endhighlight %}

Dans le cas contraire, si ce nombre est dynamique, il sera alors préferable d'utiliser un fichier de configurations.

{% highlight php %}
public function setOrder(Order $order)
{
    if ($order->state === Config::get('orders.states.blocked')) {
        throw new UnexpectedStateException();
    }

    $this->order = $order;
}
{% endhighlight %}
