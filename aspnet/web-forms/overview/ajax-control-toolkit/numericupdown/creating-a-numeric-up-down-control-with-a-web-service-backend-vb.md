---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Création d’un numérique haut/bas contrôle avec un Backend de Service Web (VB) | Microsoft Docs
author: wenz
description: Au lieu de laisser un utilisateur de taper une valeur dans une case à cocher, contrôle (qui existe sur Windows et d’autres systèmes d’exploitation) haut/bas numérique peut s’avérer plus que c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 0442b5e22e44e0767825026b26ad3da55777b962
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59384264"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="7eae5-103">Création d’un contrôle Haut/bas numérique avec un backend de service web (VB)</span><span class="sxs-lookup"><span data-stu-id="7eae5-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>

<span data-ttu-id="7eae5-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7eae5-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7eae5-105">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7eae5-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="7eae5-106">Au lieu de laisser un utilisateur de taper une valeur dans une case à cocher, une valeur numérique haut/bas contrôle (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus à l’aise.</span><span class="sxs-lookup"><span data-stu-id="7eae5-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="7eae5-107">Par défaut, le contrôle NumericUpDown toujours augmente ou diminue d’une valeur de 1, mais un service web s’avère plus de souplesse.</span><span class="sxs-lookup"><span data-stu-id="7eae5-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="7eae5-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="7eae5-108">Overview</span></span>

<span data-ttu-id="7eae5-109">Au lieu de laisser un utilisateur de taper une valeur dans une case à cocher, une valeur numérique haut/bas contrôle (qui existe sur Windows et d’autres systèmes d’exploitation) peut s’avérer plus à l’aise.</span><span class="sxs-lookup"><span data-stu-id="7eae5-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="7eae5-110">Par défaut, le `NumericUpDown` contrôle toujours augmente ou diminue d’une valeur de 1, mais un service web s’avère plus de souplesse.</span><span class="sxs-lookup"><span data-stu-id="7eae5-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="7eae5-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="7eae5-111">Steps</span></span>

<span data-ttu-id="7eae5-112">ASP.NET AJAX Control Toolkit contient le `NumericUpDown` extendeur qui ajoute automatiquement les deux boutons à une zone de texte : Une pour augmenter sa valeur, un pour réduire ce.</span><span class="sxs-lookup"><span data-stu-id="7eae5-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="7eae5-113">Toutefois le contrôle prend également en charge un appel de service web (ou l’appel de méthode de page).</span><span class="sxs-lookup"><span data-stu-id="7eae5-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="7eae5-114">Chaque fois que le haut ou vers le bas du bouton est activé, le code JavaScript de code se connecte au serveur web et exécute une méthode il.</span><span class="sxs-lookup"><span data-stu-id="7eae5-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="7eae5-115">La signature de méthode est celle qui suit :</span><span class="sxs-lookup"><span data-stu-id="7eae5-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="7eae5-116">Le `current` argument est la valeur actuelle dans la zone de texte ; le `tag` attribut est des données de contexte supplémentaire qui peuvent être définies en tant que propriété de la `NumericUpDown` extendeur (mais n’est pas obligatoire).</span><span class="sxs-lookup"><span data-stu-id="7eae5-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="7eae5-117">Pour cet exemple, le contrôle haut/bas numérique doit autoriser uniquement les valeurs qui sont des puissances de deux : 1, 2, 4, 8, 16, 32, 64 et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="7eae5-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="7eae5-118">Par conséquent, la méthode exécutée lorsque l’utilisateur souhaite augmenter la valeur doit double l’ancienne valeur ; l’autre méthode doit diviser la valeur par deux.</span><span class="sxs-lookup"><span data-stu-id="7eae5-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="7eae5-119">Voici donc le service web complète :</span><span class="sxs-lookup"><span data-stu-id="7eae5-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="7eae5-120">Enfin, créez une nouvelle page ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7eae5-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="7eae5-121">Comme d’habitude, vous devez un `ScriptManager` contrôle, un `TextBox` contrôle et un `NumericUpDownExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="7eae5-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="7eae5-122">Pour ce dernier, vous devez fournir les informations de service web :</span><span class="sxs-lookup"><span data-stu-id="7eae5-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="7eae5-123">`ServiceDownMethod` nom de la bas méthode web ou page (méthode)</span><span class="sxs-lookup"><span data-stu-id="7eae5-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="7eae5-124">`ServiceDownPath` chemin d’accès au service web avec la méthode de service vers le bas ; omettre si vous utilisez une méthode de page</span><span class="sxs-lookup"><span data-stu-id="7eae5-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="7eae5-125">`ServiceUpMethod` partie du nom de méthode web ou de page (méthode)</span><span class="sxs-lookup"><span data-stu-id="7eae5-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="7eae5-126">`ServiceUpPath` chemin d’accès au service web avec la méthode de service opérationnel ; omettre si vous utilisez une méthode de page</span><span class="sxs-lookup"><span data-stu-id="7eae5-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="7eae5-127">Voici le balisage complet pour la page :</span><span class="sxs-lookup"><span data-stu-id="7eae5-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="7eae5-128">Si vous exécutez la page, notez la façon dont la valeur dans la zone de texte double toujours lorsque vous cliquez sur le bouton du haut, êtes réduit de moitié lorsque vous cliquez sur le bouton du bas.</span><span class="sxs-lookup"><span data-stu-id="7eae5-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="7eae5-129">[![Seuls des chiffres qui sont une puissance de 2 apparaissent](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7eae5-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="7eae5-130">Seuls des chiffres qui sont une puissance de 2 apparaissent ([cliquez pour afficher l’image en taille réelle](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7eae5-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7eae5-131">Précédent</span><span class="sxs-lookup"><span data-stu-id="7eae5-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
