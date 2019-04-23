---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: À l’aide du contrôle Slider avec publication automatique (c#) | Microsoft Docs
author: wenz
description: Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris. Il est possible de rendre la comptabilisation de curseur automatique...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: a5b858a05470caa244902afbb404adbb2e4761b7
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59402724"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="3d0cb-104">À l’aide du contrôle Slider avec publication automatique (c#)</span><span class="sxs-lookup"><span data-stu-id="3d0cb-104">Using the Slider Control With Auto-Postback (C#)</span></span>

<span data-ttu-id="3d0cb-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3d0cb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3d0cb-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3d0cb-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="3d0cb-107">Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="3d0cb-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="3d0cb-108">Il est possible d’effectuer l’autopostback curseur une fois sa valeur change.</span><span class="sxs-lookup"><span data-stu-id="3d0cb-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="3d0cb-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="3d0cb-109">Overview</span></span>

<span data-ttu-id="3d0cb-110">Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="3d0cb-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="3d0cb-111">Il est possible d’effectuer l’autopostback curseur une fois sa valeur change.</span><span class="sxs-lookup"><span data-stu-id="3d0cb-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="3d0cb-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="3d0cb-112">Steps</span></span>

<span data-ttu-id="3d0cb-113">Afin de rendre le curseur automatiquement publication (postback) à une modification, les deux zones de texte besoin de l’attribut `AutoPostBack="true"`: La zone de texte qui deviendra le curseur lui-même et la zone de texte qui contient la position du curseur.</span><span class="sxs-lookup"><span data-stu-id="3d0cb-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="3d0cb-114">Voici le balisage requis pour ce faire :</span><span class="sxs-lookup"><span data-stu-id="3d0cb-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="3d0cb-115">Le `SliderExtender` contrôle à partir d’ASP.NET AJAX Control Toolkit affecte les fonctionnalités de curseur à deux zones de texte :</span><span class="sxs-lookup"><span data-stu-id="3d0cb-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="3d0cb-116">Un élément label supplémentaires est utilisé ultérieurement pour informer l’utilisateur d’une publication (postback) :</span><span class="sxs-lookup"><span data-stu-id="3d0cb-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="3d0cb-117">Enfin, le `ScriptManager` contrôle d’ASP.NET AJAX charge le code JavaScript nécessaire pour les outils de contrôle fonctionne :</span><span class="sxs-lookup"><span data-stu-id="3d0cb-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="3d0cb-118">Maintenant le curseur est publication ; sur le côté serveur, cet événement peut être intercepté et traité en :</span><span class="sxs-lookup"><span data-stu-id="3d0cb-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="3d0cb-119">[![En déplaçant le curseur déclenche une publication (postback)](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3d0cb-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="3d0cb-120">En déplaçant le curseur déclenche une publication (postback) ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3d0cb-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="3d0cb-121">[![Par la suite, la date de cette modification est écrite dans l’étiquette](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="3d0cb-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="3d0cb-122">Par la suite, la date de cette modification est écrite dans l’étiquette ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="3d0cb-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3d0cb-123">Next</span><span class="sxs-lookup"><span data-stu-id="3d0cb-123">Next</span></span>](databinding-the-slider-control-cs.md)
