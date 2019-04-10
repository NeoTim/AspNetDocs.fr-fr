---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Gestion des publications (postback) à partir d’un ModalPopup (VB) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Une attention particulière doit entreprendre lorsqu’un pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: add305855d876b5033bbd7921ad24b5e840b9acc
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59386396"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="be295-104">Gestion des publications (postback) à partir d’un ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="be295-104">Handling Postbacks from a ModalPopup (VB)</span></span>

<span data-ttu-id="be295-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="be295-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="be295-106">[Télécharger le Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="be295-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="be295-107">Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="be295-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="be295-108">Une attention particulière doit prendre lorsqu’une publication (postback) est créé à partir de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="be295-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="be295-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="be295-109">Overview</span></span>

<span data-ttu-id="be295-110">Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="be295-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="be295-111">Une attention particulière doit prendre lorsqu’une publication (postback) est créé à partir de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="be295-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="be295-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="be295-112">Steps</span></span>

<span data-ttu-id="be295-113">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="be295-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="be295-114">Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale.</span><span class="sxs-lookup"><span data-stu-id="be295-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="be295-115">Là, l’utilisateur peut entrer un nom et une adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="be295-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="be295-116">Un bouton est utilisé pour fermer la fenêtre contextuelle et enregistrer les informations.</span><span class="sxs-lookup"><span data-stu-id="be295-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="be295-117">Notez que le `OnClick` attribut a la valeur afin qu’une publication (postback) se produit lorsqu’un clic sur ce bouton :</span><span class="sxs-lookup"><span data-stu-id="be295-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="be295-118">La page elle-même se compose de deux étiquettes exactement les mêmes informations : nom et adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="be295-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="be295-119">Un bouton est utilisé pour déclencher la fenêtre contextuelle modale :</span><span class="sxs-lookup"><span data-stu-id="be295-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="be295-120">Pour que la fenêtre contextuelle s’affiche, ajoutez le `ModalPopupExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="be295-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="be295-121">Définir le `PopupControlID` ID du panneau par d’attribut et `TargetControlID` à l’ID du bouton :</span><span class="sxs-lookup"><span data-stu-id="be295-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="be295-122">Maintenant chaque fois que le `Save` dans la fenêtre contextuelle modale avec le bouton est cliqué, côté serveur `SaveData()` méthode est exécutée.</span><span class="sxs-lookup"><span data-stu-id="be295-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="be295-123">Là, vous pouvez enregistrer les données entrées dans un magasin de données.</span><span class="sxs-lookup"><span data-stu-id="be295-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="be295-124">Par souci de simplicité, les nouvelles données sont tout simplement sorties dans l’étiquette :</span><span class="sxs-lookup"><span data-stu-id="be295-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="be295-125">En outre, les contrôles de zone de texte dans la fenêtre contextuelle modale doivent être remplis avec le nom et le courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="be295-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="be295-126">Toutefois cela est nécessaire uniquement lorsque aucune publication (postback) se produit.</span><span class="sxs-lookup"><span data-stu-id="be295-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="be295-127">En l’absence d’une publication (postback), la fonctionnalité de viewstate ASP.NET remplira automatiquement les zones de texte avec les valeurs appropriées.</span><span class="sxs-lookup"><span data-stu-id="be295-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![T<span data-ttu-id="be295-128">fenêtre contextuelle modale he provoque une publication (postback)]</span><span class="sxs-lookup"><span data-stu-id="be295-128">he modal popup causes a postback]</span></span>(handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

<span data-ttu-id="be295-129">La fenêtre contextuelle modale entraîne une publication ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="be295-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="be295-130">[Précédent](using-modalpopup-with-a-repeater-control-vb.md)
> [Suivant](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="be295-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
