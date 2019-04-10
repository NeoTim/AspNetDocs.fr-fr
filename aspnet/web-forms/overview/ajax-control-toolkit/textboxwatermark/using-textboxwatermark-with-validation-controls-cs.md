---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Utilisation de TextBoxWatermark avec des contrôles de Validation (c#) | Microsoft Docs
author: wenz
description: Le contrôle TextBoxWatermark dans AJAX Control Toolkit étend une zone de texte afin qu’un texte est affiché dans la zone. Lorsqu’un utilisateur clique dans la zone, il je...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: f833868d9dbf51a9714b9bbe6730a24badc169d0
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59391011"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="dbe9c-104">Utilisation de TextBoxWatermark avec des contrôles Validation (C#)</span><span class="sxs-lookup"><span data-stu-id="dbe9c-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>

<span data-ttu-id="dbe9c-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dbe9c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dbe9c-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="dbe9c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="dbe9c-107">Le contrôle TextBoxWatermark dans AJAX Control Toolkit étend une zone de texte afin qu’un texte est affiché dans la zone.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="dbe9c-108">Lorsqu’un utilisateur clique dans la zone, elle est vidée.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="dbe9c-109">Si l’utilisateur laisse la zone sans saisie de texte, le texte prérempli réapparaît.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="dbe9c-110">Cela peut entrer en conflit avec des contrôles de Validation ASP.NET sur la même page, mais ces problèmes peuvent être surmontés.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="dbe9c-111">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="dbe9c-111">Overview</span></span>

<span data-ttu-id="dbe9c-112">Le `TextBoxWatermark` contrôle dans AJAX Control Toolkit étend une zone de texte afin qu’un texte est affiché dans la zone.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="dbe9c-113">Lorsqu’un utilisateur clique dans la zone, elle est vidée.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="dbe9c-114">Si l’utilisateur laisse la zone sans saisie de texte, le texte prérempli réapparaît.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="dbe9c-115">Cela peut entrer en conflit avec des contrôles de Validation ASP.NET sur la même page, mais ces problèmes peuvent être surmontés.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="dbe9c-116">Étapes</span><span class="sxs-lookup"><span data-stu-id="dbe9c-116">Steps</span></span>

<span data-ttu-id="dbe9c-117">La configuration de base de l’exemple est le suivant : un `TextBox` contrôle est lui appliquer un filigrane à l’aide un `TextBoxWatermarkExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="dbe9c-118">Un bouton déclenche une publication (postback) et est utilisé ultérieurement pour déclencher les contrôles de validation sur la page.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="dbe9c-119">En outre, un `ScriptManager` contrôle est requis pour initialiser d’ASP.NET AJAX :</span><span class="sxs-lookup"><span data-stu-id="dbe9c-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="dbe9c-120">Ajoutez maintenant un `RequiredFieldValidator` contrôle qui vérifie si le champ n’est texte lorsque le formulaire est envoyé.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="dbe9c-121">Le `InitialValue` propriété du validateur doit être définie sur la même valeur que celle qui est utilisée dans le `TextBoxWatermarkExtender` contrôle : Lorsque le formulaire est envoyé, la valeur d’une zone de texte inchangé est la valeur de filigrane qu’il contient :</span><span class="sxs-lookup"><span data-stu-id="dbe9c-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="dbe9c-122">Toutefois il existe un problème avec cette approche : Si le client désactive JavaScript, le champ de texte n’est pas donc prérempli avec le texte de filigrane, les `RequiredFieldValidator` ne déclenche pas d’un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="dbe9c-123">Par conséquent, une seconde `RequiredFieldValidator` contrôle est requise qui vérifie une zone de texte vide (en omettant la `InitialValue` attribut).</span><span class="sxs-lookup"><span data-stu-id="dbe9c-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="dbe9c-124">Étant donné que les deux validateurs utilisent `Display` = `"Dynamic"`, l’utilisateur final ne peut pas distinguer à partir de l’apparence visuelle parmi les deux validateurs a été déclenché ; au lieu de cela, il semblerait qu’un seul d'entre eux.</span><span class="sxs-lookup"><span data-stu-id="dbe9c-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="dbe9c-125">Enfin, ajoutez du code côté serveur pour le texte dans le champ de sortie si aucun validateur n’émis un message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="dbe9c-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![T<span data-ttu-id="dbe9c-126">Validateur de he se plaint que le champ n’est pas de texte]</span><span class="sxs-lookup"><span data-stu-id="dbe9c-126">he validator complains that there is no text in the field]</span></span>(using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

<span data-ttu-id="dbe9c-127">Le programme de validation se plaint qu’il n’existe aucun texte dans le champ ([cliquez pour afficher l’image en taille réelle](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dbe9c-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dbe9c-128">[Précédent](using-textboxwatermark-in-a-formview-cs.md)
> [Suivant](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="dbe9c-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
