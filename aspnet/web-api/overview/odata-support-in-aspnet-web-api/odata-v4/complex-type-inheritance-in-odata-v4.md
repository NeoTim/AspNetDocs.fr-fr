---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Héritage de type complexe en OData v4 avec ASP.NET’API Web (fr) Microsoft Docs
author: rick-anderson
description: Selon les spécifications OData v4, un type complexe peut hériter d’un autre type complexe. (Un type complexe est un type structuré sans clé.) API Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: f433cf625c7d6ff4922d8c4a9954682fc0f1cc33
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543598"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="5daea-104">Héritage de type complexe en OData v4 avec ASP.NET’API Web</span><span class="sxs-lookup"><span data-stu-id="5daea-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="5daea-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5daea-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="5daea-106">Selon les [spécifications](http://www.odata.org/documentation/odata-version-4-0/)OData v4 , un type complexe peut hériter d’un autre type complexe.</span><span class="sxs-lookup"><span data-stu-id="5daea-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="5daea-107">(Un type *complexe* est un type structuré sans clé.) Web API OData 5.3 prend en charge l’héritage de type complexe.</span><span class="sxs-lookup"><span data-stu-id="5daea-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="5daea-108">Ce sujet montre comment construire un modèle de données d’entité (EDM) avec des types d’héritage complexes.</span><span class="sxs-lookup"><span data-stu-id="5daea-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="5daea-109">Pour le code source complet, voir [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="5daea-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5daea-110">Versions logicielles utilisées dans le tutoriel</span><span class="sxs-lookup"><span data-stu-id="5daea-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5daea-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="5daea-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="5daea-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="5daea-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="5daea-113">Hiérarchie du modèle</span><span class="sxs-lookup"><span data-stu-id="5daea-113">Model Hierarchy</span></span>

<span data-ttu-id="5daea-114">Pour illustrer l’héritage de type complexe, nous utiliserons la hiérarchie de classe suivante.</span><span class="sxs-lookup"><span data-stu-id="5daea-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="5daea-115">`Shape`est un type complexe abstrait.</span><span class="sxs-lookup"><span data-stu-id="5daea-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="5daea-116">`Rectangle`, `Triangle`, `Circle` et sont `Shape`des `RoundRectangle` types complexes dérivés de , et dérive de `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="5daea-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="5daea-117">`Window`est un type d’entité et contient une `Shape` instance.</span><span class="sxs-lookup"><span data-stu-id="5daea-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="5daea-118">Voici les classes CLR qui définissent ces types.</span><span class="sxs-lookup"><span data-stu-id="5daea-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="5daea-119">Construire le modèle EDM</span><span class="sxs-lookup"><span data-stu-id="5daea-119">Build the EDM Model</span></span>

<span data-ttu-id="5daea-120">Pour créer l’EDM, vous pouvez utiliser **ODataConventionModelBuilder**, qui déduit les relations d’héritage des types CLR.</span><span class="sxs-lookup"><span data-stu-id="5daea-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="5daea-121">Vous pouvez également construire l’EDM explicitement, en utilisant **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="5daea-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="5daea-122">Cela prend plus de code, mais vous donne plus de contrôle sur l’EDM.</span><span class="sxs-lookup"><span data-stu-id="5daea-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="5daea-123">Ces deux exemples créent le même schéma EDM.</span><span class="sxs-lookup"><span data-stu-id="5daea-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="5daea-124">Document sur les métadonnées</span><span class="sxs-lookup"><span data-stu-id="5daea-124">Metadata Document</span></span>

<span data-ttu-id="5daea-125">Voici le document de métadonnées OData, montrant l’héritage de type complexe.</span><span class="sxs-lookup"><span data-stu-id="5daea-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="5daea-126">D’après le document sur les métadonnées, vous pouvez voir que :</span><span class="sxs-lookup"><span data-stu-id="5daea-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="5daea-127">Le `Shape` type complexe est abstrait.</span><span class="sxs-lookup"><span data-stu-id="5daea-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="5daea-128">Le `Rectangle` `Triangle`type `Circle` , , et `Shape`complexe ont le type de base .</span><span class="sxs-lookup"><span data-stu-id="5daea-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="5daea-129">Le `RoundRectangle` type a `Rectangle`le type de base .</span><span class="sxs-lookup"><span data-stu-id="5daea-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="5daea-130">Types complexes de casting</span><span class="sxs-lookup"><span data-stu-id="5daea-130">Casting Complex Types</span></span>

<span data-ttu-id="5daea-131">Le casting sur les types complexes est maintenant pris en charge.</span><span class="sxs-lookup"><span data-stu-id="5daea-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="5daea-132">Par exemple, la requête suivante `Shape` jette `Rectangle`un .</span><span class="sxs-lookup"><span data-stu-id="5daea-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="5daea-133">Voici la charge utile de réponse :</span><span class="sxs-lookup"><span data-stu-id="5daea-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
