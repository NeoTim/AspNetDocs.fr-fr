---
title: Mettre en forme les données des réponses dans l’API web ASP.NET Core
author: ardalis
description: Découvrez comment mettre en forme les données des réponses dans l’API web ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
uid: web-api/advanced/formatting
ms.openlocfilehash: 819bf1b49b56e953a9a4398e82866ba0b01ab4db
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049786"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="5c1c7-103">Mettre en forme les données des réponses dans l’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c1c7-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="5c1c7-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5c1c7-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5c1c7-105">ASP.NET Core MVC offre une prise en charge intégrée de la mise en forme des données des réponses, selon un format fixe ou en réponse à des spécifications du client.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="5c1c7-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5c1c7-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="5c1c7-107">Résultats d’une action spécifique à un format</span><span class="sxs-lookup"><span data-stu-id="5c1c7-107">Format-Specific Action Results</span></span>

<span data-ttu-id="5c1c7-108">Certains types de résultats d’action sont spécifiques à un format particulier, comme `JsonResult` et `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="5c1c7-109">Les actions peuvent retourner des résultats spécifiques qui sont toujours mis en forme d’une manière particulière.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="5c1c7-110">Par exemple, retourner un `JsonResult` retourne des données au format JSON, indépendamment des préférences du client.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="5c1c7-111">De même, retourner un `ContentResult` retourne des données de chaîne au format texte brut (tout comme retourner une chaîne).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="5c1c7-112">Une action ne doit pas nécessairement retourner un type particulier ; MVC prend en charge n’importe quelle valeur de retour d’un objet.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="5c1c7-113">Si une action retourne une implémentation de `IActionResult` et que le contrôleur hérite de `Controller`, les développeurs disposent de nombreuses méthodes helper correspondant à de nombreux choix différents.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="5c1c7-114">Les résultats provenant d’actions qui retournent des objets qui ne sont pas des types `IActionResult` sont sérialisés en utilisant l’implémentation appropriée de `IOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="5c1c7-115">Pour retourner des données dans un format spécifique depuis un contrôleur qui hérite de la classe de base `Controller`, utilisez la méthode helper intégrée `Json` pour retourner du JSON et `Content` pour du texte brut.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="5c1c7-116">Votre méthode d’action doit retourner le type de résultat spécifique (par exemple `JsonResult`) ou `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="5c1c7-117">Retour de données au format JSON :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="5c1c7-118">Exemple de réponse de cette action :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-118">Sample response from this action:</span></span>

![Onglet Réseau des Outils de développement dans Microsoft Edge montrant que le type de contenu de la réponse est application/json](formatting/_static/json-response.png)

<span data-ttu-id="5c1c7-120">Notez que le type de contenu de la réponse est `application/json`, qui apparaît à la fois dans la liste des requêtes du réseau et dans la section En-têtes de réponse.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="5c1c7-121">Notez également la liste des options présentées par le navigateur (dans ce cas, Microsoft Edge) dans l’en-tête Accept de la section des en-têtes de la requête.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="5c1c7-122">La technique actuelle ignore cet en-tête ; sa prise en compte est présentée ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="5c1c7-123">Pour retourner des données mises en forme en texte brut, utilisez `ContentResult` et le helper `Content` :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="5c1c7-124">Une réponse de cette action :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-124">A response from this action:</span></span>

![Onglet Réseau des Outils de développement dans Microsoft Edge montrant que le type de contenu de la réponse est text/plain](formatting/_static/text-response.png)

<span data-ttu-id="5c1c7-126">Notez que dans ce cas, le `Content-Type` retourné est `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="5c1c7-127">Vous pouvez également obtenir ce comportement en utilisant simplement un type de réponse chaîne :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="5c1c7-128">Pour les actions plus complexes avec plusieurs types ou options (par exemple différents codes d’état HTTP en fonction du résultat des opérations effectuées), préférez le type de retour `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="5c1c7-129">Négociation de contenu</span><span class="sxs-lookup"><span data-stu-id="5c1c7-129">Content Negotiation</span></span>

<span data-ttu-id="5c1c7-130">La négociation de contenu (*conneg*, abréviation de « Content negotiation ») se produit quand le client spécifie un [en-tête Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="5c1c7-131">Le format par défaut utilisé par ASP.NET Core MVC est JSON.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="5c1c7-132">La négociation de contenu est implémentée par `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="5c1c7-133">Elle est également intégrée dans les résultats d’une action spécifique au code d’état retournés depuis les méthodes helper (qui sont toutes basées sur `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="5c1c7-134">Vous pouvez aussi retourner un type de modèle (une classe que vous avez définie comme étant le type de transfert de vos données), que le framework encapsule automatiquement dans un `ObjectResult` pour vous.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="5c1c7-135">La méthode d’action suivante utilise les méthodes helper `Ok` et `NotFound` :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="5c1c7-136">Une réponse au format JSON est retournée sauf si un autre format a été demandé et que le serveur peut retourner ce format demandé.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="5c1c7-137">Vous pouvez utiliser un outil comme [Fiddler](http://www.telerik.com/fiddler) pour créer une requête qui inclut un en-tête Accept et pour spécifier un autre format.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="5c1c7-138">Dans ce cas, si le serveur a un *formateur* qui peut produire une réponse dans le format demandé, le résultat est retourné dans le format préféré du client.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Console Fiddler montrant une requête GET créée manuellement une valeur d’en-tête Accept application/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="5c1c7-140">Dans la capture d’écran ci-dessus, Fiddler Composer a été utilisé pour générer une requête, en spécifiant `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="5c1c7-141">Par défaut, ASP.NET Core MVC prend en charge seulement JSON : même si un autre format est spécifié, le résultat retourné est donc toujours au format JSON.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="5c1c7-142">Vous verrez comment ajouter d’autres formateurs dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="5c1c7-143">Les actions du contrôleur peuvent retourner des objets OCT (objets CLR traditionnels), auquel cas ASP.NET Core MVC crée automatiquement pour vous un `ObjectResult` qui encapsule l’objet.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="5c1c7-144">Le client reçoit l’objet sérialisé mis en forme (le format JSON est le format par défaut ; vous pouvez configurer un format XML ou d’autres formats).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="5c1c7-145">Si l’objet retourné est `null`, le framework retourne une réponse `204 No Content`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="5c1c7-146">Retour d’un type d’objet :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="5c1c7-147">Dans l’exemple, une requête d’alias d’auteur valide reçoit une réponse 200 OK avec les données de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="5c1c7-148">Une requête d’alias non valide reçoit une réponse 204 No Content.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="5c1c7-149">Les captures d’écran ci-dessous montrent la réponse aux formats XML et JSON.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="5c1c7-150">Processus de négociation de contenu</span><span class="sxs-lookup"><span data-stu-id="5c1c7-150">Content Negotiation Process</span></span>

<span data-ttu-id="5c1c7-151">La *négociation* de contenu intervient seulement si un en-tête `Accept` apparaît dans la requête.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="5c1c7-152">Quand une requête contient un en-tête Accept, le framework énumère les types de médias dans l’en-tête Accept par ordre de préférence, et tente de trouver un formateur capable de produire une réponse dans un des formats spécifiés par l’en-tête Accept.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="5c1c7-153">Si aucun formateur pouvant satisfaire la requête du client n’est trouvé, le framework tente de trouver le premier formateur capable de produire une réponse (sauf si le développeur a configuré l’option sur `MvcOptions` pour retourner à la place « 406 Not Acceptable »).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="5c1c7-154">Si la requête spécifie XML, mais que le formateur XML n’a pas été configuré, le formateur JSON est utilisé.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="5c1c7-155">Plus généralement, si aucun formateur pouvant fournir le format demandé n’est configuré, le premier formateur qui peut mettre en forme l’objet est utilisé.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="5c1c7-156">Si aucun en-tête n’est spécifié, le premier formateur capable de gérer l’objet à retourner est utilisé pour sérialiser la réponse.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="5c1c7-157">Dans ce cas, aucune négociation n’est effectuée : le serveur détermine le format à utiliser.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="5c1c7-158">Si l’en-tête Accept contient `*/*`, l’en-tête est ignoré, sauf si `RespectBrowserAcceptHeader` a la valeur true sur `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="5c1c7-159">Navigateurs et négociation de contenu</span><span class="sxs-lookup"><span data-stu-id="5c1c7-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="5c1c7-160">Contrairement aux clients d’API classiques, les navigateurs web ont tendance à fournir des en-têtes `Accept` dans un grand nombre de formats, incluant des caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="5c1c7-161">Par défaut, quand le framework détecte que la requête provient d’un navigateur, il ignore l’en-tête `Accept` et retourne à la place le contenu au format par défaut configuré pour l’application (JSON, sauf si un autre format est configuré).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="5c1c7-162">Ceci fournit une expérience plus cohérente lors de l’utilisation de différents navigateurs pour consommer des API.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="5c1c7-163">Si vous préférez que votre application prenne en compte les en-têtes Accept du navigateur, vous pouvez configurer ce comportement dans le cadre de la configuration du modèle MVC en définissant `RespectBrowserAcceptHeader` sur `true` dans la méthode `ConfigureServices`, dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="5c1c7-164">Configuration des formateurs</span><span class="sxs-lookup"><span data-stu-id="5c1c7-164">Configuring Formatters</span></span>

<span data-ttu-id="5c1c7-165">Si votre application doit prendre en charge des formats supplémentaires en plus du format par défaut JSON, vous pouvez ajouter des packages NuGet et configurer le modèle MVC pour les prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="5c1c7-166">Il existe des formateurs distincts pour les entrées et pour les sorties.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="5c1c7-167">Les formateurs d’entrée sont utilisés par la [liaison de modèle](xref:mvc/models/model-binding) ; les formateurs de sortie sont utilisés pour mettre en forme les réponses.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="5c1c7-168">Vous pouvez également configurer des [formateurs personnalisés](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="5c1c7-169">Ajout de la prise en charge du format XML</span><span class="sxs-lookup"><span data-stu-id="5c1c7-169">Adding XML Format Support</span></span>

<span data-ttu-id="5c1c7-170">Pour ajouter la prise en charge de la mise en forme XML, vous devez installer le package NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="5c1c7-171">Ajoutez XmlSerializerFormatters à la configuration du modèle MVC à *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="5c1c7-172">Vous pouvez aussi ajouter simplement le formateur de sortie :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="5c1c7-173">Ces deux approches sérialisent les résultats avec `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="5c1c7-174">Si vous préférez, vous pouvez utiliser `System.Runtime.Serialization.DataContractSerializer` en ajoutant son formateur associé :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="5c1c7-175">Une fois que vous avez ajouté la prise en charge de la mise en forme XML, vos méthodes de contrôleur doivent retourner le format approprié en fonction de l’en-tête `Accept` de la requête, comme cet exemple Fiddler le montre :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Console Fiddler : L’onglet Raw pour la demande montre que la valeur d’en-tête Accept est application/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="5c1c7-178">Vous pouvez voir dans l’onglet Inspectors que la requête GET brute a été faite avec un en-tête `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="5c1c7-179">Le volet de réponse montre l’en-tête `Content-Type: application/xml` et que l’objet `Author` a été sérialisé en XML.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="5c1c7-180">Utilisez l’onglet Composer pour modifier la requête en spécifiant `application/json` dans l’en-tête `Accept`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="5c1c7-181">Exécutez la requête pour constater que la réponse est au format JSON :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Console Fiddler : L’onglet Raw pour la demande montre que la valeur d’en-tête Accept est application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="5c1c7-184">Dans cette capture d’écran, vous pouvez voir que la requête définit un en-tête `Accept: application/json` et que la réponse spécifie de même dans son `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="5c1c7-185">L’objet `Author` est montré dans le corps de la réponse, au format JSON.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="5c1c7-186">Forçer un format particulier</span><span class="sxs-lookup"><span data-stu-id="5c1c7-186">Forcing a Particular Format</span></span>

<span data-ttu-id="5c1c7-187">Si vous voulez limiter les formats de réponse pour une action spécifique, vous pouvez appliquer le filtre `[Produces]`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="5c1c7-188">Le filtre `[Produces]` spécifie les formats de réponse pour une action (ou un contrôleur) spécifique.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="5c1c7-189">Comme la plupart des [filtres](xref:mvc/controllers/filters), celui-ci peut être appliqué à l’action, au contrôleur ou à l’étendue globale.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-189">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="5c1c7-190">Le filtre `[Produces]` force toutes les actions dans `AuthorsController` à retourner des réponses au format JSON, même si d’autres formateurs ont été configurés pour l’application et que le client a fourni un en-tête `Accept` demandant un autre format disponible.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="5c1c7-191">Pour plus d’informations, notamment comment appliquer des filtres de façon globale, consultez [Filtres](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-191">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="5c1c7-192">Formateurs pour des cas spéciaux</span><span class="sxs-lookup"><span data-stu-id="5c1c7-192">Special Case Formatters</span></span>

<span data-ttu-id="5c1c7-193">Certains cas spéciaux sont implémentés avec des formateurs intégrés.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="5c1c7-194">Par défaut, les types de retour `string` sont formatés en *text/plain* (*text/html* si c’est demandé via l’en-tête `Accept`).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="5c1c7-195">Vous pouvez éviter ce comportement en supprimant `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="5c1c7-196">Vous supprimez des formateurs dans la méthode `Configure` de *Startup.cs* (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="5c1c7-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="5c1c7-197">Les actions qui ont un type de retour d’objet de modèle retournent une réponse « 204 No Content » si `null` est retourné.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="5c1c7-198">Vous pouvez éviter ce comportement en supprimant `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="5c1c7-199">Le code suivant supprime `TextOutputFormatter` et `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="5c1c7-200">Par exemple, sans `TextOutputFormatter`, les types de retour `string` retournent « 406 Not Acceptable ».</span><span class="sxs-lookup"><span data-stu-id="5c1c7-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="5c1c7-201">Notez que s’il existe un formateur XML, il met en forme les types de retour `string` si `TextOutputFormatter` est supprimé.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="5c1c7-202">Sans `HttpNoContentOutputFormatter`, les objets null sont mis en forme avec le formateur configuré.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="5c1c7-203">Par exemple, le formateur JSON retourne simplement une réponse avec un corps `null`, tandis que le formateur XML retourne un élément XML vide avec l’attribut `xsi:nil="true"`.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="5c1c7-204">Mappages d’URL de format de réponse</span><span class="sxs-lookup"><span data-stu-id="5c1c7-204">Response Format URL Mappings</span></span>

<span data-ttu-id="5c1c7-205">Les clients peuvent demander un format particulier dans l’URL, comme dans la chaîne de requête ou une partie du chemin, ou en utilisant une extension de fichier spécifique à un format, comme .xml ou .json.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="5c1c7-206">Le mappage du chemin de la requête doit être spécifié dans la route utilisée par l’API.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="5c1c7-207">Exemple :</span><span class="sxs-lookup"><span data-stu-id="5c1c7-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="5c1c7-208">Cette route permet de spécifier le format demandé sous la forme d’une extension de fichier facultative.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="5c1c7-209">L’attribut `[FormatFilter]` vérifie l’existence de la valeur du format dans `RouteData` et mappe le format de la réponse au formateur approprié lors de la création de la réponse.</span><span class="sxs-lookup"><span data-stu-id="5c1c7-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>


|           <span data-ttu-id="5c1c7-210">Route</span><span class="sxs-lookup"><span data-stu-id="5c1c7-210">Route</span></span>            |             <span data-ttu-id="5c1c7-211">Formateur</span><span class="sxs-lookup"><span data-stu-id="5c1c7-211">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="5c1c7-212">Le formateur de sortie par défaut</span><span class="sxs-lookup"><span data-stu-id="5c1c7-212">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="5c1c7-213">Le formateur JSON (s’il est configuré)</span><span class="sxs-lookup"><span data-stu-id="5c1c7-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="5c1c7-214">Le formateur XML (s’il est configuré)</span><span class="sxs-lookup"><span data-stu-id="5c1c7-214">The XML formatter (if configured)</span></span>  |

