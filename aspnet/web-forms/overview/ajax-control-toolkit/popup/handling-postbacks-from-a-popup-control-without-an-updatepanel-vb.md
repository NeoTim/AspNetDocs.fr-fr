---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Gestion des publications (postback) à partir d’un contrôle Popup sans un UpdatePanel (VB) | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Lorsqu’une publication (postback) se produit en unités de recherche...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c0535ee78bf3ee5133d73749e8376f4b771f4d3c
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65115107"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="25f1b-104">Gestion des publications (postback) à partir d’un contrôle de fenêtre contextuelle sans un UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="25f1b-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>

<span data-ttu-id="25f1b-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="25f1b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="25f1b-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="25f1b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="25f1b-107">L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="25f1b-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="25f1b-108">Lorsqu’une publication (postback) se produit dans un tel panneau et il existe plusieurs panneaux sur la page, il est difficile de déterminer le panneau qui a été cliqué.</span><span class="sxs-lookup"><span data-stu-id="25f1b-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="overview"></a><span data-ttu-id="25f1b-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="25f1b-109">Overview</span></span>

<span data-ttu-id="25f1b-110">L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="25f1b-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="25f1b-111">Lorsqu’une publication (postback) se produit dans un tel panneau et il existe plusieurs panneaux sur la page, il est difficile de déterminer le panneau qui a été cliqué.</span><span class="sxs-lookup"><span data-stu-id="25f1b-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="25f1b-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="25f1b-112">Steps</span></span>

<span data-ttu-id="25f1b-113">Lorsque vous utilisez un `PopupControl` avec une publication (postback), mais sans avoir un `UpdatePanel` dans la page, les outils de contrôle n’offre pas un moyen de déterminer quel élément client a déclenché la fenêtre contextuelle qui à son tour a provoqué la publication (postback).</span><span class="sxs-lookup"><span data-stu-id="25f1b-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="25f1b-114">Toutefois, une petite astuce fournit une solution de contournement pour ce scénario.</span><span class="sxs-lookup"><span data-stu-id="25f1b-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="25f1b-115">Tout d’abord, voici la configuration de base : deux zones de texte qui tous deux de déclenchent la fenêtre contextuelle même, un calendrier.</span><span class="sxs-lookup"><span data-stu-id="25f1b-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="25f1b-116">Deux `PopupControlExtenders` rassembler les zones de texte et la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="25f1b-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="25f1b-117">L’idée de base consiste à ajouter un champ de formulaire masqué dans le &lt; `form` &gt; élément qui contient la zone de texte qui a lancé la fenêtre contextuelle :</span><span class="sxs-lookup"><span data-stu-id="25f1b-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="25f1b-118">Lorsque la page est chargée, le code JavaScript ajoute un gestionnaire d’événements pour les deux zones de texte : Chaque fois que l’utilisateur clique sur une zone de texte, son nom est écrit dans le champ de formulaire masqué :</span><span class="sxs-lookup"><span data-stu-id="25f1b-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="25f1b-119">Dans le code côté serveur, la valeur du champ masqué doit être lues.</span><span class="sxs-lookup"><span data-stu-id="25f1b-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="25f1b-120">Dans la mesure où les champs de formulaire masqués sont faciles à manipuler, une approche de la liste verte pour valider la valeur masquée est requise.</span><span class="sxs-lookup"><span data-stu-id="25f1b-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="25f1b-121">Une fois que la zone de texte correct a été identifiée, la date dans le calendrier est écrit dedans.</span><span class="sxs-lookup"><span data-stu-id="25f1b-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]

<span data-ttu-id="25f1b-122">[![Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="25f1b-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="25f1b-123">Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="25f1b-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>

<span data-ttu-id="25f1b-124">[![En cliquant sur une date de la place dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="25f1b-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="25f1b-125">En cliquant sur une date de la place dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="25f1b-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="25f1b-126">Précédent</span><span class="sxs-lookup"><span data-stu-id="25f1b-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
