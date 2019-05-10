---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Attribut de routage dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65125461"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="a6cb6-102">Routage par attributs dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="a6cb6-102">Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="a6cb6-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a6cb6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a6cb6-104">*Routage* est comment l’API Web correspond à un URI à une action.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="a6cb6-105">Web API 2 prend en charge un nouveau type de routage, appelé *routage par attributs*.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="a6cb6-106">Comme son nom l’indique, le routage par attributs utilise les attributs pour définir des itinéraires.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="a6cb6-107">Routage par attributs vous donne davantage de contrôle sur les URI dans votre API web.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="a6cb6-108">Par exemple, vous pouvez facilement créer des URI qui décrivent les hiérarchies de ressources.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="a6cb6-109">Le style antérieures de routage, appelé conventionnelle routage, est toujours intégralement pris en charge.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="a6cb6-110">En fait, vous pouvez combiner ces deux techniques dans le même projet.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="a6cb6-111">Cette rubrique montre comment activer le routage par attributs et décrit les différentes options de routage par attributs.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="a6cb6-112">Pour obtenir un didacticiel de bout en bout qui utilise le routage par attributs, consultez [créer une API REST avec le routage par attributs dans Web API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="a6cb6-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6cb6-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a6cb6-113">Prerequisites</span></span>

<span data-ttu-id="a6cb6-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional ou Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="a6cb6-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="a6cb6-115">Vous pouvez également utiliser Gestionnaire de Package NuGet pour installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="a6cb6-116">À partir de la **outils** menu dans Visual Studio, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a6cb6-117">Entrez la commande suivante dans la fenêtre de Console du Gestionnaire de Package :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="a6cb6-118">Pourquoi routage par attributs ?</span><span class="sxs-lookup"><span data-stu-id="a6cb6-118">Why Attribute Routing?</span></span>

<span data-ttu-id="a6cb6-119">La première version de l’API Web utilisé *conventionnelle* routage.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="a6cb6-120">Dans ce type de routage, vous définissez un ou plusieurs modèles d’itinéraire, qui sont essentiellement des paramétrables de chaînes.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="a6cb6-121">Lorsque l’infrastructure reçoit une demande, elle correspond à l’URI par rapport au modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="a6cb6-122">(Pour plus d’informations sur le routage basé sur une convention, consultez [routage dans ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="a6cb6-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="a6cb6-123">Un avantage du routage basé sur une convention est que les modèles sont définis dans un emplacement unique, et les règles de routage sont appliqués de manière cohérente sur tous les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="a6cb6-124">Malheureusement, le routage basé sur une convention rend difficile prendre en charge de certains modèles d’URI sont courantes dans les API RESTful.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="a6cb6-125">Par exemple, les ressources contiennent souvent des ressources enfants : Les clients ont passé des commandes, films ont des acteurs, la documentation ont des auteurs et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="a6cb6-126">Il est naturel pour créer des URI qui reflètent ces relations :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="a6cb6-127">Ce type d’URI est difficile de créer à l’aide du routage basé sur une convention.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="a6cb6-128">Bien qu’il est possible, les résultats ne l’adaptons pas correctement si vous avez de nombreux contrôleurs ou les types de ressources.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="a6cb6-129">Avec le routage par attributs, il est facile de définir un itinéraire pour cet URI.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="a6cb6-130">Vous ajoutez simplement un attribut à l’action du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="a6cb6-131">Voici d’autres modèles de cet attribut routage rend facile.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="a6cb6-132">**Contrôle de version**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-132">**API versioning**</span></span>

<span data-ttu-id="a6cb6-133">Dans cet exemple, « / api/v1/products » serait routé vers un contrôleur différent de « / v2/api/produits ».</span><span class="sxs-lookup"><span data-stu-id="a6cb6-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="a6cb6-134">**Segments URI surchargés**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="a6cb6-135">Dans cet exemple, « 1 » est un numéro de commande, mais « en attente » correspond à une collection.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="a6cb6-136">**Plusieurs types de paramètres**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-136">**Multiple parameter types**</span></span>

<span data-ttu-id="a6cb6-137">Dans cet exemple, « 1 » est un numéro de commande, mais « 2013/06/16 » spécifie une date.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="a6cb6-138">L’activation de routage par attributs</span><span class="sxs-lookup"><span data-stu-id="a6cb6-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="a6cb6-139">Pour activer le routage par attributs, appelez **MapHttpAttributeRoutes** lors de la configuration.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="a6cb6-140">Cette méthode d’extension est définie dans le **System.Web.Http.HttpConfigurationExtensions** classe.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="a6cb6-141">Routage par attributs peut être combiné avec [conventionnelle](routing-in-aspnet-web-api.md) routage.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="a6cb6-142">Pour définir des itinéraires reposant sur une convention, appelez le **MapHttpRoute** (méthode).</span><span class="sxs-lookup"><span data-stu-id="a6cb6-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="a6cb6-143">Pour plus d’informations sur la configuration des API Web, consultez [configuration ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="a6cb6-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="a6cb6-144">Remarque : Migration à partir de Web API 1</span><span class="sxs-lookup"><span data-stu-id="a6cb6-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="a6cb6-145">Avant d’API Web 2, les modèles de projet API Web généré code similaire à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="a6cb6-146">Si le routage par attributs est activé, ce code lève une exception.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="a6cb6-147">Si vous mettez à niveau un projet API Web existant pour utiliser le routage par attributs, veillez à mettre à jour de ce code de configuration à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="a6cb6-148">Pour plus d’informations, consultez [configuration des API Web avec hébergement ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="a6cb6-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="a6cb6-149">Ajout d’attributs de Route</span><span class="sxs-lookup"><span data-stu-id="a6cb6-149">Adding Route Attributes</span></span>

<span data-ttu-id="a6cb6-150">Voici un exemple d’un itinéraire défini à l’aide d’un attribut :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="a6cb6-151">La chaîne &quot;clients / {customerId} / commandes&quot; est le modèle d’URI pour l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="a6cb6-152">API Web a essaie de correspondre à l’URI de demande pour le modèle.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="a6cb6-153">Dans cet exemple, « customers » et « orders » sont des segments de littéral, et « {customerId} » est un paramètre de variable.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="a6cb6-154">Les URI suivants correspondrait à ce modèle :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="a6cb6-155">Vous pouvez limiter la correspondance à l’aide de [contraintes](#constraints), comme décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="a6cb6-156">Notez que le &quot;{customerId}&quot; paramètre dans le modèle d’itinéraire correspond au nom de la *customerId* paramètre dans la méthode.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="a6cb6-157">Lors de l’API Web appelle l’action du contrôleur, il essaie de lier les paramètres d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="a6cb6-158">Par exemple, si l’URI est `http://example.com/customers/1/orders`, API Web essaie de lier la valeur « 1 » pour le *customerId* paramètre dans l’action.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="a6cb6-159">Un modèle d’URI peut avoir plusieurs paramètres :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="a6cb6-160">Toutes les méthodes de contrôleur qui n’ont pas un attribut d’itinéraire utilisent le routage basé sur une convention.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="a6cb6-161">De cette façon, vous pouvez combiner les deux types de routage dans le même projet.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="a6cb6-162">Méthodes HTTP</span><span class="sxs-lookup"><span data-stu-id="a6cb6-162">HTTP Methods</span></span>

<span data-ttu-id="a6cb6-163">API Web sélectionne également les actions en fonction de la méthode HTTP de la requête (GET, POST, etc.).</span><span class="sxs-lookup"><span data-stu-id="a6cb6-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="a6cb6-164">Par défaut, les API Web recherche une correspondance non sensible à avec le début du nom de la méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="a6cb6-165">Par exemple, une méthode de contrôleur nommée `PutCustomers` correspond à une demande HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="a6cb6-166">Vous pouvez remplacer cette convention en décorant la méthode avec n’importe quel les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="a6cb6-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="a6cb6-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-168">**[HttpGet]**</span></span>
- <span data-ttu-id="a6cb6-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-169">**[HttpHead]**</span></span>
- <span data-ttu-id="a6cb6-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="a6cb6-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="a6cb6-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-172">**[HttpPost]**</span></span>
- <span data-ttu-id="a6cb6-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="a6cb6-173">**[HttpPut]**</span></span>

<span data-ttu-id="a6cb6-174">L’exemple suivant mappe la méthode CreateBook aux requêtes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="a6cb6-175">Pour toutes les autres méthodes HTTP, y compris les méthodes non standards, utilisent la **AcceptVerbs** attribut, qui prend une liste de méthodes HTTP.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="a6cb6-176">Préfixes d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="a6cb6-176">Route Prefixes</span></span>

<span data-ttu-id="a6cb6-177">Souvent, les itinéraires dans un contrôleur commencent toutes par le même préfixe.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="a6cb6-178">Exemple :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="a6cb6-179">Vous pouvez définir un préfixe commun pour l’ensemble du contrôleur à l’aide de la **[RoutePrefix]** attribut :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="a6cb6-180">Utiliser un tilde (~) sur l’attribut de méthode pour remplacer le préfixe d’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="a6cb6-181">Le préfixe d’itinéraire peut inclure des paramètres :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="a6cb6-182">Contraintes de routage</span><span class="sxs-lookup"><span data-stu-id="a6cb6-182">Route Constraints</span></span>

<span data-ttu-id="a6cb6-183">Contraintes de routage vous permettent de limiter la correspondance des paramètres dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="a6cb6-184">La syntaxe générale est &quot;{ : contrainte de paramètre}&quot;.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="a6cb6-185">Exemple :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="a6cb6-186">Ici, le premier itinéraire est uniquement activée si le &quot;id&quot; segment de l’URI est un entier.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="a6cb6-187">Sinon, le deuxième itinéraire est choisi.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="a6cb6-188">Le tableau suivant répertorie les contraintes qui sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="a6cb6-189">Contrainte</span><span class="sxs-lookup"><span data-stu-id="a6cb6-189">Constraint</span></span> | <span data-ttu-id="a6cb6-190">Description</span><span class="sxs-lookup"><span data-stu-id="a6cb6-190">Description</span></span> | <span data-ttu-id="a6cb6-191">Exemple</span><span class="sxs-lookup"><span data-stu-id="a6cb6-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a6cb6-192">alpha</span><span class="sxs-lookup"><span data-stu-id="a6cb6-192">alpha</span></span> | <span data-ttu-id="a6cb6-193">Correspondances en majuscules ou minuscules de l’alphabet Latin (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="a6cb6-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="a6cb6-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-194">{x:alpha}</span></span> |
| <span data-ttu-id="a6cb6-195">bool</span><span class="sxs-lookup"><span data-stu-id="a6cb6-195">bool</span></span> | <span data-ttu-id="a6cb6-196">Correspond à une valeur booléenne.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-196">Matches a Boolean value.</span></span> | <span data-ttu-id="a6cb6-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-197">{x:bool}</span></span> |
| <span data-ttu-id="a6cb6-198">datetime</span><span class="sxs-lookup"><span data-stu-id="a6cb6-198">datetime</span></span> | <span data-ttu-id="a6cb6-199">Correspond à un **DateTime** valeur.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="a6cb6-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-200">{x:datetime}</span></span> |
| <span data-ttu-id="a6cb6-201">decimal</span><span class="sxs-lookup"><span data-stu-id="a6cb6-201">decimal</span></span> | <span data-ttu-id="a6cb6-202">Correspond à une valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-202">Matches a decimal value.</span></span> | <span data-ttu-id="a6cb6-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-203">{x:decimal}</span></span> |
| <span data-ttu-id="a6cb6-204">double</span><span class="sxs-lookup"><span data-stu-id="a6cb6-204">double</span></span> | <span data-ttu-id="a6cb6-205">Correspond à une valeur à virgule flottante 64 bits.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="a6cb6-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-206">{x:double}</span></span> |
| <span data-ttu-id="a6cb6-207">float</span><span class="sxs-lookup"><span data-stu-id="a6cb6-207">float</span></span> | <span data-ttu-id="a6cb6-208">Correspond à une valeur à virgule flottante 32 bits.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="a6cb6-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-209">{x:float}</span></span> |
| <span data-ttu-id="a6cb6-210">guid</span><span class="sxs-lookup"><span data-stu-id="a6cb6-210">guid</span></span> | <span data-ttu-id="a6cb6-211">Correspond à une valeur GUID.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-211">Matches a GUID value.</span></span> | <span data-ttu-id="a6cb6-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-212">{x:guid}</span></span> |
| <span data-ttu-id="a6cb6-213">int</span><span class="sxs-lookup"><span data-stu-id="a6cb6-213">int</span></span> | <span data-ttu-id="a6cb6-214">Correspond à une valeur d’entier 32 bits.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="a6cb6-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-215">{x:int}</span></span> |
| <span data-ttu-id="a6cb6-216">length</span><span class="sxs-lookup"><span data-stu-id="a6cb6-216">length</span></span> | <span data-ttu-id="a6cb6-217">Correspond à une chaîne avec la longueur spécifiée ou dans une plage spécifiée de longueurs.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="a6cb6-218">{x:length(6)} {x:length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="a6cb6-219">long</span><span class="sxs-lookup"><span data-stu-id="a6cb6-219">long</span></span> | <span data-ttu-id="a6cb6-220">Correspond à une valeur d’entier 64 bits.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="a6cb6-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-221">{x:long}</span></span> |
| <span data-ttu-id="a6cb6-222">max</span><span class="sxs-lookup"><span data-stu-id="a6cb6-222">max</span></span> | <span data-ttu-id="a6cb6-223">Correspond à un entier avec une valeur maximale.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="a6cb6-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-224">{x:max(10)}</span></span> |
| <span data-ttu-id="a6cb6-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="a6cb6-225">maxlength</span></span> | <span data-ttu-id="a6cb6-226">Correspond à une chaîne avec une longueur maximale.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="a6cb6-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="a6cb6-228">min</span><span class="sxs-lookup"><span data-stu-id="a6cb6-228">min</span></span> | <span data-ttu-id="a6cb6-229">Correspond à un entier avec une valeur minimale.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="a6cb6-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-230">{x:min(10)}</span></span> |
| <span data-ttu-id="a6cb6-231">minLength</span><span class="sxs-lookup"><span data-stu-id="a6cb6-231">minlength</span></span> | <span data-ttu-id="a6cb6-232">Correspond à une chaîne avec une longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="a6cb6-233">{x:minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="a6cb6-234">range</span><span class="sxs-lookup"><span data-stu-id="a6cb6-234">range</span></span> | <span data-ttu-id="a6cb6-235">Correspond à un entier compris dans une plage de valeurs.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="a6cb6-236">{x:range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="a6cb6-237">regex</span><span class="sxs-lookup"><span data-stu-id="a6cb6-237">regex</span></span> | <span data-ttu-id="a6cb6-238">Correspond à une expression régulière.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-238">Matches a regular expression.</span></span> | <span data-ttu-id="a6cb6-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="a6cb6-240">Notez que certaines des contraintes, telles que &quot;min&quot;, acceptent des arguments entre parenthèses.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="a6cb6-241">Vous pouvez appliquer plusieurs contraintes à un paramètre, séparé par un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="a6cb6-242">Contraintes d’itinéraire personnalisé</span><span class="sxs-lookup"><span data-stu-id="a6cb6-242">Custom Route Constraints</span></span>

<span data-ttu-id="a6cb6-243">Vous pouvez créer des contraintes de routage personnalisées en implémentant la **IHttpRouteConstraint** interface.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="a6cb6-244">Par exemple, la contrainte suivante restreint un paramètre à une valeur entière non nulle.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="a6cb6-245">Le code suivant montre comment inscrire la contrainte :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="a6cb6-246">Maintenant, vous pouvez appliquer la contrainte dans vos routes :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="a6cb6-247">Vous pouvez également remplacer l’intégralité de **DefaultInlineConstraintResolver** classe en implémentant la **IInlineConstraintResolver** interface.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="a6cb6-248">Cela remplace toutes les contraintes intégrées, à moins que votre implémentation de **IInlineConstraintResolver** spécifiquement les ajoute.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="a6cb6-249">Paramètres d’URI facultatif et les valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="a6cb6-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="a6cb6-250">Vous pouvez rendre un paramètre d’URI facultatif en ajoutant un point d’interrogation pour le paramètre d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="a6cb6-251">Si un paramètre d’itinéraire est facultatif, vous devez définir une valeur par défaut pour le paramètre de méthode.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="a6cb6-252">Dans cet exemple, `/api/books/locale/1033` et `/api/books/locale` retournent la même ressource.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="a6cb6-253">Vous pouvez également spécifier une valeur par défaut dans le modèle d’itinéraire, comme suit :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="a6cb6-254">Il est presque identique à l’exemple précédent, mais il existe une légère différence de comportement lorsque la valeur par défaut est appliquée.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="a6cb6-255">Dans le premier exemple (« {lcid:int ?} »), la valeur par défaut 1033 est affectée directement au paramètre de méthode, donc le paramètre aura cette valeur exacte.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-255">In the first example ("{lcid:int?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="a6cb6-256">Dans le deuxième exemple (« {lcid:int = 1033} »), la valeur par défaut de « 1033 » passe par le processus de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-256">In the second example ("{lcid:int=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="a6cb6-257">Le binder de modèle par défaut convertira « 1033 » à la valeur numérique 1033.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="a6cb6-258">Toutefois, vous pouvez connecter dans un classeur de modèles personnalisés, ce qui peut faire quelque chose de différent.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="a6cb6-259">(Dans la plupart des cas, sauf si vous avez des classeurs de modèles personnalisés dans votre pipeline, les deux formes sera équivalentes.)</span><span class="sxs-lookup"><span data-stu-id="a6cb6-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="a6cb6-260">Noms de routes</span><span class="sxs-lookup"><span data-stu-id="a6cb6-260">Route Names</span></span>

<span data-ttu-id="a6cb6-261">Dans l’API Web, chaque routage possède un nom.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-261">In Web API, every route has a name.</span></span> <span data-ttu-id="a6cb6-262">Les noms de routes sont utiles pour générer des liens, afin que vous pouvez inclure un lien dans une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="a6cb6-263">Pour spécifier le nom d’itinéraire, définissez le **nom** propriété sur l’attribut.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="a6cb6-264">L’exemple suivant montre comment définir le nom d’itinéraire et également comment utiliser le nom d’itinéraire lors de la génération d’un lien.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="a6cb6-265">Ordre de l’itinéraire</span><span class="sxs-lookup"><span data-stu-id="a6cb6-265">Route Order</span></span>

<span data-ttu-id="a6cb6-266">Lorsque le framework tente de faire correspondre un URI avec un itinéraire, il évalue les itinéraires dans un ordre particulier.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="a6cb6-267">Pour spécifier l’ordre, définissez le **ordre** propriété sur l’attribut d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="a6cb6-268">Des valeurs plus faibles sont évalués en premier.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-268">Lower values are evaluated first.</span></span> <span data-ttu-id="a6cb6-269">La valeur d’ordre par défaut est égale à zéro.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-269">The default order value is zero.</span></span>

<span data-ttu-id="a6cb6-270">Voici comment le classement total est déterminé :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="a6cb6-271">Comparer la **ordre** propriété de l’attribut d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="a6cb6-272">Examinez chaque segment d’URI dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="a6cb6-273">Pour chaque segment, commande comme suit :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="a6cb6-274">Segments de littéral.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-274">Literal segments.</span></span>
    2. <span data-ttu-id="a6cb6-275">Paramètres d’itinéraire avec des contraintes.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="a6cb6-276">Paramètres d’itinéraire sans contraintes.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="a6cb6-277">Segments de paramètre de caractère générique avec des contraintes.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="a6cb6-278">Segments de paramètre de caractère générique sans contraintes.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="a6cb6-279">Dans le cas d’égalité, les itinéraires sont classés par une comparaison de chaînes ordinale respectant la casse ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) du modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="a6cb6-280">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-280">Here is an example.</span></span> <span data-ttu-id="a6cb6-281">Supposons que vous définissez le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="a6cb6-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="a6cb6-282">Ces itinéraires sont classés comme suit.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="a6cb6-283">orders/details</span><span class="sxs-lookup"><span data-stu-id="a6cb6-283">orders/details</span></span>
2. <span data-ttu-id="a6cb6-284">commandes / {id}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-284">orders/{id}</span></span>
3. <span data-ttu-id="a6cb6-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-285">orders/{customerName}</span></span>
4. <span data-ttu-id="a6cb6-286">orders/{\*date}</span><span class="sxs-lookup"><span data-stu-id="a6cb6-286">orders/{\*date}</span></span>
5. <span data-ttu-id="a6cb6-287">commandes / en attente</span><span class="sxs-lookup"><span data-stu-id="a6cb6-287">orders/pending</span></span>

<span data-ttu-id="a6cb6-288">Notez que « détails » sont un segment littéral et apparaît avant « {id} », mais « en attente » apparaît dernier, car le **ordre** propriété est 1.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="a6cb6-289">(Cet exemple suppose qu’il n’est aucun client nommé « détails » ou « en attente ».</span><span class="sxs-lookup"><span data-stu-id="a6cb6-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="a6cb6-290">En général, essayez d’éviter des itinéraires ambigus.</span><span class="sxs-lookup"><span data-stu-id="a6cb6-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="a6cb6-291">Dans cet exemple, un meilleur modèle d’itinéraire pour `GetByCustomer` est « clients / {customerName} »)</span><span class="sxs-lookup"><span data-stu-id="a6cb6-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
