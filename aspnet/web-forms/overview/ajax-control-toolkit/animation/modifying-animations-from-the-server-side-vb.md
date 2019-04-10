---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modification d’Animations depuis le côté serveur (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: fb7e992246b9c630d99a1493f344c4089540d67e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59398083"
---
# <a name="modifying-animations-from-the-server-side-vb"></a>Modification d’Animations depuis le côté serveur (VB)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également être modifiées sur le côté serveur


## <a name="overview"></a>Vue d'ensemble

Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également être modifiées sur le côté serveur

## <a name="steps"></a>Étapes

Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Le reste du code s’exécute sur le côté serveur et n’utilise pas de balisage ; au lieu de cela, il utilise le code pour créer le `AnimationExtender` contrôle :

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Toutefois, les outils de contrôle actuellement ne fournit pas un accès à l’API pour créer les animations individuelles. Il est toutefois possible de définir le `AnimationExtender`propriété Animations vers une chaîne qui contient le balisage XML utilisé lors de l’attribution de façon déclarative les animations. Pour créer le code XML qui ne doit pas contenir le `<Animations>` élément que vous pouvez utiliser les XML du .NET Framework prend en charge ou, comme dans le code suivant, juste fournir la chaîne :

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Enfin, ajoutez le `AnimationExtender` dans le contrôle à la page active, le `<form runat="server">` élément, s’assurer que l’animation est incluse et s’exécute :

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![Tanimation de he est créée à l’aide de côté serveur C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

L’animation est créée à l’aide au code côté serveur C# /Visual Basic ([cliquez pour afficher l’image en taille réelle](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](triggering-an-animation-in-another-control-vb.md)
> [Suivant](executing-animations-using-client-side-code-vb.md)
