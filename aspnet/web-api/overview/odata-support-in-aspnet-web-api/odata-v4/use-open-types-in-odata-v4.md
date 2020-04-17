---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Types ouverts en OData v4 avec ASP.NET’API Web (fr) Microsoft Docs
author: rick-anderson
description: Dans OData v4, un type ouvert est un type structuré qui contient des propriétés dynamiques, en plus de toutes les propriétés qui sont déclarées dans la définition de type. Ouvrir…
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d63c96df6614896b3b67eef94a8907e528572567
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543728"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="01a87-104">Open Types en OData v4 avec ASP.NET’API Web</span><span class="sxs-lookup"><span data-stu-id="01a87-104">Open Types in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="01a87-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="01a87-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="01a87-106">Dans OData v4, un *type ouvert* est un type structuré qui contient des propriétés dynamiques, en plus de toutes les propriétés qui sont déclarées dans la définition de type.</span><span class="sxs-lookup"><span data-stu-id="01a87-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="01a87-107">Les types ouverts vous permettent d’ajouter de la flexibilité à vos modèles de données.</span><span class="sxs-lookup"><span data-stu-id="01a87-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="01a87-108">Ce tutoriel montre comment utiliser les types ouverts dans ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="01a87-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="01a87-109">Ce tutoriel suppose que vous savez déjà comment créer un point de terminaison OData dans ASP.NET’API Web.</span><span class="sxs-lookup"><span data-stu-id="01a87-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="01a87-110">Si ce n’est pas le cas, commencez par lire [Créer un point de terminaison OData v4 d’abord.](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="01a87-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="01a87-111">Versions logicielles utilisées dans le tutoriel</span><span class="sxs-lookup"><span data-stu-id="01a87-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="01a87-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="01a87-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="01a87-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="01a87-113">OData v4</span></span>

<span data-ttu-id="01a87-114">Tout d’abord, une certaine terminologie OData:</span><span class="sxs-lookup"><span data-stu-id="01a87-114">First, some OData terminology:</span></span>

- <span data-ttu-id="01a87-115">Type d’entité : Type structuré avec une clé.</span><span class="sxs-lookup"><span data-stu-id="01a87-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="01a87-116">Type complexe : Type structuré sans clé.</span><span class="sxs-lookup"><span data-stu-id="01a87-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="01a87-117">Type ouvert : Type avec des propriétés dynamiques.</span><span class="sxs-lookup"><span data-stu-id="01a87-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="01a87-118">Les deux types d’entités et les types complexes peuvent être ouverts.</span><span class="sxs-lookup"><span data-stu-id="01a87-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="01a87-119">La valeur d’une propriété dynamique peut être un type primitif, un type complexe ou un type d’énumération; ou une collection de l’un de ces types.</span><span class="sxs-lookup"><span data-stu-id="01a87-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="01a87-120">Pour plus d’informations sur les types ouverts, voir la [spécification OData v4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="01a87-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="01a87-121">Installer les bibliothèques Web OData</span><span class="sxs-lookup"><span data-stu-id="01a87-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="01a87-122">Utilisez NuGet Package Manager pour installer les dernières bibliothèques Web API OData.</span><span class="sxs-lookup"><span data-stu-id="01a87-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="01a87-123">À partir de la fenêtre Console du Gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="01a87-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="01a87-124">Décrivez les types CLR</span><span class="sxs-lookup"><span data-stu-id="01a87-124">Define the CLR Types</span></span>

<span data-ttu-id="01a87-125">Commencez par définir les modèles EDM comme types CLR.</span><span class="sxs-lookup"><span data-stu-id="01a87-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="01a87-126">Lorsque le modèle de données d’entité (EDM) est créé,</span><span class="sxs-lookup"><span data-stu-id="01a87-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="01a87-127">`Category`est un type d’énumération.</span><span class="sxs-lookup"><span data-stu-id="01a87-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="01a87-128">`Address`est un type complexe.</span><span class="sxs-lookup"><span data-stu-id="01a87-128">`Address` is a complex type.</span></span> <span data-ttu-id="01a87-129">(Il n’a pas de clé, donc ce n’est pas un type d’entité.)</span><span class="sxs-lookup"><span data-stu-id="01a87-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="01a87-130">`Customer`est un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="01a87-130">`Customer` is an entity type.</span></span> <span data-ttu-id="01a87-131">(Il a une clé.)</span><span class="sxs-lookup"><span data-stu-id="01a87-131">(It has a key.)</span></span>
- <span data-ttu-id="01a87-132">`Press`est un type complexe ouvert.</span><span class="sxs-lookup"><span data-stu-id="01a87-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="01a87-133">`Book`est un type d’entité ouverte.</span><span class="sxs-lookup"><span data-stu-id="01a87-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="01a87-134">Pour créer un type ouvert, le type CLR doit avoir une propriété de type, `IDictionary<string, object>`qui détient les propriétés dynamiques.</span><span class="sxs-lookup"><span data-stu-id="01a87-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="01a87-135">Construire le modèle EDM</span><span class="sxs-lookup"><span data-stu-id="01a87-135">Build the EDM Model</span></span>

<span data-ttu-id="01a87-136">Si vous utilisez **ODataConventionModelBuilder** pour créer `Press` `Book` l’EDM, et sont automatiquement `IDictionary<string, object>` ajoutés en tant que types ouverts, en fonction de la présence d’une propriété.</span><span class="sxs-lookup"><span data-stu-id="01a87-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="01a87-137">Vous pouvez également construire l’EDM explicitement, en utilisant **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="01a87-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="01a87-138">Ajouter un contrôleur OData</span><span class="sxs-lookup"><span data-stu-id="01a87-138">Add an OData Controller</span></span>

<span data-ttu-id="01a87-139">Ensuite, ajoutez un contrôleur OData.</span><span class="sxs-lookup"><span data-stu-id="01a87-139">Next, add an OData controller.</span></span> <span data-ttu-id="01a87-140">Pour ce tutoriel, nous allons utiliser un contrôleur simplifié qui prend simplement en charge les demandes GET et POST, et utilise une liste de mémoire pour stocker des entités.</span><span class="sxs-lookup"><span data-stu-id="01a87-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="01a87-141">Notez que `Book` la première instance n’a pas de propriétés dynamiques.</span><span class="sxs-lookup"><span data-stu-id="01a87-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="01a87-142">La `Book` deuxième instance a les propriétés dynamiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="01a87-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="01a87-143">"Publié": Type primitif</span><span class="sxs-lookup"><span data-stu-id="01a87-143">"Published": Primitive type</span></span>
- <span data-ttu-id="01a87-144">"Auteurs": Collection de types primitifs</span><span class="sxs-lookup"><span data-stu-id="01a87-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="01a87-145">"Autrescategories": Collection de types d’énumération.</span><span class="sxs-lookup"><span data-stu-id="01a87-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="01a87-146">En outre, `Press` la `Book` propriété de cette instance a les propriétés dynamiques suivantes:</span><span class="sxs-lookup"><span data-stu-id="01a87-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="01a87-147">"Blog": Type primitif</span><span class="sxs-lookup"><span data-stu-id="01a87-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="01a87-148">"Adresse": Type complexe</span><span class="sxs-lookup"><span data-stu-id="01a87-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="01a87-149">Requête des métadonnées</span><span class="sxs-lookup"><span data-stu-id="01a87-149">Query the Metadata</span></span>

<span data-ttu-id="01a87-150">Pour obtenir le document de métadonnées OData, envoyez une demande GET à `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="01a87-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="01a87-151">L’organisme de réponse devrait ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="01a87-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="01a87-152">D’après le document sur les métadonnées, vous pouvez voir que :</span><span class="sxs-lookup"><span data-stu-id="01a87-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="01a87-153">Pour `Book` les `Press` types et les `OpenType` types, la valeur de l’attribut est vraie.</span><span class="sxs-lookup"><span data-stu-id="01a87-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="01a87-154">Les `Customer` `Address` types et les types n’ont pas cet attribut.</span><span class="sxs-lookup"><span data-stu-id="01a87-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="01a87-155">Le `Book` type d’entité a trois propriétés déclarées : ISBN, Titre et Presse.</span><span class="sxs-lookup"><span data-stu-id="01a87-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="01a87-156">Les métadonnées OData `Book.Properties` n’incluent pas la propriété de la classe CLR.</span><span class="sxs-lookup"><span data-stu-id="01a87-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="01a87-157">De même, le `Press` type complexe n’a que deux propriétés déclarées : nom et catégorie.</span><span class="sxs-lookup"><span data-stu-id="01a87-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="01a87-158">Les métadonnées n’incluent pas la `Press.DynamicProperties` propriété de la classe CLR.</span><span class="sxs-lookup"><span data-stu-id="01a87-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="01a87-159">Requête d’une entité</span><span class="sxs-lookup"><span data-stu-id="01a87-159">Query an Entity</span></span>

<span data-ttu-id="01a87-160">Pour obtenir le livre avec ISBN égal à "978-0-7356-7942-9", envoyer une demande GET à `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="01a87-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="01a87-161">Le corps de réponse doit ressembler à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="01a87-161">The response body should look similar to the following.</span></span> <span data-ttu-id="01a87-162">(Indented pour le rendre plus lisible.)</span><span class="sxs-lookup"><span data-stu-id="01a87-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="01a87-163">Notez que les propriétés dynamiques sont incluses en ligne avec les propriétés déclarées.</span><span class="sxs-lookup"><span data-stu-id="01a87-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="01a87-164">POST une entité</span><span class="sxs-lookup"><span data-stu-id="01a87-164">POST an Entity</span></span>

<span data-ttu-id="01a87-165">Pour ajouter une entité De livre, envoyez une demande POST à `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="01a87-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="01a87-166">Le client peut définir des propriétés dynamiques dans la charge utile de demande.</span><span class="sxs-lookup"><span data-stu-id="01a87-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="01a87-167">Voici un exemple de demande.</span><span class="sxs-lookup"><span data-stu-id="01a87-167">Here is an example request.</span></span> <span data-ttu-id="01a87-168">Notez les propriétés "Prix" et "Publié".</span><span class="sxs-lookup"><span data-stu-id="01a87-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="01a87-169">Si vous définissez un point d’arrêt dans la méthode du `Properties` contrôleur, vous pouvez voir que l’API Web a ajouté ces propriétés au dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="01a87-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="01a87-170">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="01a87-170">Additional Resources</span></span>

[<span data-ttu-id="01a87-171">Échantillon de type ouvert OData</span><span class="sxs-lookup"><span data-stu-id="01a87-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
