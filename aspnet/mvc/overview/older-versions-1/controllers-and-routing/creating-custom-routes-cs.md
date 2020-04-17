---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Création d’itinéraires personnalisés (C) Microsoft Docs
author: rick-anderson
description: Découvrez comment ajouter des itinéraires personnalisés à une application MVC ASP.NET. Dans ce tutoriel, vous apprenez à modifier le tableau d’itinéraire par défaut dans le fichier Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: b66ccc5e0cd4f6d7e5884394c2b7555b76382d3d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542753"
---
# <a name="creating-custom-routes-c"></a>Création de routes personnalisées (C#)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment ajouter des itinéraires personnalisés à une application MVC ASP.NET. Dans ce tutoriel, vous apprenez à modifier le tableau d’itinéraire par défaut dans le fichier Global.asax.

Dans ce tutoriel, vous apprenez à ajouter un itinéraire personnalisé à une application ASP.NET MVC. Vous apprenez à modifier le tableau d’itinéraire par défaut dans le fichier Global.asax avec un itinéraire personnalisé.

Pour de nombreuses applications MVC simples ASP.NET, le tableau d’itinéraire par défaut fonctionnera très bien. Cependant, vous pourriez découvrir que vous avez des besoins spécialisés de routage. Dans ce cas, vous pouvez créer un itinéraire personnalisé.

Imaginez, par exemple, que vous construisez une application de blog. Vous voudrez peut-être traiter les demandes entrantes qui ressemblent à ceci:

/Archives/12-25-2009

Lorsqu’un utilisateur saisît cette demande, vous souhaitez retourner l’entrée du blog qui correspond à la date 25/12/2009. Afin de gérer ce type de demande, vous devez créer un itinéraire personnalisé.

Le fichier Global.asax dans La liste 1 contient un nouvel itinéraire personnalisé, nommé Blog, qui gère les demandes qui ressemblent à /Archive /*date d’entrée*.

**Liste 1 - Global.asax (avec itinéraire personnalisé)**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

L’ordre des itinéraires que vous ajoutez à la table d’itinéraire est important. Notre nouvel itinéraire Blog personnalisé est ajouté avant l’itinéraire par défaut existant. Si vous avez inversé l’ordre, alors l’itinéraire par défaut sera toujours appelé au lieu de l’itinéraire personnalisé.

L’itinéraire Blog personnalisé correspond à toute demande qui commence par /Archive/. Ainsi, il correspond à toutes les URL suivantes:

- /Archives/12-25-2009

- /Archives/10-6-2004

- /Archives/pomme

L’itinéraire personnalisé cartographie la demande entrante à un contrôleur nommé Archive et invoque l’action d’entrée.). Lorsque la méthode d’entrée() est appelée, la date d’entrée est passée comme paramètre nommé entryDate.

Vous pouvez utiliser l’itinéraire personnalisé Blog avec le contrôleur dans la liste 2.

**Liste 2 - ArchiveController.cs**

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

Notez que la méthode d’entrée () dans la liste 2 accepte un paramètre de type DateTime. Le cadre MVC est assez intelligent pour convertir automatiquement la date d’entrée de l’URL en une valeur DateTime. Si le paramètre de date d’entrée de l’URL ne peut pas être converti en date d’heure, une erreur est soulevée (voir la figure 1).

**Figure 1 - Erreur de conversion du paramètre**

[![Boîte de dialogue New Project](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)

**Figure 01**: Erreur de conversion du paramètre ([Cliquez pour voir l’image grandeur nature](creating-custom-routes-cs/_static/image2.png))

## <a name="summary"></a>Récapitulatif

Le but de ce tutoriel était de démontrer comment vous pouvez créer un itinéraire personnalisé. Vous avez appris à ajouter un itinéraire personnalisé à la table d’itinéraire dans le fichier Global.asax qui représente les entrées de blog. Nous avons discuté de la façon de cartographier les demandes d’entrées de blog à un contrôleur nommé ArchiveController et une action de contrôleur nommé Entrée().

> [!div class="step-by-step"]
> [Suivant précédent](aspnet-mvc-controllers-overview-cs.md)
> [Next](creating-a-route-constraint-cs.md)
