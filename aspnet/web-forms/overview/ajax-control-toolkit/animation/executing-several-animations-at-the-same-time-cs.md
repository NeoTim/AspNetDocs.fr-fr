---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Exécution de plusieurs Animations en même temps (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter la chute...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: d70f7b9170cbd3307dae4cdb4f9ee735e3c5bee8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032766"
---
<a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="1452f-104">Exécution de plusieurs Animations en même temps (c#)</span><span class="sxs-lookup"><span data-stu-id="1452f-104">Executing Several Animations at The Same Time (C#)</span></span>
====================
<span data-ttu-id="1452f-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1452f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1452f-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1452f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="1452f-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="1452f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1452f-108">Il permet d’exécuter plusieurs animations de manière parallèle.</span><span class="sxs-lookup"><span data-stu-id="1452f-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="1452f-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="1452f-109">Overview</span></span>

<span data-ttu-id="1452f-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="1452f-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1452f-111">Il permet d’exécuter plusieurs animations de manière parallèle.</span><span class="sxs-lookup"><span data-stu-id="1452f-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="1452f-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="1452f-112">Steps</span></span>

<span data-ttu-id="1452f-113">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="1452f-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="1452f-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="1452f-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="1452f-115">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="1452f-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="1452f-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="1452f-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="1452f-117">Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="1452f-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="1452f-118">En règle générale, `<OnLoad>` accepte uniquement une animation.</span><span class="sxs-lookup"><span data-stu-id="1452f-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="1452f-119">Le framework d’Animation vous permet de joindre plusieurs animations en un seul via le `<Parallel>` élément.</span><span class="sxs-lookup"><span data-stu-id="1452f-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="1452f-120">Toutes les animations dans `<Parallel>` sont exécutées en même temps.</span><span class="sxs-lookup"><span data-stu-id="1452f-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="1452f-121">Voici l’un balisage possible pour le `AnimationExtender` contrôle, atténuant progressivement et redimensionner le panneau en même temps :</span><span class="sxs-lookup"><span data-stu-id="1452f-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="1452f-122">Et effectivement : lorsque vous exécutez ce script, le panneau s’affiche, puis redimensionne (plus que threefolding sa largeur et sa halfing sa hauteur), puis disparaît via en même temps.</span><span class="sxs-lookup"><span data-stu-id="1452f-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="1452f-123">[![Le panneau est fondu et redimensionnement (y compris son contenu, grâce à moteur de rendu du navigateur)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1452f-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span></span>

<span data-ttu-id="1452f-124">Le panneau est fondu et redimensionnement (y compris son contenu, grâce à moteur de rendu du navigateur) ([cliquez pour afficher l’image en taille réelle](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1452f-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1452f-125">[Précédent](adding-animation-to-a-control-cs.md)
> [Suivant](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1452f-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
