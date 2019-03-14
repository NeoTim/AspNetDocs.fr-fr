---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Sélection d’une liste (c#) une Animation | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Le framework utoriser également...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 9cbbfaefca43a2fb25eec30115bcc4b5112754a9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57057856"
---
<a name="picking-one-animation-out-of-a-list-c"></a>Sélection d’une animation dans une liste (C#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’infrastructure permet également au programmeur de choisir une animation dans une liste d’animations, selon l’évaluation d’un code JavaScript.


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’infrastructure permet également au programmeur de choisir une animation dans une liste d’animations, selon l’évaluation d’un code JavaScript.

## <a name="steps"></a>Étapes

Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée. Au lieu d’un des animations régulières, le `<Case>` élément entre en jeu. La valeur de son attribut SelectScript est évaluée ; la valeur de retour doit être numérique. En fonction de ce nombre, parmi les sous-animations dans &lt;cas&gt; est exécutée. Par exemple, si SelectScript a la valeur 2, les outils de contrôle s’exécute l’animation troisième dans &lt;cas&gt; (comptage démarre à 0).

Le balisage suivant définit trois sous-animations : Redimensionnement de la largeur, le redimensionnement de la hauteur et fondu. Le code JavaScript (`Math.floor(3 * Math.random())`) puis sélectionne un nombre compris entre 0 et 2, afin qu’un des trois animations est exécuté :

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


[![L’une des trois animations possibles : Le panneau obtient plus large](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

L’une des trois animations possibles : Le panneau obtient plus large ([cliquez pour afficher l’image en taille réelle](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](animation-depending-on-a-condition-cs.md)
> [Suivant](animating-in-response-to-user-interaction-cs.md)
