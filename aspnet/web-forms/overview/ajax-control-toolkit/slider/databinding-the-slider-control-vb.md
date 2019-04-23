---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Liaison de données le contrôle Slider (VB) | Microsoft Docs
author: wenz
description: Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris. Il est possible de lier la position en cours...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a60e09b7cdda7f924a4287aab8cda32fef5a53ac
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59419767"
---
# <a name="databinding-the-slider-control-vb"></a>Liaison de données du contrôle Slider (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris. Il est possible de lier la position actuelle du curseur vers un autre contrôle ASP.NET.


## <a name="overview"></a>Vue d'ensemble

Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris. Il est possible de lier la position actuelle du curseur vers un autre contrôle ASP.NET.

## <a name="steps"></a>Étapes

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Ensuite, ajoutez deux `TextBox` contrôles à la page. Un est transformé en un contrôle slider graphique et autres celui contiendra la position du curseur.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

L’étape suivante est déjà la dernière étape. Le `SliderExtender` rend un curseur en dehors de la première zone de texte de contrôle à partir d’ASP.NET AJAX Control Toolkit et met à jour automatiquement la deuxième zone de texte lorsque le curseur de position des modifications. Dans l’ordre pour que cela fonctionne, le `SliderExtender`de `TargetControlID` attribut doit être défini à l’ID de la première zone de texte ; le `BoundControlID` attribut doit être défini à l’ID de la deuxième zone de texte.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Comme vous pouvez le voir dans le navigateur, la liaison de données fonctionne dans les deux sens : entrant une nouvelle valeur dans la zone de texte des mises à jour la position du curseur. Si vous apportez à la deuxième zone de texte en lecture seule, vous pouvez ajouter une protection faible pour le champ de texte afin qu’il soit plus difficile pour l’utilisateur à mettre à jour manuellement la valeur y.


[![Curseur et zone de texte sont synchronisés](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Curseur et zone de texte sont synchronisés ([cliquez pour afficher l’image en taille réelle](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](using-the-slider-control-with-auto-postback-vb.md)
