---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Création d’un contrôle Rating (VB) | Microsoft Docs
author: wenz
description: De nombreux sites Web, à partir de commerce électronique aux sites de Communauté, offrent aux utilisateurs d’articles de taux ou éléments. Cela nécessite généralement des efforts de codage, mais nous avons le...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b229d0155fbab0437c644b41424164e4c655f5e5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049946"
---
<a name="creating-a-rating-control-vb"></a><span data-ttu-id="c8669-104">Création d’un contrôle Rating (VB)</span><span class="sxs-lookup"><span data-stu-id="c8669-104">Creating a Rating Control (VB)</span></span>
====================
<span data-ttu-id="c8669-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c8669-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c8669-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c8669-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span></span>

> <span data-ttu-id="c8669-107">De nombreux sites Web, à partir de commerce électronique aux sites de Communauté, offrent aux utilisateurs d’articles de taux ou éléments.</span><span class="sxs-lookup"><span data-stu-id="c8669-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="c8669-108">Cela nécessite généralement des efforts de codage, mais nous avons les outils de contrôle à notre disposition.</span><span class="sxs-lookup"><span data-stu-id="c8669-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="c8669-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c8669-109">Overview</span></span>

<span data-ttu-id="c8669-110">De nombreux sites Web, à partir de commerce électronique aux sites de Communauté, offrent aux utilisateurs d’articles de taux ou éléments.</span><span class="sxs-lookup"><span data-stu-id="c8669-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="c8669-111">Cela nécessite généralement des efforts de codage, mais nous avons les outils de contrôle à notre disposition.</span><span class="sxs-lookup"><span data-stu-id="c8669-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="c8669-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="c8669-112">Steps</span></span>

<span data-ttu-id="c8669-113">Tout d’abord, vous avez besoin (au moins) deux types d’images : une pour rempli les élément de contrôle d’accès et l’autre pour un élément de contrôle d’accès vide.</span><span class="sxs-lookup"><span data-stu-id="c8669-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="c8669-114">Un élément de contrôle d’accès est généralement une étoile ou un sourire.</span><span class="sxs-lookup"><span data-stu-id="c8669-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="c8669-115">Pour ce scénario, vous trouvez trois fichiers, smiley.png et empty.png et émoticône-done.png dans le cadre des téléchargements de code source pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c8669-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="c8669-116">Ensuite, créez un fichier ASP.NET et commencez par ajouter un `ScriptManager` contrôle :</span><span class="sxs-lookup"><span data-stu-id="c8669-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

<span data-ttu-id="c8669-117">Ensuite, ajoutez le `Rating` contrôle à partir d’ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="c8669-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="c8669-118">Les attributs suivants doivent être définies pour cet exemple :</span><span class="sxs-lookup"><span data-stu-id="c8669-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="c8669-119">`CurrentRating` l’évaluation initiale à utiliser</span><span class="sxs-lookup"><span data-stu-id="c8669-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="c8669-120">`MaxRating` la classification maximale</span><span class="sxs-lookup"><span data-stu-id="c8669-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="c8669-121">`EmptyStarCssClass` la classe CSS à utiliser lorsqu’un élément d’évaluation (étoiles) est vide</span><span class="sxs-lookup"><span data-stu-id="c8669-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="c8669-122">`FilledStarCssClass` la classe CSS à utiliser lors du remplissage un élément d’évaluation (étoiles)</span><span class="sxs-lookup"><span data-stu-id="c8669-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="c8669-123">`StarCssClass` la classe CSS à utiliser pour un stat visible</span><span class="sxs-lookup"><span data-stu-id="c8669-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="c8669-124">`WaitingStarCssClass` la classe CSS à utiliser pendant un nombre d’étoiles est envoyé sur le serveur</span><span class="sxs-lookup"><span data-stu-id="c8669-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="c8669-125">Et Voici le balisage qui crée un contrôle rating avec cinq éléments (smileys) dont aucun n’est remplie initialement :</span><span class="sxs-lookup"><span data-stu-id="c8669-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

<span data-ttu-id="c8669-126">Les trois classes CSS référencés devant maintenant afficher les fichiers image appropriée, qui est facile à réaliser à l’aide de CSS :</span><span class="sxs-lookup"><span data-stu-id="c8669-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

<span data-ttu-id="c8669-127">Assurez-vous que vous fournissez la largeur et la hauteur des trois images, sinon l’affichage peut sembler un peu tordue.</span><span class="sxs-lookup"><span data-stu-id="c8669-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="c8669-128">Enfin, le résultat de l’évaluation doit être affiché à l’utilisateur (ou, au moins enregistré dans une base de données).</span><span class="sxs-lookup"><span data-stu-id="c8669-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="c8669-129">Par conséquent, ajoutez une étiquette pour la sortie d’un message texte et un bouton Envoyer le formulaire de contrôle d’accès au serveur de publication :</span><span class="sxs-lookup"><span data-stu-id="c8669-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

<span data-ttu-id="c8669-130">Dans le code côté serveur, accéder au contrôle d’évaluation par le biais de son `ID` , puis accédez à son `CurrentRating` propriété qui est le nombre d’éléments de contrôle d’accès sélectionné, dans notre exemple, une valeur comprise entre 0 et 5.</span><span class="sxs-lookup"><span data-stu-id="c8669-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

<span data-ttu-id="c8669-131">Enregistrez la page et chargez-le dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="c8669-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="c8669-132">Lorsque vous pointez sur les éléments de contrôle d’accès (initialement vide), un effet de JavaScript se produit : Les modifications de contrôle d’accès.</span><span class="sxs-lookup"><span data-stu-id="c8669-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="c8669-133">Lorsque vous cliquez sur l’ensemble des étoiles, l’évaluation actuelle est conservée.</span><span class="sxs-lookup"><span data-stu-id="c8669-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="c8669-134">Enfin, lorsque vous envoyez le formulaire, le code côté serveur génère le classement sélectionné.</span><span class="sxs-lookup"><span data-stu-id="c8669-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="c8669-135">[![Création d’un système d’évaluation avec un minimum de code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c8669-135">[![Creating a rating system with minimal code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="c8669-136">Création d’un système d’évaluation avec un minimum de code ([cliquez pour afficher l’image en taille réelle](creating-a-rating-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c8669-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c8669-137">Précédent</span><span class="sxs-lookup"><span data-stu-id="c8669-137">Previous</span></span>](creating-a-rating-control-cs.md)