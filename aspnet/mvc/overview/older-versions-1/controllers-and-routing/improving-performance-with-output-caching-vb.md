---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: Améliorer les performances avec l’en cachement de sortie (VB) Microsoft Docs
author: rick-anderson
description: Dans ce tutoriel, vous apprenez comment vous pouvez améliorer considérablement les performances de vos applications Web ASP.NET MVC en profitant de la mise en cache de sortie. Vous...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: e18d4c5132d4dccc97f1465e7885c9c47a0edab1
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542688"
---
# <a name="improving-performance-with-output-caching-vb"></a>Amélioration des performances avec la mise en cache de la sortie (VB)

par [Microsoft](https://github.com/microsoft)

> Dans ce tutoriel, vous apprenez comment vous pouvez améliorer considérablement les performances de vos applications Web ASP.NET MVC en profitant de la mise en cache de sortie. Vous apprenez à mettre en cache le résultat retourné d’une action de contrôleur afin que le même contenu n’ait pas besoin d’être créé chaque fois qu’un nouvel utilisateur invoque l’action.

Le but de ce tutoriel est d’expliquer comment vous pouvez améliorer considérablement les performances d’une application ASP.NET MVC en profitant du cache de sortie. Le cache de sortie vous permet de mettre en cache le contenu retourné par une action de contrôleur. De cette façon, le même contenu n’a pas besoin d’être généré chaque fois que la même action de contrôleur est invoquée.

Imaginez, par exemple, que votre application ASP.NET MVC affiche une liste d’enregistrements de bases de données dans une vue nommée Index. Normalement, chaque fois qu’un utilisateur invoque l’action du contrôleur qui renvoie la vue de l’index, l’ensemble des enregistrements de base de données doit être récupéré dans la base de données en exécutant une requête de base de données.

Si, d’autre part, vous profitez du cache de sortie, alors vous pouvez éviter d’exécuter une requête de base de données chaque fois que n’importe quel utilisateur invoque la même action de contrôleur. La vue peut être récupérée à partir du cache au lieu d’être régénérée de l’action du contrôleur. Le cachage vous permet d’éviter d’effectuer des travaux redondants sur le serveur.

#### <a name="enabling-output-caching"></a>Permettre l’indage des sorties

Vous activez la mise &lt;en&gt; cache de sortie en ajoutant un attribut OutputCache à une action individuelle de contrôleur ou à une classe entière de contrôleur. Par exemple, le contrôleur dans la liste 1 expose une action nommée Index(). La sortie de l’action Index() est mise en cache pendant 10 secondes.

**Liste 1 - Controllers-HomeController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]

Dans les versions bêta de ASP.NET MVC, la mise en [http://www.MySite.com/](http://www.mysite.com/)cache de sortie ne fonctionne pas pour une URL comme . Au lieu de cela, [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index)vous devez entrer une URL comme .

Dans la liste 1, la sortie de l’action Index() est mise en cache pendant 10 secondes. Si vous préférez, vous pouvez spécifier une durée de cache beaucoup plus longue. Par exemple, si vous souhaitez mettre en cache la sortie d’une action de contrôleur pendant une journée, \* vous pouvez \* spécifier une durée de cache de 86400 secondes (60 secondes 60 minutes 24 heures).

Il n’y a aucune garantie que le contenu sera mis en cache pour la durée de temps que vous spécifiez. Lorsque les ressources de mémoire deviennent faibles, le cache commence à expulser automatiquement le contenu.

Le contrôleur à domicile dans la liste 1 retourne la vue de l’indice dans la liste 2. Il n’y a rien de spécial dans ce point de vue. La vue de l’index affiche simplement l’heure actuelle (voir la figure 1).

**Liste 2 - Views-Home.Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

**Figure 1 - Vue de l’indice cache**

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

Si vous invoquez l’action Index () plusieurs fois en entrant l’URL /Home/Index dans la barre d’adresse de votre navigateur et en appuyant sur le bouton Rafraîchir/recharger votre navigateur à plusieurs reprises, le temps affiché par la vue d’index ne changera pas pendant 10 secondes. Le même temps est affiché parce que la vue est mise en cache.

Il est important de comprendre que la même vue est mise en cache pour tous ceux qui visitent votre demande. Toute personne qui invoque l’action Index() recevra la même version mise en cache de la vue Index. Cela signifie que la quantité de travail que le serveur Web doit effectuer pour servir la vue Index est considérablement réduite.

La vue dans la liste 2 se trouve à faire quelque chose de vraiment simple. La vue affiche juste l’heure actuelle. Cependant, vous pouvez tout aussi bien mettre en cache une vue qui affiche un ensemble d’enregistrements de base de données. Dans ce cas, l’ensemble des enregistrements de base de données n’aurait pas besoin d’être récupéré dans la base de données chaque fois que l’action du contrôleur qui renvoie la vue est invoquée. Le cachage peut réduire la quantité de travail que votre serveur Web et votre serveur de base de données doivent effectuer.

N’utilisez pas &lt;la page %&gt; - directive OutputCache % dans une vue MVC. Cette directive est saignante dans le monde des formulaires Web et ne doit pas être utilisée dans une application ASP.NET MVC. 

#### <a name="where-content-is-cached"></a>Où le contenu est mis en cache

Par défaut, lorsque &lt;vous utilisez&gt; l’attribut OutputCache, le contenu est mis en cache dans trois emplacements : le serveur Web, tous les serveurs proxy et le navigateur Web. Vous pouvez contrôler exactement l’endroit où le &lt;contenu est&gt; mis en cache en modifiant la propriété de localisation de l’attribut OutputCache.

Vous pouvez définir la propriété De localisation à l’une des valeurs suivantes :

> · Tout
> 
> · Client
> 
> · Aval
> 
> · Serveur
> 
> · Aucun
> 
> · ServerAndClient (en)

Par défaut, la propriété De localisation a la valeur Any. Cependant, il ya des situations dans lesquelles vous voudrez peut-être cache uniquement sur le navigateur ou uniquement sur le serveur. Par exemple, si vous cachez des informations personnalisées pour chaque utilisateur, vous ne devez pas mettre en cache les informations sur le serveur. Si vous affichez des informations différentes à différents utilisateurs, vous devez mettre en cache les informations uniquement sur le client.

Par exemple, le contrôleur dans La liste 3 expose une action nommée GetName() qui renvoie le nom d’utilisateur actuel. Si Jack se connecte au site Web et invoque l’action GetName(), l’action renvoie la chaîne "Salut Jack". Si, par la suite, Jill se connecte au site Web et invoque l’action GetName(), elle recevra également la chaîne "Salut Jack". La chaîne est mise en cache sur le serveur web pour tous les utilisateurs après Jack invoque initialement l’action du contrôleur.

**Liste 3 - Controllers-BadUserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

Très probablement, le contrôleur dans la liste 3 ne fonctionne pas comme vous le souhaitez. Vous ne voulez pas afficher le message "Salut Jack" à Jill.

Vous ne devez jamais mettre en cache du contenu personnalisé dans le cache serveur. Cependant, vous pouvez mettre en cache le contenu personnalisé dans le cache du navigateur pour améliorer les performances. Si vous cachez du contenu dans le navigateur, et qu’un utilisateur invoque la même action de contrôleur plusieurs fois, le contenu peut être récupéré à partir du cache du navigateur au lieu du serveur.

Le contrôleur modifié dans la liste 4 cache la sortie de l’action GetName(). Cependant, le contenu est mis en cache uniquement sur le navigateur et non sur le serveur. De cette façon, lorsque plusieurs utilisateurs invoquent la méthode GetName(), chaque personne obtient son propre nom d’utilisateur et non le nom d’utilisateur d’une autre personne.

**Liste 4 - Controllers-UserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

Notez &lt;que l’attribut OutputCache&gt; dans la liste 4 comprend une propriété de localisation définie à la valeur OutputCacheLocation.Client. L’attribut &lt;&gt; OutputCache comprend également une propriété NoStore. La propriété NoStore est utilisée pour informer les serveurs proxy et les navigateurs qu’ils ne devraient pas stocker une copie permanente du contenu mis en cache.

#### <a name="varying-the-output-cache"></a>Variation du cache de sortie

Dans certaines situations, vous voudrez peut-être différentes versions mises en cache du même contenu. Imaginez, par exemple, que vous créez une page master/détail. La page maîtresse affiche une liste de titres de films. Lorsque vous cliquez sur un titre, vous obtenez des détails pour le film sélectionné.

Si vous cachez la page de détails, les détails du même film seront affichés quel que soit le film que vous cliquez. Le premier film sélectionné par le premier utilisateur sera affiché à tous les futurs utilisateurs.

Vous pouvez résoudre ce problème en profitant de &lt;la propriété&gt; VaryByParam de l’attribut OutputCache. Cette propriété vous permet de créer différentes versions mises en cache du même contenu lorsqu’un paramètre de formulaire ou un paramètre de chaîne de requête varie.

Par exemple, le contrôleur dans la liste 5 expose deux actions nommées Master() et Détails(). L’action Master () renvoie une liste de titres de films et l’action Détails () renvoie les détails du film sélectionné.

**Liste 5 - Controllers-MoviesController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

L’action Master() comprend une propriété VaryByParam dont la valeur est « nulle ». Lorsque l’action Master() est invoquée, la même version mise en cache de la vue Master est retournée. Tous les paramètres de formulaire ou les paramètres de chaîne de requête sont ignorés (voir la figure 2).

**Figure 2 - Le /Films/Master view**

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

**Figure 3 - La vue /Films/Détails**

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

L’action Détails () comprend une propriété VaryByParam avec la valeur "Id". Lorsque différentes valeurs du paramètre Id sont transmises à l’action du contrôleur, différentes versions mises en cache de la vue Détails sont générées.

Il est important de comprendre que l’utilisation de la propriété VaryByParam se traduit par plus de mise en cache et non moins. Une version mise en cache différente de la vue Détails est créée pour chaque version différente du paramètre Id.

Vous pouvez définir la propriété VaryByParam selon les valeurs suivantes :

> \*Créer une version mise en cache différente chaque fois qu’un paramètre de forme ou de chaîne de requête varie.
> 
> aucun - Ne jamais créer de différentes versions mises en cache
> 
> Liste des paramètres de semi-clon - Créer différentes versions mises en cache chaque fois que l’un des paramètres de forme ou de chaîne de requête dans la liste varie

#### <a name="creating-a-cache-profile"></a>Création d’un profil Cache

Comme alternative à la configuration des propriétés de &lt;cache&gt; de sortie en modifiant les propriétés de l’attribut OutputCache, vous pouvez créer un profil de cache dans le fichier de configuration web (web.config). La création d’un profil de cache dans le fichier de configuration Web offre quelques avantages importants.

Tout d’abord, en configurant la mise en cache de sortie dans le fichier de configuration Web, vous pouvez contrôler la façon dont le contrôleur actionne le contenu de cache dans un emplacement central. Vous pouvez créer un profil de cache et appliquer le profil à plusieurs contrôleurs ou actions de contrôleur.

Deuxièmement, vous pouvez modifier le fichier de configuration Web sans recompiler votre application. Si vous avez besoin de désactiver la mise en cache d’une application qui a déjà été déployée en production, vous pouvez simplement modifier les profils de cache définis dans le fichier de configuration Web. Toute modification du fichier de configuration Web sera détectée automatiquement et appliquée.

Par exemple, &lt;la&gt; section de configuration Web de mise en cache dans La liste 6 définit un profil de cache nommé Cache1Hour. La &lt;section&gt; de mise &lt;en cache&gt; doit apparaître dans la section system.web d’un fichier de configuration Web.

**Liste 6 - Section De cachage pour web.config**

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

Le contrôleur dans la liste 7 illustre comment vous pouvez appliquer le &lt;profil&gt; Cache1Hour à une action de contrôleur avec l’attribut OutputCache.

**Liste 7 - Controllers-ProfileController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

Si vous invoquez l’action Index() exposée par le contrôleur dans la liste 7, alors la même heure sera retournée pendant 1 heure.

#### <a name="summary"></a>Récapitulatif

La mise en cache de sortie vous offre une méthode très facile pour améliorer considérablement les performances de vos applications ASP.NET MVC. Dans ce tutoriel, vous avez &lt;appris&gt; à utiliser l’attribut OutputCache pour mettre en cache la sortie des actions de contrôleur. Vous avez également appris à &lt;modifier&gt; les propriétés de l’attribut OutputCache telles que les propriétés Duration et VaryByParam pour modifier la façon dont le contenu est mis en cache. Enfin, vous avez appris à définir les profils de cache dans le fichier de configuration web.

> [!div class="step-by-step"]
> [Suivant précédent](understanding-action-filters-vb.md)
> [Next](adding-dynamic-content-to-a-cached-page-vb.md)
