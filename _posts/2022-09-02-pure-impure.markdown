---
layout: post
title:  "pure / impure"
description:  "Une méthode impure possède des effets de bord, toutes ses dépendances ne sont pas déterministes mais caractérisées par l’état de l’application."
date:   2022-09-02 15:59:04 +0100
categories: jekyll update
---

On considère impure une méthode qui possède des effets de bord, dont toutes les dépendances ne sont pas déterministes mais caractérisées par l’état de l’application.

La méthode `getRefreshedRights` suivante est impure car elle manipule une instance de `User` récupérée depuis le singleton `auth()`.

{% highlight php %}
class UserService {

    public function getRefreshedRights(): Collection
    {
        $user = auth()->user();

        $rights = $user->rights;

        $rights->refresh();

        return $rights;
    }
}
{% endhighlight %}

Il est nécessaire qu’un utilisateur soit connecté pour que cette méthode fonctionne, elle est donc **dépendante d’un état de l’application qui n’est pas explicite** : avoir un utilisateur authentifié.

**L’utilisation d’une méthode impure est toujours contraignant**, ses effets de bord la rendant faillible et étroitement liée à un contexte d’utilisation.

Tester une méthode impure sera également laborieux car vous devez vous assurer que tous les effets de bord seront satisfaits en amont, principalement à l’aide de mock.

<hr>

## Une méthode pure

Une méthode pure n’est pas sujet aux effets de bord, toutes ses dépendances sont déterministes.

Dans notre exemple, remplacer le singleton `auth` par un argument `$user` rendra notre méthode pure.

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

Ainsi, notre méthode `getRefreshedRights` ne sera plus dépendante d'un état spécifique de l’application, plus facile à tester et gagnera en souplesse d'utilisation en repondant à de nouveaux usecases.