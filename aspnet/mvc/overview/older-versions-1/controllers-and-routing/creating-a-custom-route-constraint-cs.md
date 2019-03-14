---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Création d’une contrainte de Route personnalisée (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther montre comment vous pouvez créer une contrainte d’itinéraire personnalisé. Nous implémentons un simple contrainte personnalisée qui empêche un itinéraire mis en correspondance w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 0a0b6b706fdb212a745346ffaefc118e85c2a245
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043096"
---
<a name="creating-a-custom-route-constraint-c"></a>Création d’une contrainte de route personnalisée (C#)
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther montre comment vous pouvez créer une contrainte d’itinéraire personnalisé. Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire à partir de la mise en correspondance lors de l’exécution d’une demande de navigateur à partir d’un ordinateur distant.


L’objectif de ce didacticiel consiste à montrer comment vous pouvez créer une contrainte d’itinéraire personnalisé. Une contrainte d’itinéraire personnalisé vous pouvez ainsi empêcher un itinéraire à partir de la mise en correspondance, sauf si une condition personnalisée est mis en correspondance.

Dans ce didacticiel, créons une contrainte de route de Localhost. La contrainte d’itinéraire Localhost correspond uniquement aux requêtes effectuées à partir de l’ordinateur local. Les demandes à distance à partir de sur Internet ne correspondent pas.

Vous implémentez une contrainte d’itinéraire personnalisée en implémentant l’interface IRouteConstraint. Il s’agit d’une interface très simple qui décrit une méthode unique :

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

La méthode retourne une valeur booléenne. Si vous retournez la valeur false, l’itinéraire associé à la contrainte ne correspondra à la demande de navigateur.

La contrainte de Localhost est contenue dans le Listing 1.

**Liste 1 - LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

La contrainte dans le Listing 1 tire parti de la propriété IsLocal exposée par la classe HttpRequest. Cette propriété retourne la valeur true lorsque l’adresse IP de la demande est soit 127.0.0.1 ou lorsque l’adresse IP de la demande est identique à l’adresse IP du serveur.

Vous utilisez une contrainte personnalisée au sein d’un itinéraire défini dans le fichier Global.asax. Le fichier Global.asax dans le Listing 2 utilise la contrainte de Localhost pour empêcher toute personne demandant une page d’administration, à moins qu’ils envoient la demande à partir du serveur local. Par exemple, une demande de /Admin/DeleteAll échouera lorsque effectuées à partir d’un serveur distant.

**Listing 2 - Global.asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

La contrainte de Localhost est utilisée dans la définition de l’itinéraire de l’administrateur. Cet itinéraire ne doit correspondre à une demande de navigateur à distance. Sachez, cependant, que les autres itinéraires définis dans Global.asax peuvent correspondent à la même demande. Il est important de comprendre qu’une contrainte empêche un itinéraire particulier à partir d’une demande de mise en correspondance, et pas tous les itinéraires définis dans le fichier Global.asax.

Notez que l’itinéraire par défaut a été commenté à partir du fichier Global.asax dans le Listing 2. Si vous incluez l’itinéraire par défaut, l’itinéraire par défaut serait correspond aux requêtes pour le contrôleur de l’administrateur. Dans ce cas, les utilisateurs distants peuvent toujours invoquer des actions du contrôleur Admin même si leurs demandes ne correspondent pas l’itinéraire de l’administrateur.

> [!div class="step-by-step"]
> [Précédent](creating-a-route-constraint-cs.md)
> [Suivant](asp-net-mvc-controller-overview-vb.md)
