---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing dans ASP.NET’API Web (fr) Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676131"
---
# <a name="routing-in-aspnet-web-api"></a><span data-ttu-id="efa53-102">Routing in ASP.NET Web API (Routage dans l’API Web ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="efa53-102">Routing in ASP.NET Web API</span></span>

<span data-ttu-id="efa53-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="efa53-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="efa53-104">Cet article décrit comment ASP.NET’API Web achemine les demandes HTTP aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="efa53-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="efa53-105">Si vous connaissez ASP.NET MVC, le routage de l’API Web est très similaire au routage MVC.</span><span class="sxs-lookup"><span data-stu-id="efa53-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="efa53-106">La principale différence est que l’API Web utilise le verbe HTTP, et non le chemin URI, pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="efa53-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="efa53-107">Vous pouvez également utiliser le routage de style MVC dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="efa53-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="efa53-108">Cet article n’assume aucune connaissance de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="efa53-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="efa53-109">Tables de routage</span><span class="sxs-lookup"><span data-stu-id="efa53-109">Routing Tables</span></span>

<span data-ttu-id="efa53-110">Dans ASP.NET’API Web, un *contrôleur* est une classe qui traite les demandes HTTP.</span><span class="sxs-lookup"><span data-stu-id="efa53-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="efa53-111">Les méthodes publiques du contrôleur sont appelées *méthodes d’action* ou simplement *des actions*.</span><span class="sxs-lookup"><span data-stu-id="efa53-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="efa53-112">Lorsque le cadre de l’API Web reçoit une demande, il achemine la demande à une action.</span><span class="sxs-lookup"><span data-stu-id="efa53-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="efa53-113">Pour déterminer quelle action invoquer, le cadre utilise un *tableau de routage*.</span><span class="sxs-lookup"><span data-stu-id="efa53-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="efa53-114">Le modèle de projet Visual Studio pour l’API Web crée un itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="efa53-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="efa53-115">Cet itinéraire est défini dans le fichier *WebApiConfig.cs,* qui est placé dans *l’annuaire\_App Start* :</span><span class="sxs-lookup"><span data-stu-id="efa53-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="efa53-116">Pour plus d’informations sur la `WebApiConfig` classe, voir [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="efa53-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="efa53-117">Si vous auto-hébergez l’API Web, vous `HttpSelfHostConfiguration` devez régler la table de routage directement sur l’objet.</span><span class="sxs-lookup"><span data-stu-id="efa53-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="efa53-118">Pour plus d’informations, voir [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="efa53-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="efa53-119">Chaque entrée dans le tableau de routage contient un *modèle d’itinéraire*.</span><span class="sxs-lookup"><span data-stu-id="efa53-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="efa53-120">Le modèle d’itinéraire par &quot;défaut pour l’API&quot;Web est api/'controller'/id' .</span><span class="sxs-lookup"><span data-stu-id="efa53-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="efa53-121">Dans ce &quot;modèle,&quot; api est un segment de chemin littéral, et «contrôleur» et «id» sont des variables placeholder.</span><span class="sxs-lookup"><span data-stu-id="efa53-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="efa53-122">Lorsque le cadre Web API reçoit une demande HTTP, il essaie de faire correspondre l’URI à l’un des modèles d’itinéraire dans la table de routage.</span><span class="sxs-lookup"><span data-stu-id="efa53-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="efa53-123">Si aucun itinéraire ne correspond, le client reçoit une erreur de 404.</span><span class="sxs-lookup"><span data-stu-id="efa53-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="efa53-124">Par exemple, les URL suivantes correspondent à l’itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="efa53-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="efa53-125">/api/contacts</span><span class="sxs-lookup"><span data-stu-id="efa53-125">/api/contacts</span></span>
- <span data-ttu-id="efa53-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="efa53-126">/api/contacts/1</span></span>
- <span data-ttu-id="efa53-127">/api/produits/gizmo1</span><span class="sxs-lookup"><span data-stu-id="efa53-127">/api/products/gizmo1</span></span>

<span data-ttu-id="efa53-128">Cependant, l’URI suivante ne correspond pas, car il manque le &quot;segment api:&quot;</span><span class="sxs-lookup"><span data-stu-id="efa53-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="efa53-129">/contacts/1</span><span class="sxs-lookup"><span data-stu-id="efa53-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="efa53-130">La raison de l’utilisation de "api" dans l’itinéraire est d’éviter les collisions avec ASP.NET itinéraire MVC.</span><span class="sxs-lookup"><span data-stu-id="efa53-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="efa53-131">De cette façon, &quot;vous&quot; pouvez avoir /contacts &quot;aller à un&quot; contrôleur MVC, et /api / contacts aller à un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="efa53-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="efa53-132">Bien sûr, si vous n’aimez pas cette convention, vous pouvez modifier le tableau d’itinéraire par défaut.</span><span class="sxs-lookup"><span data-stu-id="efa53-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="efa53-133">Une fois qu’un itinéraire correspondant est trouvé, l’API Web sélectionne le contrôleur et l’action :</span><span class="sxs-lookup"><span data-stu-id="efa53-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="efa53-134">Pour trouver le contrôleur, &quot;l’API Web ajoute controller&quot; à la valeur de la variable *« contrôleur* ».</span><span class="sxs-lookup"><span data-stu-id="efa53-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="efa53-135">Pour trouver l’action, Web API regarde le verbe HTTP, puis cherche une action dont le nom commence par ce nom de verbe HTTP.</span><span class="sxs-lookup"><span data-stu-id="efa53-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="efa53-136">Par exemple, avec une demande GET, Web API &quot;recherche&quot;une &quot;action préfixée &quot;avec Get ,&quot;comme GetContact&quot; ou GetAllContacts .</span><span class="sxs-lookup"><span data-stu-id="efa53-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="efa53-137">Cette convention ne s’applique qu’aux verbes GET, POST, PUT, DELETE, HEAD, OPTIONS et PATCH.</span><span class="sxs-lookup"><span data-stu-id="efa53-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="efa53-138">Vous pouvez activer d’autres verbes HTTP en utilisant des attributs sur votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="efa53-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="efa53-139">Nous verrons un exemple de cela plus tard.</span><span class="sxs-lookup"><span data-stu-id="efa53-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="efa53-140">D’autres variables de place dans le modèle d’itinéraire, telles que « id », sont cartographiées en paramètres *d’action.*</span><span class="sxs-lookup"><span data-stu-id="efa53-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="efa53-141">Intéressons-nous à un exemple.</span><span class="sxs-lookup"><span data-stu-id="efa53-141">Let's look at an example.</span></span> <span data-ttu-id="efa53-142">Supposons que vous définissiez le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="efa53-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="efa53-143">Voici quelques demandes http://s d’accueil possibles, ainsi que l’action qui est invoquée pour chacune :</span><span class="sxs-lookup"><span data-stu-id="efa53-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="efa53-144">Verbe HTTP</span><span class="sxs-lookup"><span data-stu-id="efa53-144">HTTP Verb</span></span> | <span data-ttu-id="efa53-145">Chemin URI</span><span class="sxs-lookup"><span data-stu-id="efa53-145">URI Path</span></span> | <span data-ttu-id="efa53-146">Action</span><span class="sxs-lookup"><span data-stu-id="efa53-146">Action</span></span> | <span data-ttu-id="efa53-147">Paramètre</span><span class="sxs-lookup"><span data-stu-id="efa53-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="efa53-148">GET</span><span class="sxs-lookup"><span data-stu-id="efa53-148">GET</span></span> | <span data-ttu-id="efa53-149">api/produits</span><span class="sxs-lookup"><span data-stu-id="efa53-149">api/products</span></span> | <span data-ttu-id="efa53-150">GetAllProducts (en)</span><span class="sxs-lookup"><span data-stu-id="efa53-150">GetAllProducts</span></span> | <span data-ttu-id="efa53-151">*(aucun)*</span><span class="sxs-lookup"><span data-stu-id="efa53-151">*(none)*</span></span> |
| <span data-ttu-id="efa53-152">GET</span><span class="sxs-lookup"><span data-stu-id="efa53-152">GET</span></span> | <span data-ttu-id="efa53-153">api/produits/4</span><span class="sxs-lookup"><span data-stu-id="efa53-153">api/products/4</span></span> | <span data-ttu-id="efa53-154">GetProductById GetProductById</span><span class="sxs-lookup"><span data-stu-id="efa53-154">GetProductById</span></span> | <span data-ttu-id="efa53-155">4</span><span class="sxs-lookup"><span data-stu-id="efa53-155">4</span></span> |
| <span data-ttu-id="efa53-156">Suppression</span><span class="sxs-lookup"><span data-stu-id="efa53-156">DELETE</span></span> | <span data-ttu-id="efa53-157">api/produits/4</span><span class="sxs-lookup"><span data-stu-id="efa53-157">api/products/4</span></span> | <span data-ttu-id="efa53-158">DeleteProduct (DeleteProduct)</span><span class="sxs-lookup"><span data-stu-id="efa53-158">DeleteProduct</span></span> | <span data-ttu-id="efa53-159">4</span><span class="sxs-lookup"><span data-stu-id="efa53-159">4</span></span> |
| <span data-ttu-id="efa53-160">POST</span><span class="sxs-lookup"><span data-stu-id="efa53-160">POST</span></span> | <span data-ttu-id="efa53-161">api/produits</span><span class="sxs-lookup"><span data-stu-id="efa53-161">api/products</span></span> | <span data-ttu-id="efa53-162">*(pas de correspondance)*</span><span class="sxs-lookup"><span data-stu-id="efa53-162">*(no match)*</span></span> |  |

<span data-ttu-id="efa53-163">Notez que le segment *«id»* de l’URI, s’il est présent, est cartographié sur le paramètre *d’identification* de l’action.</span><span class="sxs-lookup"><span data-stu-id="efa53-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="efa53-164">Dans cet exemple, le contrôleur définit deux méthodes GET, l’une avec un *paramètre d’identification* et l’autre sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="efa53-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="efa53-165">Aussi, notez que la demande POST échouera, &quot;parce que le contrôleur ne définit pas un poste ... &quot; méthode.</span><span class="sxs-lookup"><span data-stu-id="efa53-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="efa53-166">Variations de routage</span><span class="sxs-lookup"><span data-stu-id="efa53-166">Routing Variations</span></span>

<span data-ttu-id="efa53-167">La section précédente décrivait le mécanisme de routage de base pour ASP.NET’API Web.</span><span class="sxs-lookup"><span data-stu-id="efa53-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="efa53-168">Cette section décrit certaines variations.</span><span class="sxs-lookup"><span data-stu-id="efa53-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="efa53-169">Verbes HTTP</span><span class="sxs-lookup"><span data-stu-id="efa53-169">HTTP verbs</span></span>

<span data-ttu-id="efa53-170">Au lieu d’utiliser la convention de nommage pour les verbes HTTP, vous pouvez spécifier explicitement le verbe HTTP pour une action en décorant la méthode d’action avec l’un des attributs suivants:</span><span class="sxs-lookup"><span data-stu-id="efa53-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="efa53-171">Dans l’exemple `FindProduct` suivant, la méthode est cartographiée pour les demandes GET :</span><span class="sxs-lookup"><span data-stu-id="efa53-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="efa53-172">Pour permettre plusieurs verbes HTTP pour une action, ou pour permettre des verbes HTTP autres que `[AcceptVerbs]` GET, PUT, POST, DELETE, HEAD, OPTIONS, et PATCH, utilisez l’attribut, qui prend une liste de verbes HTTP.</span><span class="sxs-lookup"><span data-stu-id="efa53-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="efa53-173">Routage par nom d’action</span><span class="sxs-lookup"><span data-stu-id="efa53-173">Routing by Action Name</span></span>

<span data-ttu-id="efa53-174">Avec le modèle de routage par défaut, l’API Web utilise le verbe HTTP pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="efa53-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="efa53-175">Cependant, vous pouvez également créer un itinéraire où le nom d’action est inclus dans l’URI:</span><span class="sxs-lookup"><span data-stu-id="efa53-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="efa53-176">Dans ce modèle d’itinéraire, le paramètre *« action »* nomme la méthode d’action sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="efa53-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="efa53-177">Avec ce style de routage, utilisez des attributs pour spécifier les verbes HTTP autorisés.</span><span class="sxs-lookup"><span data-stu-id="efa53-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="efa53-178">Supposons, par exemple, que votre contrôleur ait la méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="efa53-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="efa53-179">Dans ce cas, une demande GET pour "api/produits/détails/1" serait la carte de la `Details` méthode.</span><span class="sxs-lookup"><span data-stu-id="efa53-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="efa53-180">Ce style de routage est similaire à ASP.NET MVC, et peut être approprié pour une API de type RPC.</span><span class="sxs-lookup"><span data-stu-id="efa53-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="efa53-181">Vous pouvez remplacer le nom `[ActionName]` d’action en utilisant l’attribut.</span><span class="sxs-lookup"><span data-stu-id="efa53-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="efa53-182">Dans l’exemple suivant, il ya &quot;deux actions qui cartographient à api / produits / vignette /*id*. L’un prend en charge GET et l’autre prend en charge POST :</span><span class="sxs-lookup"><span data-stu-id="efa53-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="efa53-183">Non-Actions</span><span class="sxs-lookup"><span data-stu-id="efa53-183">Non-Actions</span></span>

<span data-ttu-id="efa53-184">Pour éviter qu’une méthode ne soit `[NonAction]` invoquée comme action, utilisez l’attribut.</span><span class="sxs-lookup"><span data-stu-id="efa53-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="efa53-185">Cela indique au cadre que la méthode n’est pas une action, même si elle correspondrait autrement aux règles d’acheminement.</span><span class="sxs-lookup"><span data-stu-id="efa53-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="efa53-186">Pour aller plus loin</span><span class="sxs-lookup"><span data-stu-id="efa53-186">Further Reading</span></span>

<span data-ttu-id="efa53-187">Ce sujet a fourni une vue de haut niveau de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="efa53-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="efa53-188">Pour plus de détails, voir [Routing et Action Selection](routing-and-action-selection.md), qui décrit exactement comment le cadre correspond à un URI à un itinéraire, sélectionne un contrôleur, puis sélectionne l’action à invoquer.</span><span class="sxs-lookup"><span data-stu-id="efa53-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
