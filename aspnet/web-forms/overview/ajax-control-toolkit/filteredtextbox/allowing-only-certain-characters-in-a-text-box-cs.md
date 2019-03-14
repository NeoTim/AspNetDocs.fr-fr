---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Autoriser seulement certains caractères dans une zone de texte (c#) | Microsoft Docs
author: wenz
description: Contrôles de validation ASP.NET peuvent garantir que seulement certains caractères sont autorisés dans l’entrée d’utilisateur. Toutefois cela toujours n’empêche pas les utilisateurs de taper non valides...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d8a1e792c9cd854591fc434f28afe98e4d91dfbe
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031326"
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="05d3b-104">Autoriser seulement certains caractères dans une zone de texte (C#)</span><span class="sxs-lookup"><span data-stu-id="05d3b-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="05d3b-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="05d3b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="05d3b-106">[Télécharger le Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="05d3b-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="05d3b-107">Contrôles de validation ASP.NET peuvent garantir que seulement certains caractères sont autorisés dans l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="05d3b-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="05d3b-108">Toutefois cette toujours n’empêche pas les utilisateurs de taper des caractères non valides et essayez d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="05d3b-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="05d3b-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="05d3b-109">Overview</span></span>

<span data-ttu-id="05d3b-110">Contrôles de validation ASP.NET peuvent garantir que seulement certains caractères sont autorisés dans l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="05d3b-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="05d3b-111">Toutefois cette toujours n’empêche pas les utilisateurs de taper des caractères non valides et essayez d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="05d3b-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="05d3b-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="05d3b-112">Steps</span></span>

<span data-ttu-id="05d3b-113">ASP.NET AJAX Control Toolkit contient le `FilteredTextBox` contrôle qui étend une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="05d3b-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="05d3b-114">Une fois activé, uniquement un certain ensemble de caractères peut-être être entré dans le champ.</span><span class="sxs-lookup"><span data-stu-id="05d3b-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="05d3b-115">Pour ce faire, nous devons abord comme d’habitude ASP.NET AJAX `ScriptManager` qui charge les bibliothèques JavaScript qui sont également utilisés par ASP.NET AJAX Control Toolkit :</span><span class="sxs-lookup"><span data-stu-id="05d3b-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="05d3b-116">Ensuite, nous avons besoin d’une zone de texte :</span><span class="sxs-lookup"><span data-stu-id="05d3b-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="05d3b-117">Enfin, le `FilteredTextBoxExtender` contrôle s’occupe de limiter les caractères que l’utilisateur est autorisé à taper.</span><span class="sxs-lookup"><span data-stu-id="05d3b-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="05d3b-118">Tout d’abord, définissez le `TargetControlID` attribut le `ID` de la `TextBox` contrôle.</span><span class="sxs-lookup"><span data-stu-id="05d3b-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="05d3b-119">Ensuite, choisissez une des `FilterType` valeurs :</span><span class="sxs-lookup"><span data-stu-id="05d3b-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="05d3b-120">`Custom` par défaut ; Vous devez fournir une liste de caractères valides</span><span class="sxs-lookup"><span data-stu-id="05d3b-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="05d3b-121">`LowercaseLetters` uniquement des lettres minuscules</span><span class="sxs-lookup"><span data-stu-id="05d3b-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="05d3b-122">`Numbers` chiffres uniquement</span><span class="sxs-lookup"><span data-stu-id="05d3b-122">`Numbers` digits only</span></span>
- <span data-ttu-id="05d3b-123">`UppercaseLetters` uniquement des lettres majuscules</span><span class="sxs-lookup"><span data-stu-id="05d3b-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="05d3b-124">Si le `Custom FilterType` est utilisé, le `ValidChars` propriété doit être défini et fournir une liste de caractères qui peuvent être tapés.</span><span class="sxs-lookup"><span data-stu-id="05d3b-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="05d3b-125">À propos : Si vous essayez de coller du texte dans la zone de texte, tous les caractères non valides sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="05d3b-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="05d3b-126">Voici le balisage pour le `FilteredTextBoxExtender` contrôle qui autorise uniquement des chiffres (quelque chose qui aurait également été possible avec `FilterType="Numbers"`) :</span><span class="sxs-lookup"><span data-stu-id="05d3b-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="05d3b-127">Exécutez la page, puis réessayez d’entrer une lettre si JavaScript est activé, il ne fonctionnera pas ; Toutefois, les chiffres apparaissent dans la page.</span><span class="sxs-lookup"><span data-stu-id="05d3b-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="05d3b-128">Toutefois Notez que la protection `FilteredTextBox` fournit n’est pas à toute épreuve : Si JavaScript est activé, toutes les données peuvent être entrées dans la zone de texte, donc vous devez utiliser une validation supplémentaire signifie que, par exemple, ASP. Contrôles de validation du NET.</span><span class="sxs-lookup"><span data-stu-id="05d3b-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="05d3b-129">[![Seuls les chiffres peuvent être entrés.](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="05d3b-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="05d3b-130">Seuls les chiffres peuvent être entrés ([cliquez pour afficher l’image en taille réelle](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="05d3b-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="05d3b-131">Next</span><span class="sxs-lookup"><span data-stu-id="05d3b-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
