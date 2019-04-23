---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Changement d’une Animation avec du Code côté Client (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 4254b7e1f2086a9cc5fbc1e8c2a4f7e2e3d2925e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59416569"
---
# <a name="changing-an-animation-using-client-side-code-vb"></a>Changement d’une animation avec du code côté client (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également être modifiée à l’aide d’un code JavaScript côté client personnalisé.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également être modifiée à l’aide d’un code JavaScript côté client personnalisé.

## <a name="steps"></a>Étapes

Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

L’animation proprement dite est lancée par un bouton HTML :

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Notez qu’il existe aucune `<Animations>` nœud dans le `AnimationExtender` contrôle. Code JavaScript personnalisé est utilisé pour fournir les animations à utiliser avec le contrôle.

Comme avec l’API du serveur de `AnimationExtender`, il n’existe aucun moyen facile pour assigner une animation pour l’extendeur encore. Toutefois l’extendeur expose plusieurs méthodes pour lire et écrire des animations inscrit avec divers événements (`OnClick`, `OnLoad`, et ainsi de suite). Voici quelques exemples :

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Le format de la valeur de retour de la `get_*()` fonctions et le format de l’argument pour le `set_*()` functions est une chaîne JSON, en fournissant une représentation d’objet de ce que serait le balisage XML. Actuellement, il n’existe aucun moyen de passer un objet dans, mais il est possible de lire un objet à partir d’une animation donnée (`get_OnXXXBehavior()` méthodes).

Voici une chaîne JSON (sans les guillemets de délimitation et bien formatées) représentant une animation déclenchée par le bouton, mais animer le volet en redimensionnant et en atténuant progressivement en même temps :

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

Le code JavaScript suivant assigne cette descripting JSON pour le `OnClick` animation de l’extendeur actuel et l’exécute :

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![L’animation s’exécute immédiatement, sans un clic de souris (et avec très peu de balisage)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

L’animation s’exécute immédiatement, sans un clic de souris (et avec très peu de balisage) ([cliquez pour afficher l’image en taille réelle](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](executing-animations-using-client-side-code-vb.md)
> [Suivant](animating-an-updatepanel-control-vb.md)
