---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulation des propriétés de DropShadow depuis du Code Client (c#) | Microsoft Docs
author: wenz
description: Personnalisation de l’Interface de contrôle DataList modification
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 3bf4b8fe85780135c821fbb7fcceefd326dce656
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381339"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a>Manipulation des propriétés de DropShadow à partir de code client (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée. Propriétés de cet extendeur peuvent également être modifiées à l’aide de code JavaScript client.


## <a name="overview"></a>Vue d'ensemble

Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée. Propriétés de cet extendeur peuvent également être modifiées à l’aide de code JavaScript client.

## <a name="steps"></a>Étapes

Le code commence par un panneau contenant des lignes de texte :

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

La classe CSS associée donne le volet d’une couleur d’arrière-plan intéressante :

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

Le `DropShadowExtender` est ajouté pour étendre le panneau avec un opacité définie sur 50 %, effet d’ombre portée :

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Ensuite, ASP.NET AJAX `ScriptManager` contrôle permet de la boîte à outils de contrôle fonctionne :

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Un autre panneau contient deux liens de JavaScript pour définir l’opacité de l’ombre : le lien moins diminue l’opacité de l’ombre, le lien plus (+) pour l’augmenter.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

La fonction JavaScript `changeOpacity()` doit ensuite d’abord trouver le `DropShadowExtender` contrôle sur la page. ASP.NET AJAX définit le `$find()` méthode pour exactement cette tâche. Ensuite, le `get_Opacity()` méthode récupère l’opacité en cours, le `set_Opacity()` méthode il définit. Le code JavaScript puis place la valeur d’opacité actuelle dans le `<label>` élément :

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![Topacité de he est modifiée du côté client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

L’opacité est modifiée du côté client ([cliquez pour afficher l’image en taille réelle](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Suivant](adjusting-the-z-index-of-a-dropshadow-vb.md)
