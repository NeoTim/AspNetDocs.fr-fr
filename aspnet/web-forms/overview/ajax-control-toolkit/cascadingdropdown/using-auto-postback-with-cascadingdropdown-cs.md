---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Utilisation de la publication automatique avec CascadingDropDownC#() | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un DropDownList chargent les valeurs associées dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bccd716814e7de544798010cecbc148ec50b5cd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574492"
---
# <a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="32275-103">Utilisation de la publication (postback) automatique avec CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="32275-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>

<span data-ttu-id="32275-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="32275-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="32275-105">[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="32275-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="32275-106">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="32275-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="32275-107">Toutefois, lors de l’utilisation du contrôle CascadingDropDown, ASP. La fonctionnalité AutoPostBack du contrôle DropDownList de NET ne fonctionne pas, car le chargement asynchrone des données dans la liste génère une publication (postback) (inutile) proprement dite.</span><span class="sxs-lookup"><span data-stu-id="32275-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="32275-108">Avec du code JavaScript, cet effet peut être évité.</span><span class="sxs-lookup"><span data-stu-id="32275-108">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="overview"></a><span data-ttu-id="32275-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="32275-109">Overview</span></span>

<span data-ttu-id="32275-110">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="32275-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="32275-111">(Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) Toutefois, lors de l’utilisation du contrôle CascadingDropDown, ASP. La fonctionnalité AutoPostBack du contrôle DropDownList de NET ne fonctionne pas, car le chargement asynchrone des données dans la liste génère une publication (postback) (inutile) proprement dite.</span><span class="sxs-lookup"><span data-stu-id="32275-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="32275-112">Avec du code JavaScript, cet effet peut être évité.</span><span class="sxs-lookup"><span data-stu-id="32275-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="32275-113">Étapes</span><span class="sxs-lookup"><span data-stu-id="32275-113">Steps</span></span>

<span data-ttu-id="32275-114">Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans le &lt;`form`élément &gt;) :</span><span class="sxs-lookup"><span data-stu-id="32275-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="32275-115">Un contrôle DropDownList est ensuite requis :</span><span class="sxs-lookup"><span data-stu-id="32275-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="32275-116">Pour cette liste, un extendeur CascadingDropDown est ajouté, fournissant des informations sur l’URL et la méthode du service Web :</span><span class="sxs-lookup"><span data-stu-id="32275-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="32275-117">L’extendeur CascadingDropDown appelle ensuite de manière asynchrone un service Web avec la signature de méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="32275-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="32275-118">La méthode retourne un tableau de type CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="32275-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="32275-119">Le constructeur du type attend la légende de la première entrée de liste, puis la valeur (attribut de `value` HTML).</span><span class="sxs-lookup"><span data-stu-id="32275-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="32275-120">Le chargement de la page dans le navigateur remplit la liste déroulante avec trois fournisseurs, la seconde étant présélectionnée.</span><span class="sxs-lookup"><span data-stu-id="32275-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="32275-121">En outre, ASP.NET définit la méthode JavaScript `__doPostBack()`.</span><span class="sxs-lookup"><span data-stu-id="32275-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="32275-122">Une fois la page chargée, cet appel JavaScript est ajouté à la liste déroulante, mais uniquement s’il existe des éléments.</span><span class="sxs-lookup"><span data-stu-id="32275-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="32275-123">S’il n’y a aucun élément dans la liste, la boîte à outils de contrôle les charge actuellement, le code JavaScript utilise donc un délai d’attente et tente à nouveau de s’exécuter en une demi-seconde.</span><span class="sxs-lookup"><span data-stu-id="32275-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="32275-124">De cette façon, une publication (postback) est exécutée uniquement lorsqu’il y a réellement des éléments dans la liste et que l’utilisateur sélectionne une entrée.</span><span class="sxs-lookup"><span data-stu-id="32275-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>

<span data-ttu-id="32275-125">[![la sélection d’un élément de liste provoque une publication (postback)](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="32275-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="32275-126">La sélection d’un élément de liste entraîne une publication ([cliquez pour afficher l’image en taille réelle](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="32275-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="32275-127">[Précédent](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Suivant](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="32275-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
