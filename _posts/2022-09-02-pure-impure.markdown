---
layout: post
title:  "pures / impures"
description:  "Une fonction impure possède des effets de bord, toutes ses dépendances ne sont pas déterministes mais caractérisées par l’état de l’application."
date:   2022-09-02 15:59:04 +0100
categories: jekyll update
---

On considère impure une fonction qui possède des effets de bord, dont toutes les dépendances ne sont pas déterministes mais caractérisées par l’état de l’application.

La méthode `getRefreshedRights` suivante est impure car elle manipule une instance de `user` récupérée depuis le singleton `auth()`.

{% highlight php %}
class UserService {

    public function getRefreshedRights(): Collection
    {
        $rights = auth()->user()->rights;

        $rights->refresh();

        return $rights;
    }
}
{% endhighlight %}

Il est nécessaire qu’un utilisateur soit connecté pour que cette méthode fonctionne, elle est donc **dépendante d’un état de l’application qui n’est pas explicite** : avoir un utilisateur authentifié.

**L’utilisation d’une méthode impure est toujours contraignant**, ses effets de bord la rendant faillible et étroitement liée à un contexte d’utilisation.

Tester une méthode impure sera également laborieux car vous devez vous assurer que tous les effets de bord seront satisfaits en amont, principalement à l’aide de mocks.

<hr>

## Une méthode pure

Une méthode pure n’est pas sujet aux effets de bord, toutes ses dépendances sont déterministes.

Dans notre example, il suffira de passer une instance `user` en argument pour rendre notre méthode pure.

Ainsi, elle ne sera plus dépendante de l’état de l’application et plus facile à tester.

{% highlight php %}
class UserService {

    public function getRefreshedRights(User $user): Collection
    {
        $rights = $user->rights;

        $rights->refresh();

        return $rights;
    }
}
{% endhighlight %}

Cette méthode pure gagne également en souplesse d'utilisation en répondant à de nouveaux usecases.

En effet, il n'est plus nécessaire qu'un utilisateur soit connecté pour la manier, nous pouvons désormais l'utiliser sur n'importe quelle instance de `User` !
