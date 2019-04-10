---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Exécution d’Animations avec du Code côté Client (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’exécution de l’animation...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: ff143aa102973279c53fe4ba052c4766f099c77d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59382210"
---
# <a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="4074a-104">Exécution d’animations avec du code côté client (VB)</span><span class="sxs-lookup"><span data-stu-id="4074a-104">Executing Animations Using Client-Side Code (VB)</span></span>

<span data-ttu-id="4074a-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4074a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4074a-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4074a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="4074a-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="4074a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4074a-108">L’exécution de l’animation peut également être déclenchée à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="4074a-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="4074a-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="4074a-109">Overview</span></span>

<span data-ttu-id="4074a-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="4074a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4074a-111">L’exécution de l’animation peut également être déclenchée à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="4074a-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="4074a-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="4074a-112">Steps</span></span>

<span data-ttu-id="4074a-113">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="4074a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="4074a-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="4074a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="4074a-115">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="4074a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="4074a-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="4074a-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="4074a-117">Dans le `<Animations>` nœud, utilisez `<OnClick>` pour exécuter les animations une fois l’utilisateur clique sur le panneau de configuration.</span><span class="sxs-lookup"><span data-stu-id="4074a-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="4074a-118">Ajoutez deux animations pour être exécutés en parallèle :</span><span class="sxs-lookup"><span data-stu-id="4074a-118">Add two animations to be executed in parallel:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="4074a-119">Fins de démonstration, cette animation (et les autres animations créées à l’aide de la boîte à outils de contrôle) sont exécutées à l’aide de code JavaScript, une fois que la page s’exécute.</span><span class="sxs-lookup"><span data-stu-id="4074a-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="4074a-120">Tout d’abord, nous devons accéder à la `AnimationExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="4074a-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="4074a-121">La bibliothèque ASP.NET AJAX fournit le `$find()` (fonction) pour cette tâche :</span><span class="sxs-lookup"><span data-stu-id="4074a-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="4074a-122">Le `AnimationExtender` contrôle expose une API riche, y compris les méthodes avec des noms identiques aux gestionnaires d’événements utilisés dans le balisage XML : `OnClick()`, `OnLoad()`, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="4074a-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="4074a-123">Par exemple, un appel de la `OnClick()` méthode exécute l’animation dans le `<OnClick>` élément de la `AnimationExtender` contrôle :</span><span class="sxs-lookup"><span data-stu-id="4074a-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="4074a-124">Voici le code JavaScript côté client complet qui émule le, cliquez sur le panneau de configuration une fois que la page a été entièrement chargée Notez que le `pageLoad()` nom de la fonction est utilisé, qui est appelée par ASP.NET AJAX une fois la page et JavaScript bibliothèques ont été inclus chargé.</span><span class="sxs-lookup"><span data-stu-id="4074a-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![T<span data-ttu-id="4074a-125">animation de he s’exécute immédiatement, sans un clic de souris]</span><span class="sxs-lookup"><span data-stu-id="4074a-125">he animation runs immediately, without a mouse click]</span></span>(executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

<span data-ttu-id="4074a-126">L’animation s’exécute immédiatement, sans un clic de souris ([cliquez pour afficher l’image en taille réelle](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4074a-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4074a-127">[Précédent](modifying-animations-from-the-server-side-vb.md)
> [Suivant](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4074a-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
