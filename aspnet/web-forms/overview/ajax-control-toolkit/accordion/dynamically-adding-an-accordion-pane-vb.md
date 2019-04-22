---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Ajout dynamique d’un volet Accordion (VB) | Microsoft Docs
author: wenz
description: Le contrôle Accordion dans AJAX Control Toolkit fournit plusieurs volets et permet à l’utilisateur afficher un d’eux à la fois. Panneaux sont généralement déclarés w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 27823e6b65dda80391d494af6f8d8da539bc3334
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59393208"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a>Ajout dynamique d’un volet Accordion (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Le contrôle Accordion dans AJAX Control Toolkit fournit plusieurs volets et permet à l’utilisateur afficher un d’eux à la fois. Panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.


## <a name="overview"></a>Vue d'ensemble

Le contrôle Accordion dans AJAX Control Toolkit fournit plusieurs volets et permet à l’utilisateur afficher un d’eux à la fois. Panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.

## <a name="steps"></a>Étapes

Le contrôle Accordion expose toutes les propriétés importantes pour le code côté serveur. Entre autres choses, le `Panes` propriété accorde l’accès à la collection de volets qui composent la Accordion. Chaque volet, il est de type `AccordionPane`. Par conséquent, il est facile de créer un volet de ce type :

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

Le `HeaderContainer` propriété du `AccordionPane` fournit l’accès aux contrôles ASP.NET dans la section d’en-tête du volet ; le `ContentContainer` propriété de `AccordionPane` fait de même pour la section de contenu du volet. Cela permet de code ASP.NET ajouter du contenu dans les volets :

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Enfin, la pane(s) doit être ajouté à la `Panes` collection de l’accordéon :

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Voici un code côté serveur complet qui ajoute deux volets à un contrôle Accordion :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Le seul élément manquant est Accordion lui-même, ce qui dépend de la présence de l’ASP.NET `ScriptManager` contrôle :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Pour terminer l’exemple, les deux classes CSS référencés dans le contrôle Accordion fournissent des informations de style pour le navigateur :

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Les données dans l’accordéon a été ajoutées dynamiquement par le code côté serveur](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Les données dans l’accordéon a été ajoutées dynamiquement par le code côté serveur ([cliquez pour afficher l’image en taille réelle](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](databinding-to-an-accordion-vb.md)
