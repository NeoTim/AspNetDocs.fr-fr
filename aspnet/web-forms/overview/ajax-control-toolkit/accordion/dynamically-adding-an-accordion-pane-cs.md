---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Ajout dynamique d’un volet Accordion (c#) | Microsoft Docs
author: wenz
description: Le contrôle Accordion dans AJAX Control Toolkit fournit plusieurs volets et permet à l’utilisateur afficher un d’eux à la fois. Panneaux sont généralement déclarés w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: ea526ce8abdf6f7013e8dd832824c21448878e0b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59416842"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a>Ajout dynamique d’un volet Accordion (c#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Le contrôle Accordion dans AJAX Control Toolkit fournit plusieurs volets et permet à l’utilisateur afficher un d’eux à la fois. Panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.


## <a name="overview"></a>Vue d'ensemble

Le contrôle Accordion dans AJAX Control Toolkit fournit plusieurs volets et permet à l’utilisateur afficher un d’eux à la fois. Panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.

## <a name="steps"></a>Étapes

Le contrôle Accordion expose toutes les propriétés importantes pour le code côté serveur. Entre autres choses, le `Panes` propriété accorde l’accès à la collection de volets qui composent la Accordion. Chaque volet, il est de type `AccordionPane`. Par conséquent, il est facile de créer un volet de ce type :

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

Le `HeaderContainer` propriété du `AccordionPane` fournit l’accès aux contrôles ASP.NET dans la section d’en-tête du volet ; le `ContentContainer` propriété de `AccordionPane` fait de même pour la section de contenu du volet. Cela permet de code ASP.NET ajouter du contenu dans les volets :

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Enfin, la pane(s) doit être ajouté à la `Panes` collection de l’accordéon :

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Voici un code côté serveur complet qui ajoute deux volets à un contrôle Accordion :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Le seul élément manquant est Accordion lui-même, ce qui dépend de la présence de l’ASP.NET `ScriptManager` contrôle :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Pour terminer l’exemple, les deux classes CSS référencés dans le contrôle Accordion fournissent des informations de style pour le navigateur :

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![Les données dans l’accordéon a été ajoutées dynamiquement par le code côté serveur](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Les données dans l’accordéon a été ajoutées dynamiquement par le code côté serveur ([cliquez pour afficher l’image en taille réelle](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](databinding-to-an-accordion-cs.md)
> [Suivant](databinding-to-an-accordion-vb.md)
