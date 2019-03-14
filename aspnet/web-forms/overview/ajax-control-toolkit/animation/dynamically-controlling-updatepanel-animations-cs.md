---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Contrôle dynamique des Animations UpdatePanel (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 4de43cca95a37270c752d57f39940339b5bb2e3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048176"
---
<a name="dynamically-controlling-updatepanel-animations-c"></a>Contrôle dynamique des animations UpdatePanel (C#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un UpdatePanel, un extendeur spécial existe qui s’appuie fortement sur l’infrastructure d’animation : UpdatePanelAnimation. Il est également utilisable avec des déclencheurs UpdatePanel.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un `UpdatePanel`, un extendeur spécial existe qui s’appuie fortement sur l’infrastructure d’animation : `UpdatePanelAnimation`. Il peut également fonctionner conjointement avec `UpdatePanel` déclencheurs.

## <a name="steps"></a>Étapes

La première étape consiste comme d’habitude à inclure le `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

L’animation dans ce scénario s’appliqueront à un affichage de l’heure actuelle. Ces informations peuvent être écrits dans une étiquette à l’aide de la `Page_Load()` (méthode), ou (par souci de simplicité) le code inline suivant est utilisé :


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

En outre, un bouton pour déclencher la mise à jour de l’heure est créé :


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Ce code est ensuite placé dans le `<ContentTemplate>` section d’un `UpdatePanel` élément. Le panneau `UpdateMode` attribut doit être défini sur `"Conditional"`, étant donné que seuls les déclencheurs peuvent mettre à jour le contenu du panneau. Dans le `<Triggers>` section de la `UpdatePanel`, un déclencheur de publication (postback) asynchrone est créé et lié à la `Click` événements du bouton. Par conséquent, si l’utilisateur clique sur le bouton, le `UpdatePanel` est actualisé. Voici le balisage pour le `UpdatePanel` contrôle :


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Enfin, le `UpdatePanelAnimationExtender` doivent être configurés : Définir le `TargetControlID` d’attribut à l’ID du panneau et définissez une animation au sein de l’extendeur. Effet de fondu rend sens, ce qui crée une agréable visual mettant l’accent sur l’heure de mise à jour. Votre balisage d’extendeur peut ensuite ressembler à ceci :


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Exécutez le fichier dans le navigateur. Chaque fois que vous cliquez sur le bouton, l’heure actuelle est indiqué dans le panneau de configuration, toujours fondu pour la durée d’une seconde.


[![L’heure actuelle est fondu](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

L’heure actuelle est fondu ([cliquez pour afficher l’image en taille réelle](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](animating-an-updatepanel-control-cs.md)
> [Suivant](adding-animation-to-a-control-vb.md)
