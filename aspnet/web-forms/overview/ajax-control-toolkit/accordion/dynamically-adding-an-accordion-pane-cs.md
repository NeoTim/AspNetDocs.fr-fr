---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Ajout dynamique d’un volet accordéon (C#) | Microsoft Docs
author: wenz
description: Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 2834f56bd77c412923f4a8f382e670727f70eae4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614463"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a>Ajout dynamique d’un volet accordéon (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.

## <a name="overview"></a>Présentation

Le contrôle accordéon dans le kit d’outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur d’en afficher un à la fois. Les panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.

## <a name="steps"></a>Étapes

Le contrôle accordéon expose toutes les propriétés importantes au code côté serveur. Entre autres choses, la propriété `Panes` accorde l’accès à la collection de volets qui composent l’accordéon. Chaque volet est de type `AccordionPane`. Il est donc trivial de créer un tel volet :

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

La propriété `HeaderContainer` de `AccordionPane` permet d’accéder aux contrôles ASP.NET dans la section d’en-tête du volet. la propriété `ContentContainer` de `AccordionPane` fait de même pour la section content du volet. Cela permet au code ASP.NET d’ajouter du contenu aux volets :

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Enfin, le ou les volets doivent être ajoutés à la collection `Panes` de l’accordéon :

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Voici un code complet côté serveur qui ajoute deux volets à un contrôle accordéon :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Le seul élément manquant est l’accordéon lui-même, qui dépend de la présence du contrôle de `ScriptManager` ASP.NET :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Pour terminer l’exemple, les deux classes CSS référencées dans le contrôle accordéon fournissent des informations de style pour le navigateur :

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]

[![les données de l’accordéon ont été ajoutées dynamiquement par le code côté serveur](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Les données de l’accordéon ont été ajoutées dynamiquement par le code côté serveur ([cliquez pour afficher l’image en taille réelle](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](databinding-to-an-accordion-cs.md)
> [Suivant](databinding-to-an-accordion-vb.md)
