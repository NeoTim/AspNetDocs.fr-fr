---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Changement d’une Animation à l’aide de Code côté Client (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bdee58aa04e1c8217c2a727b96aa8b239fe1aca
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59395601"
---
# <a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="2f6b7-104">Changement d’une animation avec du code côté client (C#)</span><span class="sxs-lookup"><span data-stu-id="2f6b7-104">Changing an Animation Using Client-Side Code (C#)</span></span>

<span data-ttu-id="2f6b7-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2f6b7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2f6b7-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2f6b7-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="2f6b7-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="2f6b7-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2f6b7-108">L’animation peut également être modifiée à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="2f6b7-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="2f6b7-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="2f6b7-109">Overview</span></span>

<span data-ttu-id="2f6b7-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="2f6b7-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2f6b7-111">L’animation peut également être modifiée à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="2f6b7-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="2f6b7-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="2f6b7-112">Steps</span></span>

<span data-ttu-id="2f6b7-113">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="2f6b7-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="2f6b7-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="2f6b7-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="2f6b7-115">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="2f6b7-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="2f6b7-116">L’animation proprement dite est lancée par un bouton HTML :</span><span class="sxs-lookup"><span data-stu-id="2f6b7-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="2f6b7-117">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="2f6b7-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="2f6b7-118">Notez qu’il existe aucune `<Animations>` nœud dans le `AnimationExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="2f6b7-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="2f6b7-119">Code JavaScript personnalisé est utilisé pour fournir les animations à utiliser avec le contrôle.</span><span class="sxs-lookup"><span data-stu-id="2f6b7-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="2f6b7-120">Comme avec l’API du serveur de `AnimationExtender`, il n’existe aucun moyen facile pour assigner une animation pour l’extendeur encore.</span><span class="sxs-lookup"><span data-stu-id="2f6b7-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="2f6b7-121">Toutefois l’extendeur expose plusieurs méthodes pour lire et écrire des animations inscrit avec divers événements (`OnClick`, `OnLoad`, et ainsi de suite).</span><span class="sxs-lookup"><span data-stu-id="2f6b7-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="2f6b7-122">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="2f6b7-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="2f6b7-123">Le format de la valeur de retour de la `get_*()` fonctions et le format de l’argument pour le `set_*()` functions est une chaîne JSON, en fournissant une représentation d’objet de ce que serait le balisage XML.</span><span class="sxs-lookup"><span data-stu-id="2f6b7-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="2f6b7-124">Actuellement, il n’existe aucun moyen de passer un objet dans, mais il est possible de lire un objet à partir d’une animation donnée (`get_OnXXXBehavior()` méthodes).</span><span class="sxs-lookup"><span data-stu-id="2f6b7-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="2f6b7-125">Voici une chaîne JSON (sans les guillemets de délimitation et bien formatées) représentant une animation déclenchée par le bouton, mais animer le volet en redimensionnant et en atténuant progressivement en même temps :</span><span class="sxs-lookup"><span data-stu-id="2f6b7-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="2f6b7-126">Le code JavaScript suivant assigne cette descripting JSON pour le `OnClick` animation de l’extendeur actuel et l’exécute :</span><span class="sxs-lookup"><span data-stu-id="2f6b7-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![T<span data-ttu-id="2f6b7-127">animation de he s’exécute immédiatement, sans un clic de souris (et avec très peu de balisage)]</span><span class="sxs-lookup"><span data-stu-id="2f6b7-127">he animation runs immediately, without a mouse click (and with very little markup)]</span></span>(changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

<span data-ttu-id="2f6b7-128">L’animation s’exécute immédiatement, sans un clic de souris (et avec très peu de balisage) ([cliquez pour afficher l’image en taille réelle](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2f6b7-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2f6b7-129">[Précédent](executing-animations-using-client-side-code-cs.md)
> [Suivant](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2f6b7-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
