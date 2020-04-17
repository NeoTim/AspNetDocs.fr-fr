---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Comprendre les filtres d’action (VB) Microsoft Docs
author: rick-anderson
description: Le but de ce tutoriel est d’expliquer les filtres d’action. Un filtre d’action est un attribut que vous pouvez appliquer à une action de contrôleur - ou un contrôleur entier ...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 70ab564bc1217f67874090d2ae6899d9bcd743a1
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542103"
---
# <a name="understanding-action-filters-vb"></a>Présentation des filtres d’actions (VB)

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> Le but de ce tutoriel est d’expliquer les filtres d’action. Un filtre d’action est un attribut que vous pouvez appliquer à une action de contrôleur -- ou à un contrôleur entier -- qui modifie la façon dont l’action est exécutée.

## <a name="understanding-action-filters"></a>Comprendre les filtres d’action

Le but de ce tutoriel est d’expliquer les filtres d’action. Un filtre d’action est un attribut que vous pouvez appliquer à une action de contrôleur -- ou à un contrôleur entier -- qui modifie la façon dont l’action est exécutée. Le cadre MVC ASP.NET comprend plusieurs filtres d’action :

- OutputCache - Ce filtre d’action cache la sortie d’une action de contrôleur pendant un certain temps.
- PoignéeError - Ce filtre d’action gère les erreurs soulevées lorsqu’une action du contrôleur s’exécute.
- Autoriser ce filtre d’action vous permet de restreindre l’accès à un utilisateur ou à un rôle particulier.

Vous pouvez également créer vos propres filtres d’action personnalisés. Par exemple, vous pouvez créer un filtre d’action personnalisé afin d’implémenter un système d’authentification personnalisé. Ou, vous pouvez créer un filtre d’action qui modifie les données de vue retournées par une action de contrôleur.

Dans ce tutoriel, vous apprenez à construire un filtre d’action à partir de zéro. Nous créons un filtre d’action Log qui enregistre différentes étapes du traitement d’une action à la fenêtre Visual Studio Output.

### <a name="using-an-action-filter"></a>Utilisation d’un filtre d’action

Un filtre d’action est un attribut. Vous pouvez appliquer la plupart des filtres d’action à une action individuelle ou à un contrôleur entier.

Par exemple, le contrôleur de données dans `Index()` la liste 1 expose une action nommée qui renvoie l’heure actuelle. Cette action est décorée `OutputCache` avec le filtre d’action. Ce filtre fait que la valeur retournée par l’action est mise en cache pendant 10 secondes.

**Liste 1`Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Si vous invoquez à plusieurs reprises l’action `Index()` en entrant l’URL /Data/Index dans la barre d’adresse de votre navigateur et en appuyant sur le bouton Rafraîchir plusieurs fois, alors vous verrez le même temps pendant 10 secondes. La sortie `Index()` de l’action est mise en cache pendant 10 secondes (voir la figure 1).

[![Temps caché](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Figure 01**: Temps de cache[(Cliquez pour voir l’image grandeur nature](understanding-action-filters-vb/_static/image3.png))

Dans la liste 1, un `OutputCache` filtre d’action `Index()` unique , le filtre d’action, est appliqué à la méthode. Si vous en avez besoin, vous pouvez appliquer plusieurs filtres d’action à la même action. Par exemple, vous pouvez appliquer `OutputCache` `HandleError` les filtres à la fois et d’action à la même action.

Dans la liste `OutputCache` 1, le `Index()` filtre d’action est appliqué à l’action. Vous pouvez également appliquer `DataController` cet attribut à la classe elle-même. Dans ce cas, le résultat retourné par toute action exposée par le contrôleur serait mis en cache pendant 10 secondes.

### <a name="the-different-types-of-filters"></a>Les différents types de filtres

Le cadre MVC ASP.NET prend en charge quatre types différents de filtres :

1. Filtres d’autorisation `IAuthorizationFilter` - Implémente l’attribut.
2. Filtres d’action `IActionFilter` - Implémente l’attribut.
3. Filtres de résultat `IResultFilter` - Implémente l’attribut.
4. Filtres d’exception `IExceptionFilter` - Implémente l’attribut.

Les filtres sont exécutés dans l’ordre indiqué ci-dessus. Par exemple, les filtres d’autorisation sont toujours exécutés avant que les filtres d’action et les filtres d’exception soient toujours exécutés après tous les autres types de filtre.

Les filtres d’autorisation sont utilisés pour implémenter l’authentification et l’autorisation des actions de contrôleur. Par exemple, le filtre Autoriser est un exemple de filtre d’autorisation.

Les filtres d’action contiennent une logique qui est exécutée avant et après l’exécution d’une action du contrôleur. Vous pouvez utiliser un filtre d’action, par exemple, pour modifier les données de vue qu’une action de contrôleur renvoie.

Les filtres de résultat contiennent une logique qui est exécutée avant et après l’exécution d’un résultat de vue. Par exemple, vous pouvez modifier un résultat de vue juste avant que la vue ne soit rendue au navigateur.

Les filtres d’exception sont le dernier type de filtre à exécuter. Vous pouvez utiliser un filtre d’exception pour gérer les erreurs soulevées par vos actions de contrôleur ou les résultats d’action du contrôleur. Vous pouvez également utiliser des filtres d’exception pour enregistrer des erreurs.

Chaque type de filtre est exécuté dans un ordre particulier. Si vous souhaitez contrôler l’ordre dans lequel les filtres du même type sont exécutés, vous pouvez définir la propriété d’ordre d’un filtre.

La classe de base pour `System.Web.Mvc.FilterAttribute` tous les filtres d’action est la classe. Si vous souhaitez implémenter un type particulier de filtre, vous devez créer une classe qui hérite de la classe de filtre de base et implémente une ou plusieurs des interfaces IAuthorizationFilter, IActionFilter, IResultFilter ou ExceptionFilter.

### <a name="the-base-actionfilterattribute-class"></a>La catégorie Base ActionFilterAttribute

Afin de vous faciliter la mise en œuvre d’un filtre d’action personnalisé, le cadre MVC ASP.NET comprend une classe de base. `ActionFilterAttribute` Cette classe implémente à la fois les `IActionFilter` interfaces et `IResultFilter` hérite de la `Filter` classe.

La terminologie ici n’est pas tout à fait cohérente. Techniquement, une classe qui hérite de la classe ActionFilterAttribute est à la fois un filtre d’action et un filtre de résultat. Cependant, dans le sens lâche, le filtre d’action mot est utilisé pour se référer à n’importe quel type de filtre dans le cadre ASP.NET MVC.

La classe ActionFilterAttribute de base a les méthodes suivantes que vous pouvez remplacer :

- OnActionExecuting - Cette méthode est appelée avant qu’une action de contrôleur ne soit exécutée.
- OnActionExecuted - Cette méthode est appelée après l’exécution d’une action de contrôleur.
- OnResultExecuting - Cette méthode est appelée avant qu’un résultat d’action de contrôleur soit exécuté.
- OnResultExecuted - Cette méthode est appelée après un résultat d’action de contrôleur est exécuté.

Dans la section suivante, nous verrons comment vous pouvez mettre en œuvre chacune de ces différentes méthodes.

### <a name="creating-a-log-action-filter"></a>Création d’un filtre d’action en rondins

Afin d’illustrer comment vous pouvez construire un filtre d’action personnalisé, nous allons créer un filtre d’action personnalisé qui enregistre les étapes de traitement d’une action contrôleur à la fenêtre Visual Studio Output. Notre `LogActionFilter` est contenu dans la liste 2.

**Liste 2`ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

Dans la liste `OnActionExecuting()`2, le `OnActionExecuted()`, , `OnResultExecuting()`et `OnResultExecuted()` les méthodes appellent tous la `Log()` méthode. Le nom de la méthode et les `Log()` données d’itinéraire actuelles sont transmis à la méthode. La `Log()` méthode écrit un message à la fenêtre Visual Studio Output (voir la figure 2).

[![Écrire à la fenêtre Visual Studio Output](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Figure 02**: Écrire à la fenêtre Visual Studio Output ([Cliquez pour voir l’image grandeur nature](understanding-action-filters-vb/_static/image6.png))

Le contrôleur à domicile dans la liste 3 illustre comment vous pouvez appliquer le filtre d’action Log à toute une classe de contrôleur. Chaque fois que l’une des actions exposées `Index()` par le `About()` contrôleur à domicile sont invoquées - soit la méthode ou la méthode - les étapes de traitement de l’action sont enregistrées à la fenêtre Visual Studio Output.

**Liste 3`Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Récapitulatif

Dans ce tutoriel, vous avez été présenté à ASP.NET filtres d’action MVC. Vous avez pris connaissance des quatre types de filtres : filtres d’autorisation, filtres d’action, filtres de résultat et filtres d’exception. Vous avez aussi `ActionFilterAttribute` appris la classe de base.

Enfin, vous avez appris à implémenter un filtre d’action simple. Nous avons créé un filtre d’action Log qui enregistre les étapes du traitement d’une action de contrôleur à la fenêtre Visual Studio Output.

> [!div class="step-by-step"]
> [Suivant précédent](asp-net-mvc-routing-overview-vb.md)
> [Next](improving-performance-with-output-caching-vb.md)
