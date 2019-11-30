---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Création de cases à cocher mutuellement exclusives (C#) | Microsoft Docs
author: wenz
description: 'Lorsque seul un ensemble d’options peut être sélectionné, les cases d’option sont généralement utilisées. Toutefois, il existe un inconvénient : une fois qu’une case d’option est sélectionnée dans un groupe,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606500"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="f8836-104">Création de cases à cocher mutuellement exclusives (C#)</span><span class="sxs-lookup"><span data-stu-id="f8836-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>

<span data-ttu-id="f8836-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f8836-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f8836-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f8836-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="f8836-107">Lorsque seul un ensemble d’options peut être sélectionné, les cases d’option sont généralement utilisées.</span><span class="sxs-lookup"><span data-stu-id="f8836-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="f8836-108">Toutefois, il existe un inconvénient : une fois qu’une case d’option est sélectionnée dans un groupe, il n’est pas possible de décocher toutes les cases d’option.</span><span class="sxs-lookup"><span data-stu-id="f8836-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="f8836-109">Les cases à cocher peuvent être désactivées à tout moment, mais ne sont pas mutuellement exclusives.</span><span class="sxs-lookup"><span data-stu-id="f8836-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="f8836-110">Ce didacticiel offre le meilleur des deux approches : les cases à cocher qui s’excluent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="f8836-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="f8836-111">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="f8836-111">Overview</span></span>

<span data-ttu-id="f8836-112">Lorsque seul un ensemble d’options peut être sélectionné, les cases d’option sont généralement utilisées.</span><span class="sxs-lookup"><span data-stu-id="f8836-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="f8836-113">Toutefois, il existe un inconvénient : une fois qu’une case d’option est sélectionnée dans un groupe, il n’est pas possible de décocher toutes les cases d’option.</span><span class="sxs-lookup"><span data-stu-id="f8836-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="f8836-114">Les cases à cocher peuvent être désactivées à tout moment, mais ne sont pas mutuellement exclusives.</span><span class="sxs-lookup"><span data-stu-id="f8836-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="f8836-115">Ce didacticiel offre le meilleur des deux approches : les cases à cocher qui s’excluent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="f8836-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="f8836-116">Étapes</span><span class="sxs-lookup"><span data-stu-id="f8836-116">Steps</span></span>

<span data-ttu-id="f8836-117">ASP.NET AJAX Control Toolkit contient l’extendeur MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="f8836-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="f8836-118">Cela permet aux programmeurs d’attribuer n’importe quelle case à un nom de groupe (attribut`Key`).</span><span class="sxs-lookup"><span data-stu-id="f8836-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="f8836-119">À partir de toutes les cases au sein du même groupe, une seule peut être sélectionnée à la fois.</span><span class="sxs-lookup"><span data-stu-id="f8836-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="f8836-120">Commençons par placer deux cases à cocher sur une nouvelle page ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f8836-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="f8836-121">Il peut y en avoir davantage, mais deux d’entre eux suffisent à démontrer le principe :</span><span class="sxs-lookup"><span data-stu-id="f8836-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="f8836-122">Pour les deux cases à cocher, un contrôle MutuallyExclusiveCheckBoxExtender doit être placé sur la page.</span><span class="sxs-lookup"><span data-stu-id="f8836-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="f8836-123">Les deux attributs de clé doivent avoir la même valeur, tout comme les attributs de valeur des éléments de case d’option HTML doivent être identiques pour désigner le groupe auquel ils appartiennent.</span><span class="sxs-lookup"><span data-stu-id="f8836-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="f8836-124">La propriété TargetControlID de l’extendeur pointe sur l’ID de la case à cocher.</span><span class="sxs-lookup"><span data-stu-id="f8836-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="f8836-125">Enfin, incluez le `ScriptManager` AJAX ASP.NET qui est requis par tous les éléments de ASP.NET AJAX Control Toolkit :</span><span class="sxs-lookup"><span data-stu-id="f8836-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="f8836-126">Enregistrer et exécuter la page : vous pouvez activer ou désactiver les deux cases à cocher. Toutefois, à aucun moment, les deux cases à cocher peuvent être activées.</span><span class="sxs-lookup"><span data-stu-id="f8836-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="f8836-127">[![une seule case à cocher peut être activée à la fois](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f8836-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="f8836-128">Une seule case à cocher peut être activée à la fois ([cliquez pour afficher l’image en taille réelle](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f8836-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f8836-129">Suivant</span><span class="sxs-lookup"><span data-stu-id="f8836-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
