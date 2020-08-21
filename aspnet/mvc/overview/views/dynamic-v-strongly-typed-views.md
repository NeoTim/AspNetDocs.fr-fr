---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Vues fortement typées Vues fortement typées | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 30b84c71c86e455f15a659abf566750f1c6eea90
ms.sourcegitcommit: feb88edfb01b32f6fc9488f0f0ddb3c5b34e6ff0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88702944"
---
# <a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="e6da9-103">Vues fortement typées</span><span class="sxs-lookup"><span data-stu-id="e6da9-103">Dynamic v.</span></span> <span data-ttu-id="e6da9-104">et vues dynamiques</span><span class="sxs-lookup"><span data-stu-id="e6da9-104">Strongly Typed Views</span></span>

<span data-ttu-id="e6da9-105">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e6da9-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e6da9-106">Il existe trois façons de passer des informations d’un contrôleur à une vue dans ASP.NET MVC 3 :</span><span class="sxs-lookup"><span data-stu-id="e6da9-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="e6da9-107">En tant qu’objet de modèle fortement typé.</span><span class="sxs-lookup"><span data-stu-id="e6da9-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="e6da9-108">En tant que type dynamique (à l’aide de @model Dynamic)</span><span class="sxs-lookup"><span data-stu-id="e6da9-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="e6da9-109">Utilisation de ViewBag</span><span class="sxs-lookup"><span data-stu-id="e6da9-109">Using the ViewBag</span></span>

<span data-ttu-id="e6da9-110">J’ai écrit une simple application de blog MVC 3 Top pour comparer et opposer les vues dynamiques et fortement typées.</span><span class="sxs-lookup"><span data-stu-id="e6da9-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="e6da9-111">Le contrôleur commence par une simple liste de blogs :</span><span class="sxs-lookup"><span data-stu-id="e6da9-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="e6da9-112">Cliquez avec le bouton droit dans la méthode IndexNotStonglyTyped () et ajoutez une vue Razor.</span><span class="sxs-lookup"><span data-stu-id="e6da9-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="e6da9-113">[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e6da9-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="e6da9-114">Assurez-vous que la case **créer un affichage fortement typé** n’est pas cochée.</span><span class="sxs-lookup"><span data-stu-id="e6da9-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="e6da9-115">La vue résultante ne contient pas la plupart des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e6da9-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="e6da9-116">Dans la mesure où nous utilisons une vue dynamique et non typée fortement typée, IntelliSense ne nous aide pas.</span><span class="sxs-lookup"><span data-stu-id="e6da9-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="e6da9-117">Le code complet est indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="e6da9-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="e6da9-118">[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e6da9-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="e6da9-119">Nous allons maintenant ajouter une vue fortement typée.</span><span class="sxs-lookup"><span data-stu-id="e6da9-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="e6da9-120">Ajoutez le code suivant au contrôleur :</span><span class="sxs-lookup"><span data-stu-id="e6da9-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

<span data-ttu-id="e6da9-121">Notez qu’il s’agit exactement de la même vue de retour (les blogs). Appelez comme vue non fortement typée.</span><span class="sxs-lookup"><span data-stu-id="e6da9-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="e6da9-122">Cliquez avec le bouton droit à l’intérieur de *StonglyTypedIndex ()* et sélectionnez **Ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="e6da9-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="e6da9-123">Cette fois-ci, sélectionnez la classe de modèle de **blog** et sélectionnez **liste** comme modèle de structure.</span><span class="sxs-lookup"><span data-stu-id="e6da9-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="e6da9-124">[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="e6da9-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="e6da9-125">Dans le nouveau modèle de vue, nous obtenons la prise en charge IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e6da9-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="e6da9-126">[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e6da9-126">[![7002.IntelliSense[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>
