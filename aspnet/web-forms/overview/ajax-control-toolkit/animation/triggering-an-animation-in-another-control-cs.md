---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Déclenchement d’une Animation dans un autre contrôle (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En règle générale, en lançant un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ca383b7a82b754c7556dcea3bcdb8e28e5c7a45d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59384849"
---
# <a name="triggering-an-animation-in-another-control-c"></a>Déclenchement d’une animation dans un autre contrôle (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En règle générale, le lancement d’une animation est déclenchée par l’interaction utilisateur avec le même contrôle. Il est cependant également possible d’interagir avec un contrôle, puis d’animation à un autre contrôle.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En règle générale, le lancement d’une animation est déclenchée par l’interaction utilisateur avec le même contrôle. Il est cependant également possible d’interagir avec un contrôle, puis d’animation à un autre contrôle.

## <a name="steps"></a>Étapes

Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Pour commencer à animer le panneau de configuration, un bouton HTML est utilisé. Notez que `<input type="button" />` est favorisée sur `<asp:Button />` dans la mesure où nous ne voulons pas une publication (postback) lorsque l’utilisateur clique sur ce bouton.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`. Il est important de définir `TargetControlID` à l’ID du bouton (l’élément déclencher l’animation), pas à l’ID du panneau (l’élément en cours d’animation)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

Dans le `<Animations>` nœud, les animations sur place comme d’habitude. Afin de les mettre à modifier le panneau de configuration, pas le bouton, définissez la `AnimationTarget` attribut pour chaque élément d’animation dans `AnimationExtender`. La valeur de `AnimationTarget` est l’ID du panneau, bien sûr. De cette façon, les animations se produisent avec le panneau, pas avec le bouton de déclenchement. Voici le `AnimationExtender` balisage pour ce scénario :

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Notez l’ordre spécial dans lequel apparaissent les animations individuelles. Tout d’abord, le bouton est désactivé une fois que l’animation s’exécute. Dans la mesure où il existe aucune `AnimationTarget` d’attribut dans le `<EnableAction>` élément, cette animation est appliquée pour le contrôle d’origine : le bouton. Les étapes de le deux animation sont effectués en parallèle (`<Parallel>` élément). Les deux ont leur `AnimationTarget` les attributs définis pour `"Panel1"`, animer ainsi le panneau de configuration, et pas sur le bouton.


[![A clic de souris sur le bouton démarre l’animation du Panneau de configuration](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Un clic de souris sur le bouton démarre l’animation du Panneau de configuration ([cliquez pour afficher l’image en taille réelle](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](disabling-actions-during-animation-cs.md)
> [Suivant](modifying-animations-from-the-server-side-cs.md)
