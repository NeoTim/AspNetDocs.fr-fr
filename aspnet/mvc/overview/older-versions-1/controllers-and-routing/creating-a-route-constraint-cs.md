---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Création d’une contrainte d’itinéraire (c#) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont le navigateur demande itinéraires de correspondance en créant des contraintes de routage avec des expressions régulières.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 42c0ce5e158e2fe9387ac218ac0762b6362094f9
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59389569"
---
# <a name="creating-a-route-constraint-c"></a>Création d’une contrainte de route (C#)

par [Stephen Walther](https://github.com/StephenWalther)

> Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont le navigateur demande itinéraires de correspondance en créant des contraintes de routage avec des expressions régulières.


Contraintes de routage vous permet de limiter les demandes du navigateur qui correspondent à un itinéraire particulier. Vous pouvez utiliser une expression régulière pour spécifier une contrainte d’itinéraire.

Par exemple, imaginez que vous avez défini l’itinéraire dans la liste 1 dans votre fichier Global.asax.

**Liste 1 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

Listing 1 contient un itinéraire nommé produit. Vous pouvez utiliser la gamme de produits pour mapper les requêtes de navigateur à le ProductController contenu dans le Listing 2.

**Listing 2 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Notez que l’action Details() exposée par le contrôleur produit accepte un paramètre unique nommé productId. Ce paramètre est un paramètre de type entier.

L’itinéraire défini dans la liste 1 correspond à une des URL suivantes :

- / Produit/23
- / Product/7

Malheureusement, l’itinéraire correspond également les URL suivantes :

- / Product/texte
- / Product/apple

Étant donné que l’action Details() attend un paramètre de type entier, qui effectue une requête qui contient l’autre chose qu’une valeur entière provoque une erreur. Par exemple, si vous tapez l’URL /Product/apple dans votre navigateur, vous obtiendrez la page d’erreur dans la Figure 1.


[![La boîte de dialogue Nouveau projet](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Figure 01**: Voir une page explode ([cliquez pour afficher l’image en taille réelle](creating-a-route-constraint-cs/_static/image2.png))


Ce que vous voulez vraiment faire est uniquement en correspondance les URL qui contiennent un productId entier approprié. Vous pouvez utiliser une contrainte lors de la définition d’un itinéraire pour restreindre les URL qui correspondent à l’itinéraire. L’itinéraire de produit modifié dans la liste 3 contient une contrainte d’expression régulière qui correspond uniquement à des entiers.

**Liste 3 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

L’expression régulière \d+ correspond à un ou plusieurs entiers. Cette contrainte provoque la gamme de produits faire correspondre les URL suivantes :

- / Product/3
- / Product/8999

Mais pas les URL suivantes :

- / Product/apple
- / Produit

- Ces demandes du navigateur seront gérées par un autre itinéraire ou, si aucun itinéraire correspondant, un *Impossible de trouver la ressource* erreur est renvoyée.

> [!div class="step-by-step"]
> [Précédent](creating-custom-routes-cs.md)
> [Suivant](creating-a-custom-route-constraint-cs.md)
