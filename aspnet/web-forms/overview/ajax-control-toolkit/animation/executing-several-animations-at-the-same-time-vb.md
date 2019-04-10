---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Exécution de plusieurs Animations en même temps (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter la chute...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: f8bb91de9642814a79d0fddd642928c25c58ebfd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59402841"
---
# <a name="executing-several-animations-at-the-same-time-vb"></a>Exécution de plusieurs Animations en même temps (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter plusieurs animations de manière parallèle.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter plusieurs animations de manière parallèle.

## <a name="steps"></a>Étapes

Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée. En règle générale, `<OnLoad>` accepte uniquement une animation. Le framework d’Animation vous permet de joindre plusieurs animations en un seul via le `<Parallel>` élément. Toutes les animations dans `<Parallel>` sont exécutées en même temps.

Voici l’un balisage possible pour le `AnimationExtender` contrôle, atténuant progressivement et redimensionner le panneau en même temps :

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

Et effectivement : lorsque vous exécutez ce script, le panneau s’affiche, puis redimensionne (plus de triple de sa largeur et sa hauteur de réduisant de moitié) et fondu en même temps.


[![TPanneau de configuration he est fondu et redimensionnement (y compris son contenu, grâce à moteur de rendu du navigateur)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)

Le panneau est fondu et redimensionnement (y compris son contenu, grâce à moteur de rendu du navigateur) ([cliquez pour afficher l’image en taille réelle](executing-several-animations-at-the-same-time-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](adding-animation-to-a-control-vb.md)
> [Suivant](executing-several-animations-after-each-other-vb.md)
