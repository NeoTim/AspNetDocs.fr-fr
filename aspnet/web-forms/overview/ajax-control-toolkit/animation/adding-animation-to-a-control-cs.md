---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Ajout d’une animation à unC#contrôle () | Microsoft Docs
author: wenz
description: Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: dd63157fe616c5f6874b7cca11f4ede15018df04
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614449"
---
# <a name="adding-animation-to-a-control-c"></a>Ajout d’une animation à un contrôle (C#)

par [Christian Wenz](https://github.com/wenz)

[Télécharger le code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment configurer une telle animation.

## <a name="overview"></a>Présentation

Le contrôle d’animation dans ASP.NET AJAX Control Toolkit n’est pas seulement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment configurer une telle animation.

## <a name="steps"></a>Étapes

La première étape consiste à inclure la `ScriptManager` dans la page de sorte que la bibliothèque AJAX ASP.NET soit chargée et que la boîte à outils de contrôle puisse être utilisée :

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

L’animation dans ce scénario sera appliquée à un panneau de texte qui ressemble à ceci :

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

La classe CSS associée pour le panneau définit une couleur d’arrière-plan et une largeur :

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Ensuite, nous avons besoin de la `AnimationExtender`. Après avoir fourni un `ID` et le `runat="server"`habituel, l’attribut `TargetControlID` doit être défini sur le contrôle à animer dans notre cas, le panneau :

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

L’animation entière est appliquée de manière déclarative, à l’aide d’une syntaxe XML, malheureusement pas entièrement prise en charge par IntelliSense de Visual Studio. Le nœud racine est `<Animations>;` dans ce nœud, plusieurs événements sont autorisés, qui déterminent à quel moment la ou les animations prennent en place :

- `OnClick` (clic de souris)
- `OnHoverOut` (lorsque la souris quitte un contrôle)
- `OnHoverOver` (lorsque la souris pointe sur un contrôle, ce qui arrête l’animation `OnHoverOut`)
- `OnLoad` (lorsque la page a été chargée)
- `OnMouseOut` (lorsque la souris quitte un contrôle)
- `OnMouseOver` (lorsque la souris pointe sur un contrôle, sans arrêter l’animation `OnMouseOut`)

Le Framework est fourni avec un ensemble d’animations, chacun représenté par son propre élément XML. Voici une sélection :

- `<Color>` (modification d’une couleur)
- `<FadeIn>` (fondu)
- `<FadeOut>` (fondu)
- `<Property>` (modification de la propriété d’un contrôle)
- `<Pulse>` (Pulsating)
- `<Resize>` (modification de la taille)
- `<Scale>` (modification proportionnelle de la taille)

Dans cet exemple, le panneau s’estompe. L’animation prend 1,5 secondes (attribut`Duration`), en affichant 24 images (étapes d’animation) par seconde (attribut`Fps`). Voici le balisage complet pour le contrôle `AnimationExtender` :

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Lorsque vous exécutez ce script, le panneau s’affiche et disparaît en fondu d’une à deux secondes.

[![le panneau est en fondu](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Le panneau est en fondu ([cliquez pour afficher l’image en taille réelle](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](executing-several-animations-at-the-same-time-cs.md)
