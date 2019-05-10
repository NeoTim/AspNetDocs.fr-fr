---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Création de Routes personnalisées (c#) | Microsoft Docs
author: microsoft
description: Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à modifier la table d’itinéraires par défaut dans le fichier Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 58f72e390f0053d136ef00ddbda0b071ba225d98
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65123357"
---
# <a name="creating-custom-routes-c"></a>Création de routes personnalisées (C#)

by [Microsoft](https://github.com/microsoft)

> Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à modifier la table d’itinéraires par défaut dans le fichier Global.asax.

Dans ce didacticiel, vous allez apprendre à ajouter un itinéraire personnalisé à une application ASP.NET MVC. Vous allez apprendre à modifier la table d’itinéraires par défaut dans le fichier Global.asax avec un itinéraire personnalisé.

Pour de nombreuses applications ASP.NET MVC simples, la table d’itinéraires par défaut fonctionnera parfaitement. Toutefois, vous pouvez découvrir que vous avez routage des besoins spécifiques. Dans ce cas, vous pouvez créer un itinéraire personnalisé.

Par exemple, imaginez que vous générez une application de blog. Vous souhaiterez peut-être gérer les demandes entrantes qui ressemblent à ceci :

/ Archive/12-25-2009

Lorsqu’un utilisateur entre cette demande, vous souhaitez retourner l’entrée de blog qui correspond à la date 25/12/2009. Pour pouvoir traiter ce type de demande, vous devez créer un itinéraire personnalisé.

Le fichier Global.asax dans le Listing 1 contient un nouvel itinéraire personnalisé, nommé Blog, qui traite les requêtes qui ressemblent à /Archive/*date d’entrée*.

**Liste 1 - Global.asax (avec itinéraire personnalisé)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

L’ordre des itinéraires que vous ajoutez à la table de routage est importante. Notre nouvel itinéraire Blog personnalisé est ajouté avant l’itinéraire par défaut existant. Si vous inversons l’ordre, puis l’itinéraire par défaut sera toujours appelée au lieu de l’itinéraire personnalisé.

L’itinéraire de Blog personnalisée correspond à toute demande qui commence par/Archive /. Par conséquent, elle correspond à toutes les URL suivantes :

- / Archive/12-25-2009

- / Archive/10-6-2004

- / Archive/apple

L’itinéraire personnalisé mappe la requête entrante à un contrôleur nommé Archive et appelle l’action Entry(). Lorsque la méthode Entry() est appelée, la date d’entrée est transmise en tant que paramètre nommé entryDate.

Vous pouvez utiliser l’itinéraire personnalisé Blog avec le contrôleur dans le Listing 2.

**Listing 2 - ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Notez que la méthode Entry() dans le Listing 2 accepte un paramètre de type DateTime. L’infrastructure MVC est suffisamment intelligent pour convertir automatiquement la date d’entrée à partir de l’URL en une valeur DateTime. Si le paramètre de date d’entrée à partir de l’URL ne peut pas être converti en une valeur DateTime, une erreur est générée (voir Figure 1).

**Figure 1 : erreur de conversion de paramètre**

[![La boîte de dialogue Nouveau projet](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Figure 01**: Erreur de conversion de paramètre ([cliquez pour afficher l’image en taille réelle](creating-custom-routes-cs/_static/image2.png))

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel a été pour illustrer comment vous pouvez créer un itinéraire personnalisé. Vous avez appris comment ajouter un itinéraire personnalisé à la table de routage dans le fichier Global.asax qui représente les entrées de blog. Nous avons vu comment mapper les requêtes pour les entrées de blog à un contrôleur nommé ArchiveController et une action de contrôleur nommé Entry().

> [!div class="step-by-step"]
> [Précédent](aspnet-mvc-controllers-overview-cs.md)
> [Suivant](creating-a-route-constraint-cs.md)
