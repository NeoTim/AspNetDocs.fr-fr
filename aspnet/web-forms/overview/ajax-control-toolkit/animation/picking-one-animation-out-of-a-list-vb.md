---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Sélection d’une liste (VB) une Animation | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Le framework utoriser également...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c60d7cff7c841d23185fbdf07abf0e894b21cf5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029496"
---
<a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="a14b5-104">Sélection d’une animation dans une liste (VB)</span><span class="sxs-lookup"><span data-stu-id="a14b5-104">Picking One Animation Out Of a List (VB)</span></span>
====================
<span data-ttu-id="a14b5-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a14b5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a14b5-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a14b5-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="a14b5-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="a14b5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a14b5-108">L’infrastructure permet également au programmeur de choisir une animation dans une liste d’animations, selon l’évaluation d’un code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a14b5-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a14b5-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="a14b5-109">Overview</span></span>

<span data-ttu-id="a14b5-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="a14b5-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a14b5-111">L’infrastructure permet également au programmeur de choisir une animation dans une liste d’animations, selon l’évaluation d’un code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a14b5-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a14b5-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="a14b5-112">Steps</span></span>

<span data-ttu-id="a14b5-113">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="a14b5-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="a14b5-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="a14b5-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="a14b5-115">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="a14b5-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="a14b5-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="a14b5-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="a14b5-117">Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="a14b5-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="a14b5-118">Au lieu d’un des animations régulières, le `<Case>` élément entre en jeu.</span><span class="sxs-lookup"><span data-stu-id="a14b5-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="a14b5-119">La valeur de son attribut SelectScript est évaluée ; la valeur de retour doit être numérique.</span><span class="sxs-lookup"><span data-stu-id="a14b5-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="a14b5-120">En fonction de ce nombre, parmi les sous-animations dans &lt;cas&gt; est exécutée.</span><span class="sxs-lookup"><span data-stu-id="a14b5-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="a14b5-121">Par exemple, si SelectScript a la valeur 2, les outils de contrôle s’exécute l’animation troisième dans &lt;cas&gt; (comptage démarre à 0).</span><span class="sxs-lookup"><span data-stu-id="a14b5-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="a14b5-122">Le balisage suivant définit trois sous-animations : Redimensionnement de la largeur, le redimensionnement de la hauteur et fondu. Le code JavaScript (`Math.floor(3 * Math.random())`) puis sélectionne un nombre compris entre 0 et 2, afin qu’un des trois animations est exécuté :</span><span class="sxs-lookup"><span data-stu-id="a14b5-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


<span data-ttu-id="a14b5-123">[![L’une des trois animations possibles : Le panneau obtient plus large](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a14b5-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="a14b5-124">L’une des trois animations possibles : Le panneau obtient plus large ([cliquez pour afficher l’image en taille réelle](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a14b5-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a14b5-125">[Précédent](animation-depending-on-a-condition-vb.md)
> [Suivant](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a14b5-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
