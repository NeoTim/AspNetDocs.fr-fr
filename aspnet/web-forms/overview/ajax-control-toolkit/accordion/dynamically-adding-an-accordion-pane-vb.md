---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Ajout dynamique d’un volet Accordion (VB) | Microsoft Docs
author: wenz
description: Le contrôle Accordion dans AJAX Control Toolkit fournit plusieurs volets et permet à l’utilisateur afficher un d’eux à la fois. Panneaux sont généralement déclarés w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 27823e6b65dda80391d494af6f8d8da539bc3334
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393208"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="9ab4e-104">Ajout dynamique d’un volet Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="9ab4e-104">Dynamically Adding An Accordion Pane (VB)</span></span>

<span data-ttu-id="9ab4e-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9ab4e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9ab4e-106">[Télécharger le Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9ab4e-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="9ab4e-107">Le contrôle Accordion dans AJAX Control Toolkit fournit plusieurs volets et permet à l’utilisateur afficher un d’eux à la fois.</span><span class="sxs-lookup"><span data-stu-id="9ab4e-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="9ab4e-108">Panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.</span><span class="sxs-lookup"><span data-stu-id="9ab4e-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="9ab4e-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="9ab4e-109">Overview</span></span>

<span data-ttu-id="9ab4e-110">Le contrôle Accordion dans AJAX Control Toolkit fournit plusieurs volets et permet à l’utilisateur afficher un d’eux à la fois.</span><span class="sxs-lookup"><span data-stu-id="9ab4e-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="9ab4e-111">Panneaux sont généralement déclarés dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.</span><span class="sxs-lookup"><span data-stu-id="9ab4e-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="9ab4e-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="9ab4e-112">Steps</span></span>

<span data-ttu-id="9ab4e-113">Le contrôle Accordion expose toutes les propriétés importantes pour le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="9ab4e-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="9ab4e-114">Entre autres choses, le `Panes` propriété accorde l’accès à la collection de volets qui composent la Accordion.</span><span class="sxs-lookup"><span data-stu-id="9ab4e-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="9ab4e-115">Chaque volet, il est de type `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="9ab4e-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="9ab4e-116">Par conséquent, il est facile de créer un volet de ce type :</span><span class="sxs-lookup"><span data-stu-id="9ab4e-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="9ab4e-117">Le `HeaderContainer` propriété du `AccordionPane` fournit l’accès aux contrôles ASP.NET dans la section d’en-tête du volet ; le `ContentContainer` propriété de `AccordionPane` fait de même pour la section de contenu du volet.</span><span class="sxs-lookup"><span data-stu-id="9ab4e-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="9ab4e-118">Cela permet de code ASP.NET ajouter du contenu dans les volets :</span><span class="sxs-lookup"><span data-stu-id="9ab4e-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="9ab4e-119">Enfin, la pane(s) doit être ajouté à la `Panes` collection de l’accordéon :</span><span class="sxs-lookup"><span data-stu-id="9ab4e-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="9ab4e-120">Voici un code côté serveur complet qui ajoute deux volets à un contrôle Accordion :</span><span class="sxs-lookup"><span data-stu-id="9ab4e-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="9ab4e-121">Le seul élément manquant est Accordion lui-même, ce qui dépend de la présence de l’ASP.NET `ScriptManager` contrôle :</span><span class="sxs-lookup"><span data-stu-id="9ab4e-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="9ab4e-122">Pour terminer l’exemple, les deux classes CSS référencés dans le contrôle Accordion fournissent des informations de style pour le navigateur :</span><span class="sxs-lookup"><span data-stu-id="9ab4e-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![T<span data-ttu-id="9ab4e-123">les données dans l’accordéon a été ajouté dynamiquement par le code côté serveur]</span><span class="sxs-lookup"><span data-stu-id="9ab4e-123">he data in the accordion was dynamically added by server-side code]</span></span>(dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

<span data-ttu-id="9ab4e-124">Les données dans l’accordéon a été ajoutées dynamiquement par le code côté serveur ([cliquez pour afficher l’image en taille réelle](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9ab4e-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9ab4e-125">Précédent</span><span class="sxs-lookup"><span data-stu-id="9ab4e-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
