---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Vue d’ensemble du routage ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes du navigateur pour les actions de contrôleur.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ed043d76b89ce31945cf3423b0c5afca9383cc21
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65123649"
---
# <a name="aspnet-mvc-routing-overview-vb"></a>Vue d’ensemble du routage ASP.NET MVC (VB)

par [Stephen Walther](https://github.com/StephenWalther)

> Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes du navigateur pour les actions de contrôleur.

Dans ce didacticiel, vous découvrirez une fonctionnalité importante de toutes les applications ASP.NET MVC appelée *routage ASP.NET*. Le module de routage ASP.NET est chargé du mappage des demandes du navigateur entrantes à des actions de contrôleur MVC particuliers. À la fin de ce didacticiel, vous comprendrez comment la table de routage standard mappe les requêtes à actions du contrôleur.

## <a name="using-the-default-route-table"></a>À l’aide de la Table d’itinéraire par défaut

Lorsque vous créez une application ASP.NET MVC, l’application est déjà configurée pour utiliser le routage ASP.NET. ASP.NET Routing est configuré dans deux emplacements.

Tout d’abord, le routage ASP.NET est activé dans le fichier de configuration de votre application Web (fichier Web.config). Il existe quatre sections dans le fichier de configuration qui sont pertinentes pour le routage : la section system.web.httpModules, la section system.web.httpHandlers, la section system.webserver.modules et la section system.webserver.handlers. Veillez à ne pas supprimer ces sections, car sans ces sections routage ne fonctionnera plus.

Deuxièmement et plus important encore, une table de routage est créée dans le fichier Global.asax de l’application. Le fichier Global.asax est un fichier spécial qui contient les gestionnaires d’événements pour les événements de cycle de vie des applications ASP.NET. La table de routage est créée lors de l’événement de démarrer l’Application.

Le fichier dans le Listing 1 contient le fichier Global.asax de valeur par défaut pour une application ASP.NET MVC.

**Liste 1 - Global.asax.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Quand une application MVC commence, l’Application\_méthode Start() est appelée. Cette méthode, appelle à son tour, la méthode RegisterRoutes(). La méthode RegisterRoutes() crée la table de routage.

La table d’itinéraires par défaut contient un itinéraire unique (nommé par défaut). L’itinéraire par défaut mappe le premier segment d’URL à un nom de contrôleur, le deuxième segment d’URL à une action de contrôleur et le troisième segment à un paramètre nommé **id**.

Imaginez que vous entrez l’URL suivante dans la barre d’adresses de votre navigateur web :

/ Home/Index/3

L’itinéraire par défaut est mappé à cette URL pour les paramètres suivants :

- contrôleur = Home

- action = Index

- id = 3

Lorsque vous demandez l’URL de base/Index/3, le code suivant est exécuté :

HomeController.Index(3)

L’itinéraire par défaut inclut les valeurs par défaut pour tous les trois paramètres. Si vous ne fournissez pas un contrôleur, puis le paramètre de contrôleur par défaut la valeur **accueil**. Si vous ne fournissez pas une action, le paramètre d’action par défaut la valeur **Index**. Enfin, si vous ne fournissez pas un id, le paramètre id par défaut est une chaîne vide.

Examinons quelques exemples montrant comment l’itinéraire par défaut mappe des URL à actions du contrôleur. Imaginez que vous entrez l’URL suivante dans votre barre d’adresses du navigateur :

/ Édition familiale

En raison de l’itinéraire par défaut, paramètres par défaut entrer cette URL entraîne la méthode Index() de la classe HomeController dans la liste de 2 à appeler.

**Listing 2 - HomeController.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

Dans la liste 2, la classe HomeController inclut une méthode nommée Index() qui accepte un paramètre unique nommé ID. L’URL de base indique à la méthode Index() soit appelée avec la valeur Nothing comme valeur du paramètre Id.

En raison de la façon que l’infrastructure MVC appelle les actions de contrôleur, l’URL de base correspond également à la méthode Index() de la classe HomeController dans le Listing 3.

**Liste 3 - HomeController.vb (action Index sans paramètre)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

La méthode Index() dans le Listing 3 n’accepte pas de tous les paramètres. L’URL de base entraîne cette méthode Index() à appeler. L’URL de base/Index/3 appelle également cette méthode (l’Id est ignoré).

La base de l’URL correspond également à la méthode Index() de la classe HomeController sur la liste 4.

**Liste 4 - HomeController.vb (action Index avec le paramètre nullable)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

Dans la liste 4, la méthode Index() a un paramètre de type entier. Étant donné que le paramètre est un paramètre nullable (peut avoir la valeur Nothing), le Index() peut être appelée sans provoquer d’erreur.

Enfin, l’appel à la méthode Index() dans la liste des 5 avec la base de l’URL entraîne une exception depuis le paramètre Id *n’est pas* un paramètre nullable. Si vous tentez d’appeler la méthode Index() puis vous obtenez l’erreur affichée dans la Figure 1.

**Liste 5 - HomeController.vb (action Index avec le paramètre Id)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]

[![Appel d’une action de contrôleur qui attend une valeur de paramètre](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Figure 01**: Appel d’une action de contrôleur qui attend une valeur de paramètre ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-routing-overview-vb/_static/image2.png))

L’URL de base/Index/3, quant à eux, fonctionne parfaitement avec l’action de contrôleur d’Index dans la liste 5. La demande /Home/Index/3 indique à la méthode Index() à être appelée avec un paramètre d’Id qui a la valeur 3.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel est de vous fournir une brève introduction au routage ASP.NET. Nous avons examiné la table d’itinéraires par défaut que vous obtenez avec une application ASP.NET MVC. Vous avez appris comment l’itinéraire par défaut mappe des URL à actions du contrôleur.

> [!div class="step-by-step"]
> [Précédent](creating-an-action-cs.md)
> [Suivant](understanding-action-filters-vb.md)
