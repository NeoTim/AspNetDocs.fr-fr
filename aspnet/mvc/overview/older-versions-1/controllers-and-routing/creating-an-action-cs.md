---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Création d’une action (C) Microsoft Docs
author: rick-anderson
description: Découvrez comment ajouter une nouvelle action à un contrôleur MVC ASP.NET. Renseignez-vous sur les exigences d’une méthode d’action.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c1edfbab41afa58c7cb2c0d306995e9fe491c90
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542272"
---
# <a name="creating-an-action-c"></a><span data-ttu-id="60c79-104">Création d’une action (C#)</span><span class="sxs-lookup"><span data-stu-id="60c79-104">Creating an Action (C#)</span></span>

<span data-ttu-id="60c79-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="60c79-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="60c79-106">Découvrez comment ajouter une nouvelle action à un contrôleur MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="60c79-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="60c79-107">Renseignez-vous sur les exigences d’une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="60c79-107">Learn about the requirements for a method to be an action.</span></span>

<span data-ttu-id="60c79-108">Le but de ce tutoriel est d’expliquer comment vous pouvez créer une nouvelle action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="60c79-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="60c79-109">Vous en apprendrez davantage sur les exigences d’une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="60c79-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="60c79-110">Vous apprenez également à empêcher une méthode d’être exposée comme une action.</span><span class="sxs-lookup"><span data-stu-id="60c79-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="60c79-111">Ajout d’une action à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="60c79-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="60c79-112">Vous ajoutez une nouvelle action à un contrôleur en ajoutant une nouvelle méthode au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="60c79-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="60c79-113">Par exemple, le contrôleur dans la liste 1 contient une action nommée Index() et une action nommée SayHello().</span><span class="sxs-lookup"><span data-stu-id="60c79-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="60c79-114">Les deux méthodes sont exposées comme des actions.</span><span class="sxs-lookup"><span data-stu-id="60c79-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="60c79-115">**Liste 1 - Controllers-HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="60c79-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="60c79-116">Pour être exposée à l’univers comme une action, une méthode doit répondre à certaines exigences :</span><span class="sxs-lookup"><span data-stu-id="60c79-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="60c79-117">La méthode doit être publique.</span><span class="sxs-lookup"><span data-stu-id="60c79-117">The method must be public.</span></span>
- <span data-ttu-id="60c79-118">La méthode ne peut pas être une méthode statique.</span><span class="sxs-lookup"><span data-stu-id="60c79-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="60c79-119">La méthode ne peut pas être une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="60c79-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="60c79-120">La méthode ne peut pas être un constructeur, getter, ou setter.</span><span class="sxs-lookup"><span data-stu-id="60c79-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="60c79-121">La méthode ne peut pas avoir des types génériques ouverts.</span><span class="sxs-lookup"><span data-stu-id="60c79-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="60c79-122">La méthode n’est pas une méthode de la classe de base du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="60c79-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="60c79-123">La méthode ne peut pas contenir des paramètres **ref** ou **out.**</span><span class="sxs-lookup"><span data-stu-id="60c79-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="60c79-124">Notez qu’il n’y a aucune restriction sur le type de retour d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="60c79-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="60c79-125">Une action de contrôleur peut retourner une chaîne, un DateTime, une instance de la classe Random, ou vide.</span><span class="sxs-lookup"><span data-stu-id="60c79-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="60c79-126">Le cadre MVC ASP.NET convertira tout type de retour qui n’est pas un résultat d’action en chaîne et rendra la chaîne au navigateur.</span><span class="sxs-lookup"><span data-stu-id="60c79-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="60c79-127">Lorsque vous ajoutez une méthode qui ne viole pas ces exigences à un contrôleur, la méthode est exposée comme une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="60c79-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="60c79-128">Fais attention ici.</span><span class="sxs-lookup"><span data-stu-id="60c79-128">Be careful here.</span></span> <span data-ttu-id="60c79-129">Une action de contrôleur peut être invoquée par toute personne connectée à Internet.</span><span class="sxs-lookup"><span data-stu-id="60c79-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="60c79-130">Ne créez pas, par exemple, une action de contrôleur DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="60c79-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="60c79-131">Empêcher l’invoquée d’une méthode publique</span><span class="sxs-lookup"><span data-stu-id="60c79-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="60c79-132">Si vous avez besoin de créer une méthode publique dans une classe de contrôleur et que vous ne voulez pas exposer la méthode comme une action de contrôleur, alors vous pouvez empêcher la méthode d’être invoquée en utilisant l’attribut [NonAction].</span><span class="sxs-lookup"><span data-stu-id="60c79-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="60c79-133">Par exemple, le contrôleur de la liste 2 contient une méthode publique nommée CompanySecrets() qui est décorée de l’attribut [NonAction].</span><span class="sxs-lookup"><span data-stu-id="60c79-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="60c79-134">**Liste 2 - Controllers-WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="60c79-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="60c79-135">Si vous tentez d’invoquer l’action du contrôleur CompanySecrets() en tapant /Work/CompanySecrets dans la barre d’adresse de votre navigateur, vous recevrez le message d’erreur dans la figure 1.</span><span class="sxs-lookup"><span data-stu-id="60c79-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>

<span data-ttu-id="60c79-136">[![Invoquer une méthode de non-action](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="60c79-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="60c79-137">**Figure 01**: Invoquer une méthode non-action ([Cliquez pour voir l’image grandeur nature](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="60c79-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="60c79-138">[Suivant précédent](creating-a-controller-cs.md)
> [Next](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="60c79-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
