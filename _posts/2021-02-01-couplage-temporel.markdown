---
layout: post
title:  "Couplage temporel"
description:  "le couplage temporel est un comportement contextuel d’une méthode qui n’est pas pertinent dans tous les cas d’utilisation."
date:   2021-02-05 12:00:04 +0100
categories: jekyll update
---

le couplage temporel est un comportement contextuel d’une méthode qui n’est pas pertinent dans tous les cas d’utilisation.

{% highlight php %}
class Authenticate {

    public function login(integer $id): User
    {
        $user = Auth::login($id);

        $this->refreshRigths($user);

        return $user;
    }
}
{% endhighlight %}

L’appel de cette méthode `refreshRigths` <b>n’est pas problématique en soit</b>, il faut cependant déterminer si sa présence au sein de notre méthode `authenticate` n’est pas contraignante.

<b>Voulons nous vraiment associer une authentification à un rafraichissement des droits</b>, ce comportement doit il être tacite ?

Si ce n’est pas le cas alors cet appel de `refreshRigths` peut s’averer problématique et restreindre notre utilisation de la méthode `authenticate`.

> Et si je veux uniquement effectuer un `Auth::login` sans refresh les droits ? 

<b>Un couplage temporel dégrade la testabilité et la souplesse d’une méthode</b> la rendant étroitement liée à un contexte d’utilisation.

Par ailleurs, si ce comportement est également innatendu, nous sommes alors confronté à une problématique de [distance sémantique](https://quality-explained.fr/distance-semantique/).