---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Appeler une API Web à partir d’un client .NET (C#)-ASP.NET 4. x
author: MikeWasson
description: Ce didacticiel montre comment appeler une API Web à partir d’une application .NET 4. x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 484d927eeb0ba49f5f00d476f4658ebc081d0a4a
ms.sourcegitcommit: a4c3c7e04e5f53cf8cd334f036d324976b78d154
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84172937"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="b3422-103">Appeler une API Web à partir d’un client .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="b3422-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="b3422-104">par [Mike Wasson](https://github.com/MikeWasson) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b3422-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b3422-105">[Télécharger le projet terminé](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="b3422-105">[Download Completed Project](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="b3422-106">[Instructions de téléchargement](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="b3422-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="b3422-107">Ce didacticiel montre comment appeler une API Web à partir d’une application .NET, à l’aide de [System .net. http. httpclient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="b3422-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="b3422-108">Dans ce didacticiel, une application cliente est écrite et utilise l’API Web suivante :</span><span class="sxs-lookup"><span data-stu-id="b3422-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="b3422-109">Action</span><span class="sxs-lookup"><span data-stu-id="b3422-109">Action</span></span> | <span data-ttu-id="b3422-110">HTTP method</span><span class="sxs-lookup"><span data-stu-id="b3422-110">HTTP method</span></span> | <span data-ttu-id="b3422-111">URI relatif</span><span class="sxs-lookup"><span data-stu-id="b3422-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b3422-112">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="b3422-112">Get a product by ID</span></span> | <span data-ttu-id="b3422-113">GET</span><span class="sxs-lookup"><span data-stu-id="b3422-113">GET</span></span> | <span data-ttu-id="b3422-114">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="b3422-114">/api/products/*id*</span></span> |
| <span data-ttu-id="b3422-115">Créer un produit</span><span class="sxs-lookup"><span data-stu-id="b3422-115">Create a new product</span></span> | <span data-ttu-id="b3422-116">POST</span><span class="sxs-lookup"><span data-stu-id="b3422-116">POST</span></span> | <span data-ttu-id="b3422-117">/api/products</span><span class="sxs-lookup"><span data-stu-id="b3422-117">/api/products</span></span> |
| <span data-ttu-id="b3422-118">Mettre à jour un produit</span><span class="sxs-lookup"><span data-stu-id="b3422-118">Update a product</span></span> | <span data-ttu-id="b3422-119">PUT</span><span class="sxs-lookup"><span data-stu-id="b3422-119">PUT</span></span> | <span data-ttu-id="b3422-120">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="b3422-120">/api/products/*id*</span></span> |
| <span data-ttu-id="b3422-121">Supprimer un produit</span><span class="sxs-lookup"><span data-stu-id="b3422-121">Delete a product</span></span> | <span data-ttu-id="b3422-122">Suppression</span><span class="sxs-lookup"><span data-stu-id="b3422-122">DELETE</span></span> | <span data-ttu-id="b3422-123">*ID* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="b3422-123">/api/products/*id*</span></span> |

<span data-ttu-id="b3422-124">Pour savoir comment implémenter cette API avec API Web ASP.NET, consultez [création d’une API Web qui prend en charge les opérations CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="b3422-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="b3422-125">Pour plus de simplicité, l’application cliente dans ce didacticiel est une application console Windows.</span><span class="sxs-lookup"><span data-stu-id="b3422-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="b3422-126">**Httpclient** est également pris en charge pour les applications Windows Phone et Windows Store.</span><span class="sxs-lookup"><span data-stu-id="b3422-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="b3422-127">Pour plus d’informations, consultez [écriture de code client d’API Web pour plusieurs plateformes à l’aide de bibliothèques portables](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="b3422-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<span data-ttu-id="b3422-128">**Remarque :** Si vous transmettez des URL de base et des URI relatifs comme des valeurs codées en dur, tenez à l’esprit des règles d’utilisation de l' `HttpClient` API.</span><span class="sxs-lookup"><span data-stu-id="b3422-128">**NOTE:** If you pass base URLs and relative URIs as hard-coded values, be mindful of the rules for utilizing the `HttpClient` API.</span></span> <span data-ttu-id="b3422-129">La `HttpClient.BaseAddress` propriété doit être définie sur une adresse avec une barre oblique finale ( `/` ).</span><span class="sxs-lookup"><span data-stu-id="b3422-129">The `HttpClient.BaseAddress` property should be set to an address with a trailing forward slash (`/`).</span></span> <span data-ttu-id="b3422-130">Par exemple, lorsque vous passez des URI de ressource codés en dur à la `HttpClient.GetAsync` méthode, n’incluez pas de barre oblique de début.</span><span class="sxs-lookup"><span data-stu-id="b3422-130">For example, when passing hard-coded resource URIs to the `HttpClient.GetAsync` method, don't include a leading forward slash.</span></span> <span data-ttu-id="b3422-131">Pour obtenir un `Product` par ID :</span><span class="sxs-lookup"><span data-stu-id="b3422-131">To get a `Product` by ID:</span></span>

1. <span data-ttu-id="b3422-132">Définie`client.BaseAddress = new Uri("https://localhost:5001/");`</span><span class="sxs-lookup"><span data-stu-id="b3422-132">Set `client.BaseAddress = new Uri("https://localhost:5001/");`</span></span>
1. <span data-ttu-id="b3422-133">Demandez un `Product` .</span><span class="sxs-lookup"><span data-stu-id="b3422-133">Request a `Product`.</span></span> <span data-ttu-id="b3422-134">Par exemple, `client.GetAsync<Product>("api/products/4");`.</span><span class="sxs-lookup"><span data-stu-id="b3422-134">For example, `client.GetAsync<Product>("api/products/4");`.</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="b3422-135">Création d'application console</span><span class="sxs-lookup"><span data-stu-id="b3422-135">Create the Console Application</span></span>

<span data-ttu-id="b3422-136">Dans Visual Studio, créez une nouvelle application console Windows nommée **HttpClientSample** et collez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3422-136">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="b3422-137">Le code précédent est l’application cliente complète.</span><span class="sxs-lookup"><span data-stu-id="b3422-137">The preceding code is the complete client app.</span></span>

<span data-ttu-id="b3422-138">`RunAsync`s’exécute et se bloque jusqu’à ce qu’il se termine.</span><span class="sxs-lookup"><span data-stu-id="b3422-138">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="b3422-139">La plupart des méthodes **httpclient** sont asynchrones, car elles exécutent des e/s réseau.</span><span class="sxs-lookup"><span data-stu-id="b3422-139">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="b3422-140">Toutes les tâches asynchrones sont effectuées dans `RunAsync` .</span><span class="sxs-lookup"><span data-stu-id="b3422-140">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="b3422-141">Normalement, une application ne bloque pas le thread principal, mais cette application n’autorise aucune interaction.</span><span class="sxs-lookup"><span data-stu-id="b3422-141">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="b3422-142">Installer les bibliothèques clientes de l’API Web</span><span class="sxs-lookup"><span data-stu-id="b3422-142">Install the Web API Client Libraries</span></span>

<span data-ttu-id="b3422-143">Utilisez le gestionnaire de package NuGet pour installer le package de bibliothèques clientes de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="b3422-143">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="b3422-144">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="b3422-144">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="b3422-145">Dans la console du gestionnaire de package (PMC), tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="b3422-145">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="b3422-146">La commande précédente ajoute les packages NuGet suivants au projet :</span><span class="sxs-lookup"><span data-stu-id="b3422-146">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="b3422-147">Microsoft. AspNet. WebApi. client</span><span class="sxs-lookup"><span data-stu-id="b3422-147">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="b3422-148">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="b3422-148">Newtonsoft.Json</span></span>

<span data-ttu-id="b3422-149">Netwonsoft. JSON (également appelé Json.NET) est une infrastructure JSON très performante pour .NET.</span><span class="sxs-lookup"><span data-stu-id="b3422-149">Netwonsoft.Json (also known as Json.NET) is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="b3422-150">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="b3422-150">Add a Model Class</span></span>

<span data-ttu-id="b3422-151">Examiner la classe `Product` :</span><span class="sxs-lookup"><span data-stu-id="b3422-151">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="b3422-152">Cette classe correspond au modèle de données utilisé par l’API Web.</span><span class="sxs-lookup"><span data-stu-id="b3422-152">This class matches the data model used by the web API.</span></span> <span data-ttu-id="b3422-153">Une application peut utiliser **httpclient** pour lire une `Product` instance à partir d’une réponse http.</span><span class="sxs-lookup"><span data-stu-id="b3422-153">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="b3422-154">L’application n’a pas besoin d’écrire de code de désérialisation.</span><span class="sxs-lookup"><span data-stu-id="b3422-154">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="b3422-155">Créer et initialiser HttpClient</span><span class="sxs-lookup"><span data-stu-id="b3422-155">Create and Initialize HttpClient</span></span>

<span data-ttu-id="b3422-156">Examinez la propriété **httpclient** statique :</span><span class="sxs-lookup"><span data-stu-id="b3422-156">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="b3422-157">**Httpclient** est conçu pour être instancié une fois et réutilisé tout au long de la vie d’une application.</span><span class="sxs-lookup"><span data-stu-id="b3422-157">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="b3422-158">Les conditions suivantes peuvent entraîner des erreurs **SocketException** :</span><span class="sxs-lookup"><span data-stu-id="b3422-158">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="b3422-159">Création d’une nouvelle instance **httpclient** par demande.</span><span class="sxs-lookup"><span data-stu-id="b3422-159">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="b3422-160">Serveur soumis à une charge importante.</span><span class="sxs-lookup"><span data-stu-id="b3422-160">Server under heavy load.</span></span>

<span data-ttu-id="b3422-161">La création d’une nouvelle instance **httpclient** par demande peut épuiser les sockets disponibles.</span><span class="sxs-lookup"><span data-stu-id="b3422-161">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="b3422-162">Le code suivant initialise l’instance **httpclient** :</span><span class="sxs-lookup"><span data-stu-id="b3422-162">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="b3422-163">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="b3422-163">The preceding code:</span></span>

* <span data-ttu-id="b3422-164">Définit l’URI de base pour les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3422-164">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="b3422-165">Remplacez le numéro de port par le port utilisé dans l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="b3422-165">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="b3422-166">L’application ne fonctionne pas, sauf si le port de l’application serveur est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b3422-166">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="b3422-167">Définit l’en-tête Accept sur « application/JSON ».</span><span class="sxs-lookup"><span data-stu-id="b3422-167">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="b3422-168">La définition de cet en-tête indique au serveur d’envoyer des données au format JSON.</span><span class="sxs-lookup"><span data-stu-id="b3422-168">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="b3422-169">Envoyer une demande d’extraction pour récupérer une ressource</span><span class="sxs-lookup"><span data-stu-id="b3422-169">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="b3422-170">Le code suivant envoie une requête d’extraction pour un produit :</span><span class="sxs-lookup"><span data-stu-id="b3422-170">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="b3422-171">La méthode **GetAsync** envoie la requête http.</span><span class="sxs-lookup"><span data-stu-id="b3422-171">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="b3422-172">Lorsque la méthode se termine, elle retourne un **HttpResponseMessage** qui contient la réponse http.</span><span class="sxs-lookup"><span data-stu-id="b3422-172">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="b3422-173">Si le code d’État dans la réponse est un code de réussite, le corps de la réponse contient la représentation JSON d’un produit.</span><span class="sxs-lookup"><span data-stu-id="b3422-173">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="b3422-174">Appelez **ReadAsAsync** pour désérialiser la charge utile JSON en une `Product` instance.</span><span class="sxs-lookup"><span data-stu-id="b3422-174">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="b3422-175">La méthode **ReadAsAsync** est asynchrone, car le corps de la réponse peut être arbitrairement grand.</span><span class="sxs-lookup"><span data-stu-id="b3422-175">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="b3422-176">**Httpclient** ne lève pas d’exception lorsque la réponse http contient un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="b3422-176">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="b3422-177">Au lieu de cela, la propriété **IsSuccessStatusCode** a la **valeur false** si l’État est un code d’erreur.</span><span class="sxs-lookup"><span data-stu-id="b3422-177">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="b3422-178">Si vous préférez traiter les codes d’erreur HTTP comme des exceptions, appelez [HttpResponseMessage. EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) sur l’objet Response.</span><span class="sxs-lookup"><span data-stu-id="b3422-178">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="b3422-179">`EnsureSuccessStatusCode`lève une exception si le code d’État se trouve en dehors de la plage 200 &ndash; 299.</span><span class="sxs-lookup"><span data-stu-id="b3422-179">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="b3422-180">Notez que **httpclient** peut lever des exceptions pour d’autres raisons &mdash; , par exemple, si la demande expire.</span><span class="sxs-lookup"><span data-stu-id="b3422-180">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="b3422-181">Formateurs de type média à désérialiser</span><span class="sxs-lookup"><span data-stu-id="b3422-181">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="b3422-182">Quand **ReadAsAsync** est appelé sans paramètre, il utilise un jeu de *formateurs de médias* par défaut pour lire le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="b3422-182">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="b3422-183">Les formateurs par défaut prennent en charge les données JSON, XML et de type URL de formulaire.</span><span class="sxs-lookup"><span data-stu-id="b3422-183">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="b3422-184">Au lieu d’utiliser les formateurs par défaut, vous pouvez fournir une liste de formateurs à la méthode **ReadAsAsync** .</span><span class="sxs-lookup"><span data-stu-id="b3422-184">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="b3422-185">L’utilisation d’une liste de formateurs est utile si vous avez un formateur de type de média personnalisé :</span><span class="sxs-lookup"><span data-stu-id="b3422-185">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="b3422-186">Pour plus d’informations, consultez [formateurs de médias dans API Web ASP.NET 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="b3422-186">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="b3422-187">Envoi d’une demande de publication pour créer une ressource</span><span class="sxs-lookup"><span data-stu-id="b3422-187">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="b3422-188">Le code suivant envoie une demande de publication contenant une `Product` instance au format JSON :</span><span class="sxs-lookup"><span data-stu-id="b3422-188">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="b3422-189">Méthode **PostAsJsonAsync** :</span><span class="sxs-lookup"><span data-stu-id="b3422-189">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="b3422-190">Sérialise un objet au format JSON.</span><span class="sxs-lookup"><span data-stu-id="b3422-190">Serializes an object to JSON.</span></span>
* <span data-ttu-id="b3422-191">Envoie la charge utile JSON dans une demande de publication.</span><span class="sxs-lookup"><span data-stu-id="b3422-191">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="b3422-192">Si la demande est réussie :</span><span class="sxs-lookup"><span data-stu-id="b3422-192">If the request succeeds:</span></span>

* <span data-ttu-id="b3422-193">Elle doit retourner une réponse 201 (créée).</span><span class="sxs-lookup"><span data-stu-id="b3422-193">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="b3422-194">La réponse doit inclure l’URL des ressources créées dans l’en-tête Location.</span><span class="sxs-lookup"><span data-stu-id="b3422-194">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="b3422-195">Envoi d’une demande PUT pour mettre à jour une ressource</span><span class="sxs-lookup"><span data-stu-id="b3422-195">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="b3422-196">Le code suivant envoie une demande PUT pour mettre à jour un produit :</span><span class="sxs-lookup"><span data-stu-id="b3422-196">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="b3422-197">La méthode **PutAsJsonAsync** fonctionne comme **PostAsJsonAsync**, à ceci près qu’elle envoie une demande put au lieu de la publication.</span><span class="sxs-lookup"><span data-stu-id="b3422-197">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="b3422-198">Envoi d’une demande DELETE pour supprimer une ressource</span><span class="sxs-lookup"><span data-stu-id="b3422-198">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="b3422-199">Le code suivant envoie une demande DELETE pour supprimer un produit :</span><span class="sxs-lookup"><span data-stu-id="b3422-199">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="b3422-200">Comme par exemple, une demande DELETE n’a pas de corps de demande.</span><span class="sxs-lookup"><span data-stu-id="b3422-200">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="b3422-201">Vous n’avez pas besoin de spécifier le format JSON ou XML avec DELETE.</span><span class="sxs-lookup"><span data-stu-id="b3422-201">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="b3422-202">Tester l’exemple</span><span class="sxs-lookup"><span data-stu-id="b3422-202">Test the sample</span></span>

<span data-ttu-id="b3422-203">Pour tester l’application cliente :</span><span class="sxs-lookup"><span data-stu-id="b3422-203">To test the client app:</span></span>

1. <span data-ttu-id="b3422-204">[Téléchargez](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) et exécutez l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="b3422-204">[Download](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="b3422-205">[Instructions de téléchargement](/aspnet/core/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="b3422-205">[Download instructions](/aspnet/core/#how-to-download-a-sample).</span></span> <span data-ttu-id="b3422-206">Vérifiez que l’application serveur fonctionne.</span><span class="sxs-lookup"><span data-stu-id="b3422-206">Verify the server app is working.</span></span> <span data-ttu-id="b3422-207">Par exemple, `http://localhost:64195/api/products` doit retourner une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="b3422-207">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="b3422-208">Définissez l’URI de base pour les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3422-208">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="b3422-209">Remplacez le numéro de port par le port utilisé dans l’application serveur.</span><span class="sxs-lookup"><span data-stu-id="b3422-209">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="b3422-210">Exécutez l’application cliente.</span><span class="sxs-lookup"><span data-stu-id="b3422-210">Run the client app.</span></span> <span data-ttu-id="b3422-211">La sortie suivante est produite :</span><span class="sxs-lookup"><span data-stu-id="b3422-211">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
