---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Remplissage d’une liste avec CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 4cf5637eed1ecf8a09e8a98fa0193b6d162fd92b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65130472"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="21b77-103">Remplissage d’une liste avec CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="21b77-103">Filling a List Using CascadingDropDown (VB)</span></span>

<span data-ttu-id="21b77-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="21b77-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="21b77-105">[Télécharger le Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="21b77-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="21b77-106">Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans un autre objet DropDownList.</span><span class="sxs-lookup"><span data-stu-id="21b77-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="21b77-107">(Par exemple, une liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) La première difficulté à résoudre est réellement remplir une liste déroulante à l’aide de ce contrôle.</span><span class="sxs-lookup"><span data-stu-id="21b77-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="overview"></a><span data-ttu-id="21b77-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="21b77-108">Overview</span></span>

<span data-ttu-id="21b77-109">Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans un autre objet DropDownList.</span><span class="sxs-lookup"><span data-stu-id="21b77-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="21b77-110">(Par exemple, une liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) La première difficulté à résoudre est réellement remplir une liste déroulante à l’aide de ce contrôle.</span><span class="sxs-lookup"><span data-stu-id="21b77-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="21b77-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="21b77-111">Steps</span></span>

<span data-ttu-id="21b77-112">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="21b77-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="21b77-113">Ensuite, un contrôle DropDownList est nécessaire :</span><span class="sxs-lookup"><span data-stu-id="21b77-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="21b77-114">Pour cette liste, un extendeur CascadingDropDown est ajouté.</span><span class="sxs-lookup"><span data-stu-id="21b77-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="21b77-115">Il envoie une demande asynchrone à un service web qui renvoie ensuite une liste d’entrées à afficher dans la liste.</span><span class="sxs-lookup"><span data-stu-id="21b77-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="21b77-116">Pour ce faire, les attributs de CascadingDropDown suivants doivent être définies :</span><span class="sxs-lookup"><span data-stu-id="21b77-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="21b77-117">`ServicePath`: URL d’un service web fournissant les entrées de liste</span><span class="sxs-lookup"><span data-stu-id="21b77-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="21b77-118">`ServiceMethod`: Méthode Web fournir les entrées de liste</span><span class="sxs-lookup"><span data-stu-id="21b77-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="21b77-119">`TargetControlID`: ID de la liste déroulante</span><span class="sxs-lookup"><span data-stu-id="21b77-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="21b77-120">`Category`: Informations de catégorie sont soumises à la méthode web lorsqu’elle est appelée</span><span class="sxs-lookup"><span data-stu-id="21b77-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="21b77-121">`PromptText`: Texte affiché lorsque le chargement asynchrone des données de liste à partir du serveur</span><span class="sxs-lookup"><span data-stu-id="21b77-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="21b77-122">Voici le balisage pour le `CascadingDropDown` élément.</span><span class="sxs-lookup"><span data-stu-id="21b77-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="21b77-123">La seule différence entre c# et VB est le nom du service web associé :</span><span class="sxs-lookup"><span data-stu-id="21b77-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="21b77-124">Le code JavaScript en provenance de la `CascadingDropDown` extendeur appelle une méthode de service web avec la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="21b77-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="21b77-125">Par conséquent, l’aspect important est que la méthode doit retourner un tableau de type `CascadingDropDownNameValue` (défini par ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="21b77-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="21b77-126">Dans le `CascadingDropDownNameValue` constructeur, le premier texte d’entrée de la liste, puis sa valeur doivent être fournies, tout comme `<option value="VALUE">NAME</option>` dans HTML.</span><span class="sxs-lookup"><span data-stu-id="21b77-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="21b77-127">Voici quelques exemples de données :</span><span class="sxs-lookup"><span data-stu-id="21b77-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="21b77-128">Chargement de la page dans le navigateur déclenche la liste à remplir avec les trois fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="21b77-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>

<span data-ttu-id="21b77-129">[![La liste est remplie automatiquement](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="21b77-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="21b77-130">La liste est remplie automatiquement ([cliquez pour afficher l’image en taille réelle](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="21b77-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="21b77-131">[Précédent](using-auto-postback-with-cascadingdropdown-cs.md)
> [Suivant](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="21b77-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
