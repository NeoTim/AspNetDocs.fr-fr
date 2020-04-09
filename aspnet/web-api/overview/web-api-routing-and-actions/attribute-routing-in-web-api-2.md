---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Itinéraire d’attribut dans ASP.NET’API Web 2 (fr) Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: bb68fe8e6769619029a3fa039d6f0d6f3303afbe
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675383"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="bbce2-102">Itinéraire d’attribut dans ASP.NET API Web 2</span><span class="sxs-lookup"><span data-stu-id="bbce2-102">Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="bbce2-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bbce2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bbce2-104">*Routing* est la façon dont l’API Web correspond à une URI à une action.</span><span class="sxs-lookup"><span data-stu-id="bbce2-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="bbce2-105">Web API 2 prend en charge un nouveau type de routage, appelé *itinéraire d’attribut*.</span><span class="sxs-lookup"><span data-stu-id="bbce2-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="bbce2-106">Comme son nom l’indique, le routage d’attribut utilise des attributs pour définir les itinéraires.</span><span class="sxs-lookup"><span data-stu-id="bbce2-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="bbce2-107">Le routage d’attribut vous donne plus de contrôle sur les URL dans votre API Web.</span><span class="sxs-lookup"><span data-stu-id="bbce2-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="bbce2-108">Par exemple, vous pouvez facilement créer des URL qui décrivent les hiérarchies de ressources.</span><span class="sxs-lookup"><span data-stu-id="bbce2-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="bbce2-109">Le style antérieur de routage, appelé itinéraire basé sur les conventions, est toujours entièrement pris en charge.</span><span class="sxs-lookup"><span data-stu-id="bbce2-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="bbce2-110">En fait, vous pouvez combiner les deux techniques dans le même projet.</span><span class="sxs-lookup"><span data-stu-id="bbce2-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="bbce2-111">Ce sujet montre comment activer le routage d’attribut et décrit les différentes options pour le routage d’attribut.</span><span class="sxs-lookup"><span data-stu-id="bbce2-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="bbce2-112">Pour un tutoriel de bout en bout qui utilise le routage d’attribut, voir [Créer une API REST avec Routing Attribut dans l’API Web 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="bbce2-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bbce2-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="bbce2-113">Prerequisites</span></span>

<span data-ttu-id="bbce2-114">[Studio visuel 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Édition communautaire, professionnelle ou d’entreprise</span><span class="sxs-lookup"><span data-stu-id="bbce2-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="bbce2-115">Vous pouvez également utiliser NuGet Package Manager pour installer les paquets nécessaires.</span><span class="sxs-lookup"><span data-stu-id="bbce2-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="bbce2-116">Du menu **Tools** de Visual Studio, sélectionnez **NuGet Package Manager,** puis sélectionnez **La console Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="bbce2-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bbce2-117">Entrez la commande suivante dans la fenêtre console De gestionnaire de paquet :</span><span class="sxs-lookup"><span data-stu-id="bbce2-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="bbce2-118">Pourquoi attribuer le routage?</span><span class="sxs-lookup"><span data-stu-id="bbce2-118">Why Attribute Routing?</span></span>

<span data-ttu-id="bbce2-119">La première version de l’API Web a utilisé le routage *basé sur les conventions.*</span><span class="sxs-lookup"><span data-stu-id="bbce2-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="bbce2-120">Dans ce type de routage, vous définissez un ou plusieurs modèles d’itinéraire, qui sont essentiellement des chaînes paramétrisées.</span><span class="sxs-lookup"><span data-stu-id="bbce2-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="bbce2-121">Lorsque le cadre reçoit une demande, il correspond à l’URI par rapport au modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbce2-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="bbce2-122">Pour plus d’informations sur le routage basé sur les congrès, voir [Routing dans ASP.NET’API Web](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bbce2-122">For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="bbce2-123">L’un des avantages de l’itinéraire fondé sur les conventions est que les modèles sont définis en un seul endroit, et que les règles d’itinéraire sont appliquées uniformément dans tous les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="bbce2-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="bbce2-124">Malheureusement, le routage fondé sur les conventions rend difficile le soutien de certains modèles d’URI qui sont courants dans les API RESTful.</span><span class="sxs-lookup"><span data-stu-id="bbce2-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="bbce2-125">Par exemple, les ressources contiennent souvent des ressources pour enfants : les clients ont des commandes, les films ont des acteurs, les livres ont des auteurs, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="bbce2-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="bbce2-126">Il est naturel de créer des URI qui reflètent ces relations :</span><span class="sxs-lookup"><span data-stu-id="bbce2-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="bbce2-127">Ce type d’URI est difficile à créer à l’aide d’un itinéraire basé sur la convention.</span><span class="sxs-lookup"><span data-stu-id="bbce2-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="bbce2-128">Bien que cela puisse être fait, les résultats ne s’évoluent pas bien si vous avez de nombreux contrôleurs ou types de ressources.</span><span class="sxs-lookup"><span data-stu-id="bbce2-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="bbce2-129">Avec le routage d’attribut, il est trivial de définir une voie pour cette URI.</span><span class="sxs-lookup"><span data-stu-id="bbce2-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="bbce2-130">Il vous suffit d’ajouter un attribut à l’action du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="bbce2-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="bbce2-131">Voici quelques autres modèles qui attribuent le routage rend facile.</span><span class="sxs-lookup"><span data-stu-id="bbce2-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="bbce2-132">**Contrôle de version d’API**</span><span class="sxs-lookup"><span data-stu-id="bbce2-132">**API versioning**</span></span>

<span data-ttu-id="bbce2-133">Dans cet exemple, "/api/v1/products" serait acheminé vers un contrôleur différent de "/api/v2/products".</span><span class="sxs-lookup"><span data-stu-id="bbce2-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="bbce2-134">**Segments URI surchargés**</span><span class="sxs-lookup"><span data-stu-id="bbce2-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="bbce2-135">Dans cet exemple, "1" est un numéro de commande, mais des cartes "en attente" à une collection.</span><span class="sxs-lookup"><span data-stu-id="bbce2-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="bbce2-136">**Types de paramètres multiples**</span><span class="sxs-lookup"><span data-stu-id="bbce2-136">**Multiple parameter types**</span></span>

<span data-ttu-id="bbce2-137">Dans cet exemple, "1" est un numéro de commande, mais "2013/06/16" précise une date.</span><span class="sxs-lookup"><span data-stu-id="bbce2-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="bbce2-138">Permettre le routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="bbce2-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="bbce2-139">Pour activer le routage des attributs, appelez **MapHttpAttributeRoutes** pendant la configuration.</span><span class="sxs-lookup"><span data-stu-id="bbce2-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="bbce2-140">Cette méthode d’extension est définie dans la classe **System.Web.Http.HttpConfigurationExtensions.**</span><span class="sxs-lookup"><span data-stu-id="bbce2-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="bbce2-141">Le routage d’attribut peut être combiné avec le routage [basé sur la convention.](routing-in-aspnet-web-api.md)</span><span class="sxs-lookup"><span data-stu-id="bbce2-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="bbce2-142">Pour définir les itinéraires basés sur les conventions, appelez la méthode **MapHttpRoute.**</span><span class="sxs-lookup"><span data-stu-id="bbce2-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="bbce2-143">Pour plus d’informations sur la configuration de l’API Web, voir [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bbce2-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="bbce2-144">Note: Migreing From Web API 1</span><span class="sxs-lookup"><span data-stu-id="bbce2-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="bbce2-145">Avant l’API Web 2, les modèles de projet Web API ont généré du code comme celui-ci :</span><span class="sxs-lookup"><span data-stu-id="bbce2-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="bbce2-146">Si le routage d’attribut est activé, ce code jettera une exception.</span><span class="sxs-lookup"><span data-stu-id="bbce2-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="bbce2-147">Si vous mettez à niveau un projet d’API Web existant pour utiliser le routage d’attribut, assurez-vous de mettre à jour ce code de configuration à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="bbce2-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="bbce2-148">Pour plus d’informations, voir [Configuring Web API avec ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="bbce2-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="bbce2-149">Ajout d’attributs d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="bbce2-149">Adding Route Attributes</span></span>

<span data-ttu-id="bbce2-150">Voici un exemple d’itinéraire défini à l’aide d’un attribut :</span><span class="sxs-lookup"><span data-stu-id="bbce2-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="bbce2-151">Les &quot;clients de la chaîne/customerId/commandes&quot; sont le modèle URI pour l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbce2-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="bbce2-152">L’API Web tente de faire correspondre la demande URI au modèle.</span><span class="sxs-lookup"><span data-stu-id="bbce2-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="bbce2-153">Dans cet exemple, les « clients » et les « commandes » sont des segments littérals, et « CustomerId » est un paramètre variable.</span><span class="sxs-lookup"><span data-stu-id="bbce2-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="bbce2-154">Les URL suivantes correspondraient à ce modèle :</span><span class="sxs-lookup"><span data-stu-id="bbce2-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="bbce2-155">Vous pouvez restreindre l’appariement en utilisant des [contraintes](#constraints), décrites plus tard dans ce sujet.</span><span class="sxs-lookup"><span data-stu-id="bbce2-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="bbce2-156">Notez &quot;que le&quot; paramètre «customerId» dans le modèle d’itinéraire correspond au nom du *paramètre customerId* dans la méthode.</span><span class="sxs-lookup"><span data-stu-id="bbce2-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="bbce2-157">Lorsque l’API Web invoque l’action du contrôleur, il essaie de lier les paramètres d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbce2-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="bbce2-158">Par exemple, si l’URI est, `http://example.com/customers/1/orders`l’API Web tente de lier la valeur "1" au *paramètre customerId* dans l’action.</span><span class="sxs-lookup"><span data-stu-id="bbce2-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="bbce2-159">Un modèle URI peut avoir plusieurs paramètres :</span><span class="sxs-lookup"><span data-stu-id="bbce2-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="bbce2-160">Toutes les méthodes de contrôleur qui n’ont pas d’attribut d’itinéraire utilisent le routage basé sur la convention.</span><span class="sxs-lookup"><span data-stu-id="bbce2-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="bbce2-161">De cette façon, vous pouvez combiner les deux types de routage dans le même projet.</span><span class="sxs-lookup"><span data-stu-id="bbce2-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="bbce2-162">HTTP Methods</span><span class="sxs-lookup"><span data-stu-id="bbce2-162">HTTP Methods</span></span>

<span data-ttu-id="bbce2-163">L’API Web sélectionne également les actions basées sur la méthode HTTP de la demande (GET, POST, etc.).</span><span class="sxs-lookup"><span data-stu-id="bbce2-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="bbce2-164">Par défaut, l’API Web recherche un match insensible au cas avec le début du nom de la méthode du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="bbce2-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="bbce2-165">Par exemple, une `PutCustomers` méthode de contrôleur nommée correspond à une demande HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="bbce2-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="bbce2-166">Vous pouvez passer outre à cette convention en décorant la méthode avec tous les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="bbce2-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="bbce2-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="bbce2-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="bbce2-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="bbce2-168">**[HttpGet]**</span></span>
- <span data-ttu-id="bbce2-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="bbce2-169">**[HttpHead]**</span></span>
- <span data-ttu-id="bbce2-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="bbce2-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="bbce2-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="bbce2-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="bbce2-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="bbce2-172">**[HttpPost]**</span></span>
- <span data-ttu-id="bbce2-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="bbce2-173">**[HttpPut]**</span></span>

<span data-ttu-id="bbce2-174">L’exemple suivant cartographie la méthode CreateBook aux demandes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="bbce2-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="bbce2-175">Pour toutes les autres méthodes HTTP, y compris les méthodes non standard, utilisez **l’attribut AcceptVerbs,** qui prend une liste de méthodes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bbce2-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="bbce2-176">Préfixes d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="bbce2-176">Route Prefixes</span></span>

<span data-ttu-id="bbce2-177">Souvent, les itinéraires dans un contrôleur commencent tous avec le même préfixe.</span><span class="sxs-lookup"><span data-stu-id="bbce2-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="bbce2-178">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="bbce2-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="bbce2-179">Vous pouvez définir un préfixe commun pour un contrôleur entier en utilisant **l’attribut [RoutePrefix]** :</span><span class="sxs-lookup"><span data-stu-id="bbce2-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="bbce2-180">Utilisez un tilde sur l’attribut de méthode pour remplacer le préfixe de l’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="bbce2-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="bbce2-181">Le préfixe de l’itinéraire peut inclure des paramètres :</span><span class="sxs-lookup"><span data-stu-id="bbce2-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="bbce2-182">Contraintes d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="bbce2-182">Route Constraints</span></span>

<span data-ttu-id="bbce2-183">Les contraintes d’itinéraire vous permettent de limiter la façon dont les paramètres du modèle d’itinéraire sont appariés.</span><span class="sxs-lookup"><span data-stu-id="bbce2-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="bbce2-184">La syntaxe &quot;générale est «paramètre:contrainte».&quot;</span><span class="sxs-lookup"><span data-stu-id="bbce2-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="bbce2-185">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="bbce2-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="bbce2-186">Ici, le premier itinéraire ne &quot;sera&quot; choisi que si le segment d’id de l’URI est un intégrant.</span><span class="sxs-lookup"><span data-stu-id="bbce2-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="bbce2-187">Dans le cas contraire, le deuxième itinéraire sera choisi.</span><span class="sxs-lookup"><span data-stu-id="bbce2-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="bbce2-188">Le tableau suivant énumère les contraintes qui sont prises en charge.</span><span class="sxs-lookup"><span data-stu-id="bbce2-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="bbce2-189">Contrainte</span><span class="sxs-lookup"><span data-stu-id="bbce2-189">Constraint</span></span> | <span data-ttu-id="bbce2-190">Description</span><span class="sxs-lookup"><span data-stu-id="bbce2-190">Description</span></span> | <span data-ttu-id="bbce2-191">Exemple</span><span class="sxs-lookup"><span data-stu-id="bbce2-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bbce2-192">alpha</span><span class="sxs-lookup"><span data-stu-id="bbce2-192">alpha</span></span> | <span data-ttu-id="bbce2-193">Correspond aux caractères de l’alphabet latin majuscule ou inférieur (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="bbce2-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="bbce2-194">X:alpha</span><span class="sxs-lookup"><span data-stu-id="bbce2-194">{x:alpha}</span></span> |
| <span data-ttu-id="bbce2-195">bool</span><span class="sxs-lookup"><span data-stu-id="bbce2-195">bool</span></span> | <span data-ttu-id="bbce2-196">Correspond à une valeur Boolean.</span><span class="sxs-lookup"><span data-stu-id="bbce2-196">Matches a Boolean value.</span></span> | <span data-ttu-id="bbce2-197">X:bool</span><span class="sxs-lookup"><span data-stu-id="bbce2-197">{x:bool}</span></span> |
| <span data-ttu-id="bbce2-198">DATETIME</span><span class="sxs-lookup"><span data-stu-id="bbce2-198">datetime</span></span> | <span data-ttu-id="bbce2-199">Correspond à une valeur **DateTime.**</span><span class="sxs-lookup"><span data-stu-id="bbce2-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="bbce2-200">X: date d’actualité</span><span class="sxs-lookup"><span data-stu-id="bbce2-200">{x:datetime}</span></span> |
| <span data-ttu-id="bbce2-201">Décimal</span><span class="sxs-lookup"><span data-stu-id="bbce2-201">decimal</span></span> | <span data-ttu-id="bbce2-202">Correspond à une valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="bbce2-202">Matches a decimal value.</span></span> | <span data-ttu-id="bbce2-203">X:décimale</span><span class="sxs-lookup"><span data-stu-id="bbce2-203">{x:decimal}</span></span> |
| <span data-ttu-id="bbce2-204">double</span><span class="sxs-lookup"><span data-stu-id="bbce2-204">double</span></span> | <span data-ttu-id="bbce2-205">Correspond à une valeur de 64 bits de point flottant.</span><span class="sxs-lookup"><span data-stu-id="bbce2-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="bbce2-206">X:double</span><span class="sxs-lookup"><span data-stu-id="bbce2-206">{x:double}</span></span> |
| <span data-ttu-id="bbce2-207">float</span><span class="sxs-lookup"><span data-stu-id="bbce2-207">float</span></span> | <span data-ttu-id="bbce2-208">Correspond à une valeur de 32 bits de point flottant.</span><span class="sxs-lookup"><span data-stu-id="bbce2-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="bbce2-209">X:float</span><span class="sxs-lookup"><span data-stu-id="bbce2-209">{x:float}</span></span> |
| <span data-ttu-id="bbce2-210">guid</span><span class="sxs-lookup"><span data-stu-id="bbce2-210">guid</span></span> | <span data-ttu-id="bbce2-211">Correspond à une valeur GUID.</span><span class="sxs-lookup"><span data-stu-id="bbce2-211">Matches a GUID value.</span></span> | <span data-ttu-id="bbce2-212">X:guid</span><span class="sxs-lookup"><span data-stu-id="bbce2-212">{x:guid}</span></span> |
| <span data-ttu-id="bbce2-213">int</span><span class="sxs-lookup"><span data-stu-id="bbce2-213">int</span></span> | <span data-ttu-id="bbce2-214">Correspond à une valeur integer 32 bits.</span><span class="sxs-lookup"><span data-stu-id="bbce2-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="bbce2-215">X:int</span><span class="sxs-lookup"><span data-stu-id="bbce2-215">{x:int}</span></span> |
| <span data-ttu-id="bbce2-216">length</span><span class="sxs-lookup"><span data-stu-id="bbce2-216">length</span></span> | <span data-ttu-id="bbce2-217">Assorti une chaîne avec la longueur spécifiée ou dans une plage spécifiée de longueurs.</span><span class="sxs-lookup"><span data-stu-id="bbce2-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="bbce2-218">X:longueur(6) X:longueur(1,20)</span><span class="sxs-lookup"><span data-stu-id="bbce2-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="bbce2-219">long</span><span class="sxs-lookup"><span data-stu-id="bbce2-219">long</span></span> | <span data-ttu-id="bbce2-220">Correspond à une valeur integer 64 bits.</span><span class="sxs-lookup"><span data-stu-id="bbce2-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="bbce2-221">X:long</span><span class="sxs-lookup"><span data-stu-id="bbce2-221">{x:long}</span></span> |
| <span data-ttu-id="bbce2-222">max</span><span class="sxs-lookup"><span data-stu-id="bbce2-222">max</span></span> | <span data-ttu-id="bbce2-223">Assortie un intégrant avec une valeur maximale.</span><span class="sxs-lookup"><span data-stu-id="bbce2-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="bbce2-224">X:max(10)</span><span class="sxs-lookup"><span data-stu-id="bbce2-224">{x:max(10)}</span></span> |
| <span data-ttu-id="bbce2-225">Maxlength</span><span class="sxs-lookup"><span data-stu-id="bbce2-225">maxlength</span></span> | <span data-ttu-id="bbce2-226">Assortie une corde d’une longueur maximale.</span><span class="sxs-lookup"><span data-stu-id="bbce2-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="bbce2-227">X:maxlength(10)</span><span class="sxs-lookup"><span data-stu-id="bbce2-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="bbce2-228">min</span><span class="sxs-lookup"><span data-stu-id="bbce2-228">min</span></span> | <span data-ttu-id="bbce2-229">Assortie un intégrant d’une valeur minimale.</span><span class="sxs-lookup"><span data-stu-id="bbce2-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="bbce2-230">X:min(10)</span><span class="sxs-lookup"><span data-stu-id="bbce2-230">{x:min(10)}</span></span> |
| <span data-ttu-id="bbce2-231">minlength</span><span class="sxs-lookup"><span data-stu-id="bbce2-231">minlength</span></span> | <span data-ttu-id="bbce2-232">Assorti une chaîne d’une longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="bbce2-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="bbce2-233">X:minlength(10)</span><span class="sxs-lookup"><span data-stu-id="bbce2-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="bbce2-234">range</span><span class="sxs-lookup"><span data-stu-id="bbce2-234">range</span></span> | <span data-ttu-id="bbce2-235">Correspond à un intégrant dans une gamme de valeurs.</span><span class="sxs-lookup"><span data-stu-id="bbce2-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="bbce2-236">X:gamme (10,50)</span><span class="sxs-lookup"><span data-stu-id="bbce2-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="bbce2-237">regex</span><span class="sxs-lookup"><span data-stu-id="bbce2-237">regex</span></span> | <span data-ttu-id="bbce2-238">Correspond à une expression régulière.</span><span class="sxs-lookup"><span data-stu-id="bbce2-238">Matches a regular expression.</span></span> | <span data-ttu-id="bbce2-239">X:regex (d{3}{3}-d{4}-d $)</span><span class="sxs-lookup"><span data-stu-id="bbce2-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="bbce2-240">Notez que certaines des &quot;contraintes, telles que min&quot;, prendre des arguments entre parenthèses.</span><span class="sxs-lookup"><span data-stu-id="bbce2-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="bbce2-241">Vous pouvez appliquer de multiples contraintes à un paramètre, séparé par un côlon.</span><span class="sxs-lookup"><span data-stu-id="bbce2-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="bbce2-242">Contraintes d’itinéraire personnalisé</span><span class="sxs-lookup"><span data-stu-id="bbce2-242">Custom Route Constraints</span></span>

<span data-ttu-id="bbce2-243">Vous pouvez créer des contraintes d’itinéraire personnalisées en implémentant l’interface **IHttpRouteConstraint.**</span><span class="sxs-lookup"><span data-stu-id="bbce2-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="bbce2-244">Par exemple, la contrainte suivante limite un paramètre à une valeur non nulle.</span><span class="sxs-lookup"><span data-stu-id="bbce2-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="bbce2-245">Le code suivant montre comment enregistrer la contrainte :</span><span class="sxs-lookup"><span data-stu-id="bbce2-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="bbce2-246">Vous pouvez maintenant appliquer la contrainte dans vos itinéraires :</span><span class="sxs-lookup"><span data-stu-id="bbce2-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="bbce2-247">Vous pouvez également remplacer l’ensemble de la classe **DefaultInlineConstraintResolver** en implémentant l’interface **IInlineConstraintResolver.**</span><span class="sxs-lookup"><span data-stu-id="bbce2-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="bbce2-248">Cela remplacera toutes les contraintes intégrées, à moins que votre mise en œuvre **d’IInlineConstraintResolver** les ajoute spécifiquement.</span><span class="sxs-lookup"><span data-stu-id="bbce2-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="bbce2-249">Paramètres URI optionnels et valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="bbce2-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="bbce2-250">Vous pouvez rendre un paramètre URI facultatif en ajoutant un point d’interrogation au paramètre d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbce2-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="bbce2-251">Si un paramètre d’itinéraire est facultatif, vous devez définir une valeur par défaut pour le paramètre de la méthode.</span><span class="sxs-lookup"><span data-stu-id="bbce2-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="bbce2-252">Dans cet `/api/books/locale/1033` exemple, et `/api/books/locale` retourner la même ressource.</span><span class="sxs-lookup"><span data-stu-id="bbce2-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="bbce2-253">Vous pouvez également spécifier une valeur par défaut à l’intérieur du modèle d’itinéraire, comme suit :</span><span class="sxs-lookup"><span data-stu-id="bbce2-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="bbce2-254">C’est presque la même chose que l’exemple précédent, mais il ya une légère différence de comportement lorsque la valeur par défaut est appliquée.</span><span class="sxs-lookup"><span data-stu-id="bbce2-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="bbce2-255">Dans le premier exemple (« lcid : int ? »), la valeur par défaut de 1033 est attribuée directement au paramètre de la méthode, de sorte que le paramètre aura cette valeur exacte.</span><span class="sxs-lookup"><span data-stu-id="bbce2-255">In the first example ("{lcid:int?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="bbce2-256">Dans le deuxième exemple (« lcid : int-1033 »), la valeur par défaut de « 1033 » passe par le processus de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="bbce2-256">In the second example ("{lcid:int=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="bbce2-257">Le reliure de modèle par défaut convertira « 1033 » à la valeur numérique 1033.</span><span class="sxs-lookup"><span data-stu-id="bbce2-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="bbce2-258">Cependant, vous pouvez brancher un classeur de modèle personnalisé, qui pourrait faire quelque chose de différent.</span><span class="sxs-lookup"><span data-stu-id="bbce2-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="bbce2-259">(Dans la plupart des cas, à moins d’avoir des liants modèles personnalisés dans votre pipeline, les deux formulaires seront équivalents.)</span><span class="sxs-lookup"><span data-stu-id="bbce2-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="bbce2-260">Noms d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="bbce2-260">Route Names</span></span>

<span data-ttu-id="bbce2-261">Dans l’API Web, chaque itinéraire a un nom.</span><span class="sxs-lookup"><span data-stu-id="bbce2-261">In Web API, every route has a name.</span></span> <span data-ttu-id="bbce2-262">Les noms d’itinéraires sont utiles pour générer des liens, de sorte que vous pouvez inclure un lien dans une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="bbce2-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="bbce2-263">Pour spécifier le nom de l’itinéraire, définissez la propriété **Name** sur l’attribut.</span><span class="sxs-lookup"><span data-stu-id="bbce2-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="bbce2-264">L’exemple suivant montre comment définir le nom de l’itinéraire, et aussi comment utiliser le nom de l’itinéraire lors de la génération d’un lien.</span><span class="sxs-lookup"><span data-stu-id="bbce2-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="bbce2-265">Ordre d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="bbce2-265">Route Order</span></span>

<span data-ttu-id="bbce2-266">Lorsque le cadre tente de faire correspondre un URI à un itinéraire, il évalue les itinéraires dans un ordre particulier.</span><span class="sxs-lookup"><span data-stu-id="bbce2-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="bbce2-267">Pour spécifier l’ordre, définissez la propriété **de l’Ordre** sur l’attribut de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbce2-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="bbce2-268">Les valeurs inférieures sont évaluées en premier.</span><span class="sxs-lookup"><span data-stu-id="bbce2-268">Lower values are evaluated first.</span></span> <span data-ttu-id="bbce2-269">La valeur de commande par défaut est nulle.</span><span class="sxs-lookup"><span data-stu-id="bbce2-269">The default order value is zero.</span></span>

<span data-ttu-id="bbce2-270">Voici comment la commande totale est déterminée :</span><span class="sxs-lookup"><span data-stu-id="bbce2-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="bbce2-271">Comparez la propriété **de l’Ordre** de l’attribut de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbce2-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="bbce2-272">Regardez chaque segment URI dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbce2-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="bbce2-273">Pour chaque segment, commandez comme suit :</span><span class="sxs-lookup"><span data-stu-id="bbce2-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="bbce2-274">Segments littérales.</span><span class="sxs-lookup"><span data-stu-id="bbce2-274">Literal segments.</span></span>
    2. <span data-ttu-id="bbce2-275">Paramètres d’itinéraire avec contraintes.</span><span class="sxs-lookup"><span data-stu-id="bbce2-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="bbce2-276">Paramètres d’itinéraire sans contraintes.</span><span class="sxs-lookup"><span data-stu-id="bbce2-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="bbce2-277">Segments de paramètres Wildcard avec des contraintes.</span><span class="sxs-lookup"><span data-stu-id="bbce2-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="bbce2-278">Segments de paramètres Wildcard sans contraintes.</span><span class="sxs-lookup"><span data-stu-id="bbce2-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="bbce2-279">Dans le cas d’une égalité, les itinéraires sont ordonnés par une comparaison de chaîne ordinaire insensible au cas ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) du modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbce2-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="bbce2-280">Voici un exemple.</span><span class="sxs-lookup"><span data-stu-id="bbce2-280">Here is an example.</span></span> <span data-ttu-id="bbce2-281">Supposons que vous définissiez le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="bbce2-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="bbce2-282">Ces itinéraires sont commandés comme suit.</span><span class="sxs-lookup"><span data-stu-id="bbce2-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="bbce2-283">commandes/détails</span><span class="sxs-lookup"><span data-stu-id="bbce2-283">orders/details</span></span>
2. <span data-ttu-id="bbce2-284">commandes/id</span><span class="sxs-lookup"><span data-stu-id="bbce2-284">orders/{id}</span></span>
3. <span data-ttu-id="bbce2-285">commandes/'customerName'</span><span class="sxs-lookup"><span data-stu-id="bbce2-285">orders/{customerName}</span></span>
4. <span data-ttu-id="bbce2-286">commandes/\*date de</span><span class="sxs-lookup"><span data-stu-id="bbce2-286">orders/{\*date}</span></span>
5. <span data-ttu-id="bbce2-287">commandes/en attente</span><span class="sxs-lookup"><span data-stu-id="bbce2-287">orders/pending</span></span>

<span data-ttu-id="bbce2-288">Notez que les « détails » sont un segment littéral et apparaissent avant « id », mais « en attente » apparaît en dernier parce que la propriété **de l’Ordre** est de 1.</span><span class="sxs-lookup"><span data-stu-id="bbce2-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="bbce2-289">(Cet exemple suppose qu’il n’y a pas de clients nommés «détails» ou «en attente».</span><span class="sxs-lookup"><span data-stu-id="bbce2-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="bbce2-290">En général, essayez d’éviter les voies ambigues.</span><span class="sxs-lookup"><span data-stu-id="bbce2-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="bbce2-291">Dans cet exemple, un `GetByCustomer` meilleur modèle d’itinéraire pour est "clients / clientName" )</span><span class="sxs-lookup"><span data-stu-id="bbce2-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
