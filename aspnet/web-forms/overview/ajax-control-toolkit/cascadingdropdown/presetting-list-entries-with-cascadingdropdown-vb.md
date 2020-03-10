---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Prédéfinition des entrées de liste avec CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un DropDownList chargent les valeurs associées dans anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 58d675993777f9dcbe0ce1890a60046c91ee8907
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597929"
---
# <a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="822be-103">Prédéfinition des entrées de liste avec CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="822be-103">Presetting List Entries with CascadingDropDown (VB)</span></span>

<span data-ttu-id="822be-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="822be-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="822be-105">[Télécharger le code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="822be-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="822be-106">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="822be-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="822be-107">Avec un peu de code, il est possible qu’un élément de liste soit présélectionné une fois que les données ont été chargées dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="822be-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="overview"></a><span data-ttu-id="822be-108">Présentation</span><span class="sxs-lookup"><span data-stu-id="822be-108">Overview</span></span>

<span data-ttu-id="822be-109">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à un contrôle DropDownList chargent les valeurs associées dans un autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="822be-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="822be-110">(Par exemple, une liste fournit une liste des États-Unis et la liste suivante est ensuite remplie avec les villes principales dans cet État.) Avec un peu de code, il est possible qu’un élément de liste soit présélectionné une fois que les données ont été chargées dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="822be-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="822be-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="822be-111">Steps</span></span>

<span data-ttu-id="822be-112">Pour activer les fonctionnalités de ASP.NET AJAX et de Control Toolkit, le contrôle de `ScriptManager` doit être placé n’importe où sur la page (mais dans l’élément `<form>`) :</span><span class="sxs-lookup"><span data-stu-id="822be-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="822be-113">Un contrôle DropDownList est ensuite requis :</span><span class="sxs-lookup"><span data-stu-id="822be-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="822be-114">Pour cette liste, un extendeur CascadingDropDown est ajouté, fournissant des informations sur l’URL et la méthode du service Web :</span><span class="sxs-lookup"><span data-stu-id="822be-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="822be-115">L’extendeur CascadingDropDown appelle ensuite de manière asynchrone un service Web avec la signature de méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="822be-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="822be-116">La méthode retourne un tableau de type CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="822be-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="822be-117">Le constructeur du type attend la légende de la première entrée de liste, puis la valeur (attribut de `value` HTML).</span><span class="sxs-lookup"><span data-stu-id="822be-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="822be-118">Si le troisième argument est défini sur true, l’élément de liste est automatiquement sélectionné dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="822be-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="822be-119">Le chargement de la page dans le navigateur remplit la liste déroulante avec trois fournisseurs, la seconde étant présélectionnée.</span><span class="sxs-lookup"><span data-stu-id="822be-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>

<span data-ttu-id="822be-120">[![la liste est remplie et présélectionnée automatiquement](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="822be-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="822be-121">La liste est remplie et présélectionnée automatiquement ([cliquez pour afficher l’image en taille réelle](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="822be-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="822be-122">[Précédent](using-cascadingdropdown-with-a-database-vb.md)
> [Suivant](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="822be-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
