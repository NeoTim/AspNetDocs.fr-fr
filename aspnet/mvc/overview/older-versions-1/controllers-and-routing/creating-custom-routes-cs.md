---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Création d’itinéraires personnalisés (C) Microsoft Docs
author: rick-anderson
description: Découvrez comment ajouter des itinéraires personnalisés à une application MVC ASP.NET. Dans ce tutoriel, vous apprenez à modifier le tableau d’itinéraire par défaut dans le fichier Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: b66ccc5e0cd4f6d7e5884394c2b7555b76382d3d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542753"
---
# <a name="creating-custom-routes-c"></a><span data-ttu-id="27637-104">Création de routes personnalisées (C#)</span><span class="sxs-lookup"><span data-stu-id="27637-104">Creating Custom Routes (C#)</span></span>

<span data-ttu-id="27637-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="27637-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="27637-106">Découvrez comment ajouter des itinéraires personnalisés à une application MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="27637-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="27637-107">Dans ce tutoriel, vous apprenez à modifier le tableau d’itinéraire par défaut dans le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="27637-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>

<span data-ttu-id="27637-108">Dans ce tutoriel, vous apprenez à ajouter un itinéraire personnalisé à une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="27637-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="27637-109">Vous apprenez à modifier le tableau d’itinéraire par défaut dans le fichier Global.asax avec un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="27637-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="27637-110">Pour de nombreuses applications MVC simples ASP.NET, le tableau d’itinéraire par défaut fonctionnera très bien.</span><span class="sxs-lookup"><span data-stu-id="27637-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="27637-111">Cependant, vous pourriez découvrir que vous avez des besoins spécialisés de routage.</span><span class="sxs-lookup"><span data-stu-id="27637-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="27637-112">Dans ce cas, vous pouvez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="27637-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="27637-113">Imaginez, par exemple, que vous construisez une application de blog.</span><span class="sxs-lookup"><span data-stu-id="27637-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="27637-114">Vous voudrez peut-être traiter les demandes entrantes qui ressemblent à ceci:</span><span class="sxs-lookup"><span data-stu-id="27637-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="27637-115">/Archives/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="27637-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="27637-116">Lorsqu’un utilisateur saisît cette demande, vous souhaitez retourner l’entrée du blog qui correspond à la date 25/12/2009.</span><span class="sxs-lookup"><span data-stu-id="27637-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="27637-117">Afin de gérer ce type de demande, vous devez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="27637-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="27637-118">Le fichier Global.asax dans La liste 1 contient un nouvel itinéraire personnalisé, nommé Blog, qui gère les demandes qui ressemblent à /Archive /*date d’entrée*.</span><span class="sxs-lookup"><span data-stu-id="27637-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="27637-119">**Liste 1 - Global.asax (avec itinéraire personnalisé)**</span><span class="sxs-lookup"><span data-stu-id="27637-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="27637-120">L’ordre des itinéraires que vous ajoutez à la table d’itinéraire est important.</span><span class="sxs-lookup"><span data-stu-id="27637-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="27637-121">Notre nouvel itinéraire Blog personnalisé est ajouté avant l’itinéraire par défaut existant.</span><span class="sxs-lookup"><span data-stu-id="27637-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="27637-122">Si vous avez inversé l’ordre, alors l’itinéraire par défaut sera toujours appelé au lieu de l’itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="27637-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="27637-123">L’itinéraire Blog personnalisé correspond à toute demande qui commence par /Archive/.</span><span class="sxs-lookup"><span data-stu-id="27637-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="27637-124">Ainsi, il correspond à toutes les URL suivantes:</span><span class="sxs-lookup"><span data-stu-id="27637-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="27637-125">/Archives/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="27637-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="27637-126">/Archives/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="27637-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="27637-127">/Archives/pomme</span><span class="sxs-lookup"><span data-stu-id="27637-127">/Archive/apple</span></span>

<span data-ttu-id="27637-128">L’itinéraire personnalisé cartographie la demande entrante à un contrôleur nommé Archive et invoque l’action d’entrée.).</span><span class="sxs-lookup"><span data-stu-id="27637-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="27637-129">Lorsque la méthode d’entrée() est appelée, la date d’entrée est passée comme paramètre nommé entryDate.</span><span class="sxs-lookup"><span data-stu-id="27637-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="27637-130">Vous pouvez utiliser l’itinéraire personnalisé Blog avec le contrôleur dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="27637-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="27637-131">**Liste 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="27637-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="27637-132">Notez que la méthode d’entrée () dans la liste 2 accepte un paramètre de type DateTime.</span><span class="sxs-lookup"><span data-stu-id="27637-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="27637-133">Le cadre MVC est assez intelligent pour convertir automatiquement la date d’entrée de l’URL en une valeur DateTime.</span><span class="sxs-lookup"><span data-stu-id="27637-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="27637-134">Si le paramètre de date d’entrée de l’URL ne peut pas être converti en date d’heure, une erreur est soulevée (voir la figure 1).</span><span class="sxs-lookup"><span data-stu-id="27637-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="27637-135">**Figure 1 - Erreur de conversion du paramètre**</span><span class="sxs-lookup"><span data-stu-id="27637-135">**Figure 1 - Error from converting parameter**</span></span>

<span data-ttu-id="27637-136">[![Boîte de dialogue New Project](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="27637-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="27637-137">**Figure 01**: Erreur de conversion du paramètre ([Cliquez pour voir l’image grandeur nature](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="27637-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>

## <a name="summary"></a><span data-ttu-id="27637-138">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="27637-138">Summary</span></span>

<span data-ttu-id="27637-139">Le but de ce tutoriel était de démontrer comment vous pouvez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="27637-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="27637-140">Vous avez appris à ajouter un itinéraire personnalisé à la table d’itinéraire dans le fichier Global.asax qui représente les entrées de blog.</span><span class="sxs-lookup"><span data-stu-id="27637-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="27637-141">Nous avons discuté de la façon de cartographier les demandes d’entrées de blog à un contrôleur nommé ArchiveController et une action de contrôleur nommé Entrée().</span><span class="sxs-lookup"><span data-stu-id="27637-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="27637-142">[Suivant précédent](aspnet-mvc-controllers-overview-cs.md)
> [Next](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="27637-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
