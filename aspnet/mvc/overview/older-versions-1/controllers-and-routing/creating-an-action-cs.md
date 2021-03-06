---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Création d’une action (C) Microsoft Docs
author: rick-anderson
description: Découvrez comment ajouter une nouvelle action à un contrôleur MVC ASP.NET. Renseignez-vous sur les exigences d’une méthode d’action.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c1edfbab41afa58c7cb2c0d306995e9fe491c90
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542272"
---
# <a name="creating-an-action-c"></a>Création d’une action (C#)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment ajouter une nouvelle action à un contrôleur MVC ASP.NET. Renseignez-vous sur les exigences d’une méthode d’action.

Le but de ce tutoriel est d’expliquer comment vous pouvez créer une nouvelle action de contrôleur. Vous en apprendrez davantage sur les exigences d’une méthode d’action. Vous apprenez également à empêcher une méthode d’être exposée comme une action.

## <a name="adding-an-action-to-a-controller"></a>Ajout d’une action à un contrôleur

Vous ajoutez une nouvelle action à un contrôleur en ajoutant une nouvelle méthode au contrôleur. Par exemple, le contrôleur dans la liste 1 contient une action nommée Index() et une action nommée SayHello(). Les deux méthodes sont exposées comme des actions.

**Liste 1 - Controllers-HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

Pour être exposée à l’univers comme une action, une méthode doit répondre à certaines exigences :

- La méthode doit être publique.
- La méthode ne peut pas être une méthode statique.
- La méthode ne peut pas être une méthode d’extension.
- La méthode ne peut pas être un constructeur, getter, ou setter.
- La méthode ne peut pas avoir des types génériques ouverts.
- La méthode n’est pas une méthode de la classe de base du contrôleur.
- La méthode ne peut pas contenir des paramètres **ref** ou **out.**

Notez qu’il n’y a aucune restriction sur le type de retour d’une action de contrôleur. Une action de contrôleur peut retourner une chaîne, un DateTime, une instance de la classe Random, ou vide. Le cadre MVC ASP.NET convertira tout type de retour qui n’est pas un résultat d’action en chaîne et rendra la chaîne au navigateur.

Lorsque vous ajoutez une méthode qui ne viole pas ces exigences à un contrôleur, la méthode est exposée comme une action de contrôleur. Fais attention ici. Une action de contrôleur peut être invoquée par toute personne connectée à Internet. Ne créez pas, par exemple, une action de contrôleur DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Empêcher l’invoquée d’une méthode publique

Si vous avez besoin de créer une méthode publique dans une classe de contrôleur et que vous ne voulez pas exposer la méthode comme une action de contrôleur, alors vous pouvez empêcher la méthode d’être invoquée en utilisant l’attribut [NonAction]. Par exemple, le contrôleur de la liste 2 contient une méthode publique nommée CompanySecrets() qui est décorée de l’attribut [NonAction].

**Liste 2 - Controllers-WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Si vous tentez d’invoquer l’action du contrôleur CompanySecrets() en tapant /Work/CompanySecrets dans la barre d’adresse de votre navigateur, vous recevrez le message d’erreur dans la figure 1.

[![Invoquer une méthode de non-action](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figure 01**: Invoquer une méthode non-action ([Cliquez pour voir l’image grandeur nature](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Suivant précédent](creating-a-controller-cs.md)
> [Next](asp-net-mvc-routing-overview-vb.md)
