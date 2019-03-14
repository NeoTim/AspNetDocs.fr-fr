---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relations d’entité dans OData v4 à l’aide d’ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: 'La plupart des jeux de données définissent les relations entre des entités : Les clients ont passé des commandes ; la documentation ont auteurs ; produits ont des fournisseurs. L’utilisation d’OData, les clients peuvent les parcourir...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d07ddab83462ee1bc84ba8ab15fe906937f506e6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054616"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="0d9f2-104">Relations d’entité dans OData v4 à l’aide d’ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="0d9f2-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="0d9f2-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0d9f2-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="0d9f2-106">La plupart des jeux de données définissent les relations entre des entités : Les clients ont passé des commandes ; la documentation ont auteurs ; produits ont des fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="0d9f2-107">L’utilisation d’OData, les clients peuvent naviguer sur les relations d’entité.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="0d9f2-108">Étant donné un produit, vous pouvez trouver le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="0d9f2-109">Vous pouvez également créer ou supprimer des relations.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-109">You can also create or remove relationships.</span></span> <span data-ttu-id="0d9f2-110">Par exemple, vous pouvez définir le fournisseur pour un produit.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-110">For example, you can set the supplier for a product.</span></span>
>
> <span data-ttu-id="0d9f2-111">Ce didacticiel montre comment prendre en charge ces opérations dans OData v4 à l’aide d’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="0d9f2-112">Le didacticiel s’appuie sur le didacticiel [créer une Using ASP.NET Web API 2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="0d9f2-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0d9f2-113">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="0d9f2-113">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="0d9f2-114">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="0d9f2-114">Web API 2.1</span></span>
> - <span data-ttu-id="0d9f2-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="0d9f2-115">OData v4</span></span>
> - <span data-ttu-id="0d9f2-116">Visual Studio 2013 (Téléchargez Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="0d9f2-116">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="0d9f2-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="0d9f2-117">Entity Framework 6</span></span>
> - <span data-ttu-id="0d9f2-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0d9f2-118">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="0d9f2-119">Versions de didacticiels</span><span class="sxs-lookup"><span data-stu-id="0d9f2-119">Tutorial versions</span></span>
>
> <span data-ttu-id="0d9f2-120">Pour la Version 3 d’OData, consultez [prenant en charge les Relations d’entité dans OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="0d9f2-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="0d9f2-121">Ajouter une entité de fournisseur</span><span class="sxs-lookup"><span data-stu-id="0d9f2-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="0d9f2-122">Le didacticiel s’appuie sur le didacticiel [créer une Using ASP.NET Web API 2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="0d9f2-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="0d9f2-123">Tout d’abord, nous avons besoin d’une entité associée.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-123">First, we need a related entity.</span></span> <span data-ttu-id="0d9f2-124">Ajoutez une classe nommée `Supplier` dans le dossier Models.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="0d9f2-125">Ajoutez une propriété de navigation à la `Product` classe :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="0d9f2-126">Ajouter un nouveau **DbSet** à la `ProductsContext` classe, afin que Entity Framework inclura la table de fournisseur dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="0d9f2-127">Dans WebApiConfig.cs, ajoutez un &quot;fournisseurs&quot; du jeu d’entités à l’entity data model :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="0d9f2-128">Ajouter un contrôleur de fournisseurs</span><span class="sxs-lookup"><span data-stu-id="0d9f2-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="0d9f2-129">Ajouter un `SuppliersController` classe dans le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="0d9f2-130">Je ne montrent comment ajouter des opérations CRUD pour ce contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="0d9f2-131">Les étapes sont les mêmes que pour le contrôleur de produits (consultez [créer un point de terminaison OData v4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="0d9f2-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="0d9f2-132">Obtention des entités associées</span><span class="sxs-lookup"><span data-stu-id="0d9f2-132">Getting Related Entities</span></span>

<span data-ttu-id="0d9f2-133">Pour obtenir le fournisseur pour un produit, le client envoie une demande GET :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="0d9f2-134">Pour prendre en charge de cette demande, ajoutez la méthode suivante à la `ProductsController` classe :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="0d9f2-135">Cette méthode utilise une convention d’affectation de noms par défaut</span><span class="sxs-lookup"><span data-stu-id="0d9f2-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="0d9f2-136">Nom de la méthode : GetX, où X est la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="0d9f2-137">Nom du paramètre : *clé*</span><span class="sxs-lookup"><span data-stu-id="0d9f2-137">Parameter name: *key*</span></span>

<span data-ttu-id="0d9f2-138">Si vous suivez cette convention d’affectation de noms, les API Web mappe automatiquement la requête HTTP à la méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="0d9f2-139">Exemple HTTP de demande :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="0d9f2-140">Exemple de réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="0d9f2-141">Obtention d’une collection connexe</span><span class="sxs-lookup"><span data-stu-id="0d9f2-141">Getting a related collection</span></span>

<span data-ttu-id="0d9f2-142">Dans l’exemple précédent, un produit est associé à un seul fournisseur.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="0d9f2-143">Une propriété de navigation peut également retourner une collection.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="0d9f2-144">Le code suivant obtient les produits pour un fournisseur :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="0d9f2-145">Dans ce cas, la méthode retourne un **IQueryable** au lieu d’un **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="0d9f2-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="0d9f2-146">Exemple HTTP de demande :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="0d9f2-147">Exemple de réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="0d9f2-148">Création d’une relation entre des entités</span><span class="sxs-lookup"><span data-stu-id="0d9f2-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="0d9f2-149">OData prend en charge la création ou la suppression des relations entre deux entités existantes.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="0d9f2-150">Dans la terminologie d’OData v4, la relation est une &quot;référence&quot;.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="0d9f2-151">(Dans OData v3, la relation a été appelée une *lien*.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="0d9f2-152">Les différences de protocole n’a pas d’importance pour ce didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="0d9f2-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="0d9f2-153">Une référence a son propre URI, le formulaire `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="0d9f2-154">Par exemple, voici l’URI pour résoudre la référence entre un produit et son fournisseur :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="0d9f2-155">Pour ajouter une relation, le client envoie une demande POST ou PUT à cette adresse.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="0d9f2-156">Si la propriété de navigation est une entité unique, tel que `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="0d9f2-157">VALIDER si la propriété de navigation est une collection, tel que `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="0d9f2-158">Le corps de la requête contient l’URI de l’autre entité dans la relation.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="0d9f2-159">Voici un exemple de demande :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="0d9f2-160">Dans cet exemple, le client envoie une demande PUT pour `/Products(6)/Supplier/$ref`, qui est l’URI $ref pour la `Supplier` du produit avec l’ID = 6.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="0d9f2-161">Si la demande aboutit, le serveur envoie une réponse 204 (aucun contenu) :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="0d9f2-162">Voici la méthode de contrôleur pour ajouter une relation à un `Product`:</span><span class="sxs-lookup"><span data-stu-id="0d9f2-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="0d9f2-163">Le *navigationProperty* paramètre spécifie quant à la relation à définir.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="0d9f2-164">(S’il existe plus d’une propriété de navigation sur l’entité, vous pouvez ajouter d’autres `case` instructions.)</span><span class="sxs-lookup"><span data-stu-id="0d9f2-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="0d9f2-165">Le *lien* paramètre contient l’URI du fournisseur.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="0d9f2-166">API Web analyse automatiquement le corps de la requête pour obtenir la valeur de ce paramètre.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="0d9f2-167">Pour rechercher le fournisseur, nous avons besoin de l’ID (ou clé), qui fait partie de la *lien* paramètre.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="0d9f2-168">Pour ce faire, utilisez la méthode d’assistance suivante :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="0d9f2-169">En fait, cette méthode utilise la bibliothèque OData pour fractionner le chemin d’accès URI en segments, recherchez le segment qui contient la clé et convertir la clé en type correct.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="0d9f2-170">Suppression d’une relation entre des entités</span><span class="sxs-lookup"><span data-stu-id="0d9f2-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="0d9f2-171">Pour supprimer une relation, le client envoie une requête HTTP DELETE à l’URI de $ref :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="0d9f2-172">Voici la méthode de contrôleur pour supprimer la relation entre un produit et un fournisseur :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="0d9f2-173">Dans ce cas, `Product.Supplier` est la &quot;1&quot; fin d’une relation 1-à-plusieurs, afin de pouvoir supprimer la relation en définissant simplement `Product.Supplier` à `null`.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="0d9f2-174">Dans le &quot;nombreux&quot; fin d’une relation, le client doit spécifier quels connexes d’entité à supprimer.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="0d9f2-175">Pour ce faire, le client envoie l’URI de l’entité associée dans la chaîne de requête de la demande.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="0d9f2-176">Par exemple, pour supprimer « Product 1 » de « fournisseur 1 » :</span><span class="sxs-lookup"><span data-stu-id="0d9f2-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="0d9f2-177">Pour ce faire dans l’API Web, nous devons inclure un paramètre supplémentaire dans le `DeleteRef` (méthode).</span><span class="sxs-lookup"><span data-stu-id="0d9f2-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="0d9f2-178">Voici la méthode de contrôleur pour supprimer un produit à partir de la `Supplier.Products` relation.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="0d9f2-179">Le *clé* paramètre est la clé pour le fournisseur et le *relatedKey* paramètre est la clé du produit à supprimer de la `Products` relation.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="0d9f2-180">Notez que les API Web obtient automatiquement la clé de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="0d9f2-180">Note that Web API automatically gets the key from the query string.</span></span>
