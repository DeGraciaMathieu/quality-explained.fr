---
layout: post
title:  "La distance sémantique"
description:  "La distance sémantique est définie par l’écart qui existe entre le nom d’une méthode et les actions qu'elle realise."
date:   2021-02-03 22:59:04 +0100
categories: jekyll update
---

La distance sémantique est définie par <b>l’écart qui existe entre le nom d’une méthode et les actions qu'elle realise</b>.

{% highlight php %}
function requestHttpTokenValidation(Validation $validation)
{
    $valideable = DnsTxtTokenValidation::create([
        'token' => $validation->token,
    ]);

    Mail::to($validation->user)->send(new ValidationEmail($valideable));

    return $valideable;
}
{% endhighlight %}

Rien n’indique ici que la création d’une validation implique d’envoyer un mail : <b>Ce comportement n’est pas explicite</b>.

La définition d’une méthode se doit d’être <b>claire sur ses intentions</b>, tout <b>comportement caché est une source d’incomprehensions</b> et de surprises que l’on doit éviter.

Une grande distance sémantique est souvent <b>symptomatique d’un problème de conception</b> et revelateur d'une `class` possédant trop de préocupations.

<hr>

<h1>How to fix</h1>

Dans le cas présent, il est important de se poser la question suivante :

> Cette préoccupation est-elle suffisament proche pour exister au sein de cette méthode ?

Si la réponse est oui, on peut alors <b>chercher à découpler</b> cette méthode en utilisant par exemple un `Event Dispatcher`.

{% highlight php %}
function requestHttpTokenValidation(Validation $validation)
{
    $valideable = DnsTxtTokenValidation::create([
        'token' => $validation->token,
    ]);

    Event::dispatch(new ValidationRequested($valideable));

    return $valideable;
}
{% endhighlight %}

Au contraire, si la réponse est non, nous sommes probablement face à un [couplage temporel](https://quality-explained.fr/couplage-temporel/).

Il est conseillé de refactoriser en profondeur cette méthode pour en <b>extraire les différentes préoccupations</b>.
