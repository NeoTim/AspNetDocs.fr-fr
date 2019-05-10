---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Ajustement de l’Index-Z d’un DropShadow (c#) | Microsoft Docs
author: wenz
description: Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée. Toutefois cette ombre parfois est en conflit avec d’autres contrôles, pour le programme d’insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 21ad1ec314be68f7285c044d5e90c21c201a90ef
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65132159"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="8ddf3-104">Ajustement de l’index-Z d’un DropShadow (C#)</span><span class="sxs-lookup"><span data-stu-id="8ddf3-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>

<span data-ttu-id="8ddf3-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8ddf3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8ddf3-106">[Télécharger le Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8ddf3-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="8ddf3-107">Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="8ddf3-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="8ddf3-108">Toutefois cette ombre parfois entre en conflit avec d’autres contrôles, par exemple le contrôle de Menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8ddf3-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="8ddf3-109">Quand une entrée de menu s’affiche, il apparaît derrière l’ombre portée.</span><span class="sxs-lookup"><span data-stu-id="8ddf3-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="overview"></a><span data-ttu-id="8ddf3-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="8ddf3-110">Overview</span></span>

<span data-ttu-id="8ddf3-111">Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="8ddf3-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="8ddf3-112">Toutefois cette ombre parfois entre en conflit avec d’autres contrôles, par exemple le contrôle de Menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8ddf3-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="8ddf3-113">Quand une entrée de menu s’affiche, il apparaît derrière l’ombre portée.</span><span class="sxs-lookup"><span data-stu-id="8ddf3-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="8ddf3-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="8ddf3-114">Steps</span></span>

<span data-ttu-id="8ddf3-115">Le code commence avec le panneau proprement dit, contenant suffisamment de texte afin que le panneau contient suffisamment de texte pour l’effet soit visible :</span><span class="sxs-lookup"><span data-stu-id="8ddf3-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="8ddf3-116">Un autre panneau est placé directement avant le `panelShadow` Panneau de configuration.</span><span class="sxs-lookup"><span data-stu-id="8ddf3-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="8ddf3-117">Il contient un menu avec l’orientation horizontale afin que les entrées de menu s’affiche au-dessus (ou plutôt : sous) le `dropShadow` Panneau de configuration) :</span><span class="sxs-lookup"><span data-stu-id="8ddf3-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="8ddf3-118">Ensuite, le `DropShadowExtender` est ajouté pour étendre le `panelShadow` panneau avec un effet d’ombre portée :</span><span class="sxs-lookup"><span data-stu-id="8ddf3-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="8ddf3-119">Enfin, ASP.NET AJAX `ScriptManager` contrôle permet de la boîte à outils de contrôle fonctionne :</span><span class="sxs-lookup"><span data-stu-id="8ddf3-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="8ddf3-120">Lorsque vous exécutez ce script, les entrées de menu apparaissent sous le panneau de configuration.</span><span class="sxs-lookup"><span data-stu-id="8ddf3-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="8ddf3-121">Toutefois le menu utilise la classe CSS `panel` où vous devez simplement définir deux choses pour rendre les éléments apparaissent devant l’autre panneau :</span><span class="sxs-lookup"><span data-stu-id="8ddf3-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="8ddf3-122">Positionnement relatif</span><span class="sxs-lookup"><span data-stu-id="8ddf3-122">Relative positioning</span></span>
- <span data-ttu-id="8ddf3-123">Un index-z positif</span><span class="sxs-lookup"><span data-stu-id="8ddf3-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="8ddf3-124">Ensuite, le `DropShadowExtender` contrôle n’est pas en conflit plus avec le contrôle de Menu.</span><span class="sxs-lookup"><span data-stu-id="8ddf3-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>

<span data-ttu-id="8ddf3-125">[![Avant : L’entrée de menu n’est pas visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8ddf3-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="8ddf3-126">Avant : L’entrée de menu n’est pas visible ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8ddf3-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>

<span data-ttu-id="8ddf3-127">[![Après : L’entrée de menu s’affiche](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8ddf3-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="8ddf3-128">Après : L’entrée de menu s’affiche ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8ddf3-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8ddf3-129">Next</span><span class="sxs-lookup"><span data-stu-id="8ddf3-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
