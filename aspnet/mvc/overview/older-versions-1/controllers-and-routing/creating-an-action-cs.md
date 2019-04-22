---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Création d’une Action (c#) | Microsoft Docs
author: microsoft
description: Découvrez comment ajouter une nouvelle action à un contrôleur ASP.NET MVC. En savoir plus sur la configuration requise pour une méthode devant subir une action.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: c66e066bd3e241e667924dacc114f57151df822a
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59389555"
---
# <a name="creating-an-action-c"></a><span data-ttu-id="6777a-104">Création d’une action (C#)</span><span class="sxs-lookup"><span data-stu-id="6777a-104">Creating an Action (C#)</span></span>

<span data-ttu-id="6777a-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6777a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6777a-106">Découvrez comment ajouter une nouvelle action à un contrôleur ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6777a-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="6777a-107">En savoir plus sur la configuration requise pour une méthode devant subir une action.</span><span class="sxs-lookup"><span data-stu-id="6777a-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="6777a-108">L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer une nouvelle action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6777a-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="6777a-109">Vous en savoir plus sur la configuration requise d’une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="6777a-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="6777a-110">Vous allez également apprendre à une méthode empêcher l’exposition en tant qu’action.</span><span class="sxs-lookup"><span data-stu-id="6777a-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="6777a-111">Ajout d’une Action à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="6777a-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="6777a-112">Vous ajoutez une nouvelle action à un contrôleur d’en ajoutant une nouvelle méthode au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6777a-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="6777a-113">Par exemple, le contrôleur dans le Listing 1 contient une action nommée Index() et une action nommée SayHello().</span><span class="sxs-lookup"><span data-stu-id="6777a-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="6777a-114">Les deux méthodes sont exposées en tant qu’actions.</span><span class="sxs-lookup"><span data-stu-id="6777a-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="6777a-115">**Liste 1 - Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="6777a-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="6777a-116">Pour pouvoir être exposés à l’univers en tant qu’action, une méthode doit remplir certaines conditions :</span><span class="sxs-lookup"><span data-stu-id="6777a-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="6777a-117">La méthode doit être publique.</span><span class="sxs-lookup"><span data-stu-id="6777a-117">The method must be public.</span></span>
- <span data-ttu-id="6777a-118">La méthode ne peut pas être une méthode statique.</span><span class="sxs-lookup"><span data-stu-id="6777a-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="6777a-119">La méthode ne peut pas être une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="6777a-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="6777a-120">La méthode ne peut pas être un constructeur, un accesseur Get ou un accesseur Set.</span><span class="sxs-lookup"><span data-stu-id="6777a-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="6777a-121">La méthode ne peut pas avoir de types génériques ouverts.</span><span class="sxs-lookup"><span data-stu-id="6777a-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="6777a-122">La méthode n’est pas une méthode de la classe de base du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6777a-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="6777a-123">La méthode ne peut pas contenir **ref** ou **out** paramètres.</span><span class="sxs-lookup"><span data-stu-id="6777a-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="6777a-124">Notez qu’il n’y a aucune restriction sur le type de retour d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6777a-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="6777a-125">Une action de contrôleur peut retourner une chaîne, une valeur DateTime, une instance de la classe Random, ou void.</span><span class="sxs-lookup"><span data-stu-id="6777a-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="6777a-126">L’infrastructure ASP.NET MVC convertit tout type de retour n’est pas un résultat d’action dans une chaîne et afficher la chaîne dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="6777a-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="6777a-127">Lorsque vous ajoutez une méthode qui ne viole pas ces exigences à un contrôleur, la méthode est exposée comme une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6777a-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="6777a-128">Soyez prudent ici.</span><span class="sxs-lookup"><span data-stu-id="6777a-128">Be careful here.</span></span> <span data-ttu-id="6777a-129">Une action de contrôleur peut être appelée par toute personne connectée à Internet.</span><span class="sxs-lookup"><span data-stu-id="6777a-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="6777a-130">Ne créez pas, par exemple, une action de contrôleur DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="6777a-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="6777a-131">Empêche l’appelé une méthode publique</span><span class="sxs-lookup"><span data-stu-id="6777a-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="6777a-132">Si vous devez créer une méthode publique dans une classe de contrôleur et que vous ne souhaitez pas exposer la méthode comme une action de contrôleur puis vous pouvez empêcher la méthode invoquée à l’aide de l’attribut [NonAction].</span><span class="sxs-lookup"><span data-stu-id="6777a-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="6777a-133">Par exemple, le contrôleur dans le Listing 2 contient une méthode publique nommée CompanySecrets() est décorée avec l’attribut [NonAction].</span><span class="sxs-lookup"><span data-stu-id="6777a-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="6777a-134">**Listing 2 - Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="6777a-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="6777a-135">Si vous tentez d’appeler l’action du contrôleur CompanySecrets() en tapant /Work/CompanySecrets dans la barre d’adresses de votre navigateur vous allez obtenir le message d’erreur dans la Figure 1.</span><span class="sxs-lookup"><span data-stu-id="6777a-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="6777a-136">[![Appel d’une méthode NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6777a-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="6777a-137">**Figure 01**: Appel d’une méthode NonAction ([cliquez pour afficher l’image en taille réelle](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6777a-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6777a-138">[Précédent](creating-a-controller-cs.md)
> [Suivant](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6777a-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
